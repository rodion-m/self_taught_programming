# Еще раз про асинхронную машину состояний и где именно там аллокации

Несмотря на то, что про `async/await` уже было сказано много слов и записано множество докладов, тем не менее в своей практике преподавания и наставничества я часто сталкиваюсь с недопониманием устройства `async/await` даже у разработчиков уровня Middle+.

Как известно, при компиляции асинхронного метода компилятор преобразует код, разбивая его на отдельные шаги. Потом, во время выполнения каждый шаг прерывается асинхронной операцией. Когда она завершается, надо точно понимать, куда вернуть управление — в какой конкретно шаг. Поэтому все шаги нумеруются и компилятор очень строго следит за тем откуда куда можно перейти. В computer science такое решение называется *машиной состояний*. Еще, по-русски её называют *конечный автомат*. Далее, для краткости, я буду использовать сокращение SM (*state machine*).

Итак, в данной статье мы подробно рассмотрим *машину состояний*, сгенерированную компилятором C# из асинхронного метода для понимания принципа работы асинхронности в C#.

## "Высокуровневый" C#

Сначала рассмотрим пример простого кода на обычном ("высокуровневом") C#. 
```csharp
using System;
using System.Threading.Tasks;
using System.IO;

public class Program {
    private string _fileContent;
    
    public async Task Main() {
        await Task.Delay(100);
        
        int delay = int.Parse(Console.ReadLine());
        await Task.Delay(delay);
        
        _fileContent = await File.ReadAllTextAsync("file1");
        
        await Task.Delay(delay);
    }
}
```
Код сначала ожидает 100 мс, затем считывает из консоли сколько еще ожидать, еще ожидает, считыват данные из файла и еще ожидает. Логики в последовательности этих вызовов искать не стоит, для нас здесь главное, что это просто понятные асинхронные вызовы.

## "Низкоуровневый" C#

Далее следует код, который генерирует компилятор из "высокоуровневого" (обычного) C#.
Сразу скажу, что оригинальный код, сгенерированный компилятором, [выглядит](https://sharplab.io/#v2:CYLg1APgAgTAjAWAFBQAwAIpwKwG5lqZwB0AkgPL5IEDMmMRA7OgN7LofoAOATgJYA3AIYAXAKZEMAfQBmfADZiAwgHsAduI1VO6dpyh0oADkwA2dAFkhfNQAoAlKz06OUAJxniAETHyhAT1s4VFR7bRcOZxcbEXRgXwD0AF50GOIABSEeAGcxW1U1bJVFYgAlMSFgABkbPPswqJ13Tx8/QPi2hqQIyO6e2QVldU1YlOaAMUGyiuAAQXl5ABUxAA8RWez/NQBjWwAiOUU4Pa6e3T6I5qhTbwT2u9POAF9kJ6A===) так, как будто разработчики кодо-генератора делали все для того, чтобы человеку было непонятно ничего. Тем не менее, кому интересно, оригинальный код можно [посмотреть на sharplab.io](https://sharplab.io/#v2:CYLg1APgAgTAjAWAFBQAwAIpwKwG5lqZwB0AkgPL5IEDMmMRA7OgN7LofoAOATgJYA3AIYAXAKZEMAfQBmfADZiAwgHsAduI1VO6dpyh0oADkwA2dAFkhfNQAoAlKz06OUAJxniAETHyhAT1s4VFR7bRcOZxcbEXRgXwD0AF50GOIABSEeAGcxW1U1bJVFYgAlMSFgABkbPPswqJ13Tx8/QPi2hqQIyO6e2QVldU1YlOaAMUGyiuAAQXl5ABUxAA8RWez/NQBjWwAiOUU4Pa6e3T6I5qhTbwT2u9POAF9kJ6A===).

В общем, я [попросил](https://chat.openai.com/share/ea94242e-4a09-4f86-9ef5-d881ade02b73) нейросеть провести небольшой рефакторинг, чтобы сделать код более читаемым, а также сопроводил значимые и неочевидные места подробными комментариями. Вот что получилось (содержимое класса `Program`):
```csharp
// Машина состояний (SM)
private sealed class AsyncStateMachine : IAsyncStateMachine
{
    // Определение состояний для машины состояний
    public enum State
    {
        NotStarted,               // Машина состояний не запущена - начальное состояние
        WaitingAfterInitialDelay, // Ожидание после начальной задержки
        WaitingForFileRead,       // Ожидание чтения файла
        WaitingAfterFinalDelay,   // Ожидание после последней задержки
        Finished                  // Завершено
    }

    public State CurrentState; // Текущее состояние машины состояний

    public AsyncTaskMethodBuilder Builder; // Строитель задачи асинхронного метода

    public Program Instance; // Экземпляр программы (оригинального класса)

    private int DelayDuration; // Длительность задержки (переменная delay стала полем машины состояний)

    private string FileContentTemp; // Временное хранение содержимого файла

    private TaskAwaiter DelayAwaiter; // Ожидатель задержки

    private TaskAwaiter<string> ReadFileAwaiter; // Ожидатель чтения файла

    private void MoveNext()
    {
        try
        {
            switch (CurrentState)
            {
                case State.NotStarted:
                    // Запуск начальной задержки
                    DelayAwaiter = Task.Delay(100).GetAwaiter();
                    if (DelayAwaiter.IsCompleted)
                    {
                        /* 
                            В случае если таска сразу после запуска завершилась, произойдет переход к выполнению следующего этапа машины состояний (WaitingAfterInitialDelay)
                            Такое бывает, например, когда в методе с модификатором async нет асинхронных вызовов, либо если мы эвэйтим уже завершенную таску.
                        */
                        goto case State.WaitingAfterInitialDelay;
                    }
                    // Конкретно в этом кейсе, исполнение не зайдет в if, который выше, а выполнит две нижние строки
                    CurrentState = State.WaitingAfterInitialDelay;
                    Builder.AwaitUnsafeOnCompleted(ref DelayAwaiter, ref this);
                    /* 
                        AwaitUnsafeOnCompleted запланирует, что указанная машина состояний (ref this) будет продвинута вперед после завершения работы указанного awaiter'а (будет вызван метод MoveNext).
                        По смыслу это похоже на ContinueWith.
                        [ссылка на исходник под кодом] *
                    */
                    break;

                case State.WaitingAfterInitialDelay:
                    DelayAwaiter.GetResult();
                    /*
                        В случае если в асинхронном методе случился эксепшн, тогда он будет выброшен при вызове GetResult и мы сразу попадем в блок catch.
                    */

                    DelayDuration = int.Parse(Console.ReadLine());
                    DelayAwaiter = Task.Delay(DelayDuration).GetAwaiter();
                    if (DelayAwaiter.IsCompleted)
                    {
                        goto case State.WaitingForFileRead;
                    }
                    CurrentState = State.WaitingForFileRead;
                    Builder.AwaitUnsafeOnCompleted(ref DelayAwaiter, ref this);
                    break;

                case State.WaitingForFileRead:
                    /*
                        Важно, что если выполнение идет по реально асинхронному сценарию (т. е. мы попадаем сюда не из goto case), и используется контекст синхронизации по умолчанию, либо он не задан (что по умолчанию в запросах ASP.NET Core, например), то метод MoveNext() будет вызван из какого-то потока пула потоков. То есть, разные состояния SM могут быть запущены разными потоками.
                        Обычно, нам, программистам, эта особенность не мешает. Но есть редкие кейсы, где это может быть важно - как, например, кейс в одной из задачек на самопроверку ниже в статье.
                    */
                    DelayAwaiter.GetResult();
                    ReadFileAwaiter = File.ReadAllTextAsync("file1").GetAwaiter();
                    if (ReadFileAwaiter.IsCompleted)
                    {
                        goto case State.WaitingAfterFinalDelay;
                    }
                    CurrentState = State.WaitingAfterFinalDelay;
                    Builder.AwaitUnsafeOnCompleted(ref ReadFileAwaiter, ref this);
                    break;

                case State.WaitingAfterFinalDelay:
                    // Завершение чтения файла и установка результата
                    FileContentTemp = ReadFileAwaiter.GetResult();
                    Instance._fileContent = FileContentTemp;
                    FileContentTemp = null;
                    DelayAwaiter = Task.Delay(DelayDuration).GetAwaiter();
                    if (DelayAwaiter.IsCompleted)
                    {
                        CurrentState = State.Finished;
                        return;
                    }
                    CurrentState = State.Finished;
                    Builder.AwaitUnsafeOnCompleted(ref DelayAwaiter, ref this);
                    break;
            }
        }
        catch (Exception exception)
        {
            CurrentState = State.Finished;
            Builder.SetException(exception);
        }
    }
}

private string _fileContent; // Содержимое файла

[AsyncStateMachine(typeof(AsyncStateMachine))]
public Task Main()
{
    AsyncStateMachine stateMachine = new AsyncStateMachine();
    stateMachine.Builder = AsyncTaskMethodBuilder.Create();
    stateMachine.Instance = this;
    stateMachine.CurrentState = AsyncStateMachine.State.NotStarted;
    stateMachine.Builder.Start(ref stateMachine);
    /* 
        Первый вызов MoveNext происходит прямо в stateMachine.Builder.Start. Т. е. первое состояние нашей SM фактически выполняется синхронно (и далее до первого реального асинхронного вызова).
        Исходник **
    */
    return stateMachine.Builder.Task;
}
```
\* Исходный код метода `AsyncTaskMethodBuilder.AwaitUnsafeOnCompleted` доступен [по ссылке](https://github.com/dotnet/runtime/blob/v8.0.1/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncTaskMethodBuilder.cs#L63)
\*\* Исходный код этого метода `AsyncStateMachine.Builder.Start` доступен [по ссылке](https://github.com/dotnet/runtime/blob/v8.0.1/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncMethodBuilderCore.cs#L38).

Из кода выше видно, что, фактически, на каждое использование ключевого слова `await` компилятор генерирует дополнительное состояние для машины состояний (SM). Кроме того, важно отметить, что саму машину состояний компилятор сгенерирует в случае если в определении типа метода используется модификатор `async`.

Кстати, этот код не запустится, потому что в нем нет некоторых вспомогательных методов, которые генерирует компилятор. Но он позволяет понять, как работает асинхронность в C#.

## Разбираемся с аллокациями

Интересно, что в Debug режиме `AsyncStateMachine` для тасок представлен в виде класса, а в Release — в виде структуры (`struct`). Но хоть это и структура, если выполнение пойдет действительно по асинхронному сценарию, под капотом в Runtime все-таки произойдет аллокация для `AsyncStateMachine`.

Когда выполнение идет по асинхронному сценарию (вызов `DelayAwaiter.IsCompleted` возвращает `false`), CLR'у машину состояний необходимо переместить из стека в управляемую кучу, для этого она упаковывается (boxing) в [AsyncStateMachineBox<TStateMachine>](https://github.com/dotnet/runtime/blob/v8.0.1/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncTaskMethodBuilderT.cs#L275) рантаймом. 
Для `Task` это происходит внутри [AsyncTaskMethodBuilder<TResult>.GetStateMachineBox<TStateMachine>](https://github.com/dotnet/runtime/blob/v8.0.1/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncTaskMethodBuilderT.cs#L220). 
Для `ValueTask` это происходит внутри цепочки ([AsyncValueTaskMethodBuilder.AwaitUnsafeOnCompleted](https://github.com/dotnet/runtime/blob/v8.0.1/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncValueTaskMethodBuilder.cs#L98) -> [AsyncTaskMethodBuilder<TResult>.AwaitUnsafeOnCompleted](https://github.com/dotnet/runtime/blob/v8.0.1/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncTaskMethodBuilderT.cs#L92) -> [AsyncTaskMethodBuilder<TResult>.GetStateMachineBox<TStateMachine>](https://github.com/dotnet/runtime/blob/v8.0.1/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncTaskMethodBuilderT.cs#L220)). 

## Пулинг 

В CLR предусмотрена возможность [пулинга*](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/PoolingAsyncValueTaskMethodBuilderT.cs#L212) `AsyncStateMachineBox` для минимизации аллокаций (метод [StateMachineBox<TStateMachine> RentFromCache()](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/PoolingAsyncValueTaskMethodBuilderT.cs#L299)).

\* пулинг (pooling) в данном контексте — это техника повторного использования данных для экономии места в куче приложения и ресурсов **GC**. В случае пулинга машины состояний, CLR сможет повторно ее использовать для `ValueTask`, чтобы сэкономить на аллокациях в куче. Хотя, даже Стивен Тауб [сомневается](https://devblogs.microsoft.com/dotnet/async-valuetask-pooling-in-net-5) в реальной эффективности такого подхода.

Отдельно отмечу, что использование `ValueTask` часто **не отменяет** аллокаций в случае асинхронного сценария.

Ну и финальное замечание — если присмотрется внимательно что стало с переменной `delay`, то мы увидим, что она была захвачена и перенесена в поле машины состояний (`DelayDuration` в очищенном коде и `<delay>5__2` [в коде от компилятора](https://sharplab.io/#v2:D4AQTAjAsAUCAMACEECsBuWDkQHQEkB5TGLAZmTBwHZEBvWRJxABwCcBLANwEMAXAKY4kAfQBmHADYCAwgHsAdoKUlmiRsxAUQADmQA2RAFkeHBQAoAlPQ1qmIAJwHcAEQGSeAT3MR48S6p2TLZ2ZnyIACbuXogAvIhhuAAKPGwAzgLm8gppctK4AEoCPBEAMmaZlgEhao7Obh7eUY3VMEHBbe3iUrKKyuHxdQBiPYXFEQCCkpIAKgIAHnwTaZ4KAMbmAEQS0hCbre3qnUF1IPqu0U2XB8wAvrC3QA==)). Соответственно, важно понимать, что если выполнение идет по асинхронному сценарию (что довольно часто), value type переменные забоксятся вместе с машиной состояний и будут перемещены в управляемую кучу (heap) — поэтому `Span`ы запрещено использовать в методах с модификатором `async`.

## Что еще почитать по теме

Например, [целый гайд от Стивена Тауба "How Async/Await Really Works in C#"](https://devblogs.microsoft.com/dotnet/how-async-await-really-works/) ("Как на самом деле работает Async/Await в C#"). Переводы также [доступны](https://habr.com/ru/articles/732738/) на хабре.

Или статью [Prefer ValueTask to Task, always; and don't await twice](https://blog.marcgravell.com/2019/08/prefer-valuetask-to-task-always-and.html), которая описывает не только особенности `ValueTask`, но и то, как реализовать даже асинхронную логику через `IValueTaskSource` или `ManualResetValueTaskSourceCore`, минимизируя кол-во выделений памяти в куче.

## Упражнения для самопроверки

1. Что выведет программа?
```csharp
void Main()
{
    RunAsync(); //"fire-and-forget"
    Console.WriteLine("Main");
    Thread.Sleep(1500);
}

async Task RunAsync()
{
    Console.WriteLine("RunAsync 1");
    await Task.Delay(1000);
    Console.WriteLine("RunAsync 2");
}
```
Ответ:
Т. к. первое состояние точно выполнится синхронно (его выполняет вызывающий поток), то сразу при вызове `RunAsync` в консоль будет выведено "RunAsync 1", затем после запуска `Task.Delay(1000)` вызывающий поток сразу продолжит выполнение и перейдет к выполнению `Console.WriteLine("Main")`, после чего он переключится в [состояние](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.threadstate?view=net-8.0) `WaitSleepJoin` ([`Wait:ExecutionDelay`](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.threadwaitreason?view=net-8.0)) и уснет, затем примерно через секунду другой поток (thread pool worker) напишет в консоль "RunAsync 2". В итоге, получим вывод:

```
RunAsync 1
Main
RunAsync 2
```

2. Сколько состояний SM будет сгенерировано для такого кода?
```csharp
async Task Delay1()
{
    await Task.Delay(1);
}
```
Ответ:
* первое состояние - это этап, на котором запускается таска `Task.Delay(1)`
* второе состояние - продолжение (continuation) с вызовом `GetResult`, которое выполнится после завершения ранее запущенной таски.
* Итого: 2 состояния.

Кстати, технически, поле state там может принимать еще одно значение `-2`, которое устанавливается после завершения всех операций, но фактически оно эквивалетно начальному состоянию.

3. А для такого?
```csharp
async Task MultiDelay()
{
    var task = Task.Delay(1);
    await task;
    await task;
    await task;
}
```
Ответ:
* 1 до всех эвейтов на запуск `Task.Delay`
* 1 continiation с вызовом `GetResult` после первого await'a 
* затем 2 доп. состояния, делающих фактически синхронное потребление уже завершенной таски (они выполняются по цепочке через `goto`)
* Итого: 4 состояния.

4. И финальная задача. Сколько состояний SM будет сгенерировано для такого кода?
```csharp
Task Delay1()
{
    return Task.Delay(1);
}
```
Ответ:
Машина состояний для этого кода вообще не будет сгенерирована, т. к. отсутствует модификатор `async` в объявлении метода `Delay1`.

5. Бонус: Вопрос, который иногда задают на собеседованиях о том, почему запрещено использовать `await` внутри `lock`. Чтобы ответить на него, достаточно концептуально (**упрощенно**) воссоздать код, в который [разворачивается](https://sharplab.io/#v2:D4AQTAjAsAUCDMACciDCiDetE+UkALIgLIAUAlJtrjQDYD2AxgNakAuAFgJYDO5A3NRwBfWMKA==) ключевое слово `lock`:
```csharp
object _syncObj = new();

async Task DelayLocked()
{
    Monitor.Enter(_syncObj);
    await Task.Delay(1);
    Monitor.Exit(_syncObj);
}
```
Ответ:
Если вы еще не сталкивались с этой задачей и не знаете ответ на нее, для лучшего усваивания, я бы рекомендовал попробовать решить ее самостоятельно - запустите код из вышеупомянутого примера, посмотрите что он выдаст, затем посмотрите на сгенерированную машину состояний и постарайтесь понять что происходит. А о результатах своих исследований вы можете написать в комментариях к этой статье. Спойлер: результат выполнения этого кода зависит от контекста синхронизации.

Материал актуален для версии .NET 8.0.1. С удовольствием отвечу на ваши вопросы в комментариях.

Кстати, подход из этой статьи похож на то, что еще в 2017 году делал Сергей Тепляков в [своей статье](https://devblogs.microsoft.com/premier-developer/dissecting-the-async-methods-in-c/). Отличие в том, что в моем примере SM сохранена более аккуратно в первозданном виде, еще сразу внутри SM код сопроважден комментариями, разобран вопрос с аллокациями и приведены разные кейсы.

Заканчивая, хочу поблагодарить Марка Шевченко (@markshevchenko) и Евгения Пешкова (@epeshk) за ревью этой статьи. А еще, интересно, что в будущих версиях .NET `async/await` [может](https://github.com/dotnet/runtime/issues/94620) сильно преобразиться.



---


#### Для обложки

```csharp
private void MoveNext()
{
    int num = <>1__state;
    TaskAwaiter awaiter;
    if (num != 0)
    {
        awaiter = Task.Delay(1).GetAwaiter();
        if (!awaiter.IsCompleted)
        {
            num = (<>1__state = 0);
            <>u__1 = awaiter;
            <Job1>d__1 stateMachine = this;
            <>t__builder.AwaitUnsafeOnCompleted(ref awaiter, ref stateMachine);
            return;
        }
    }
    else
    {
        awaiter = <>u__1;
        <>u__1 = default(TaskAwaiter);
        num = (<>1__state = -1);
    }
    awaiter.GetResult();
}

private void MoveNext()
{
    switch (currentState)
    {
        case State.NotStarted:
            taskAwaiter = Task.Delay(1).GetAwaiter();
            if (taskAwaiter.IsCompleted)
            {
                goto case State.Completed;
            }
            currentState = State.AfterDelay;
            taskBuilder.AwaitUnsafeOnCompleted
            break;
        case State.AfterDelay:
            taskAwaiter.GetResult();
            currentState = State.Completed;
            break;
    }
}
```
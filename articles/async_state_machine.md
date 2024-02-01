# Асинхронная машина состояний
В этом документе приведен пример `машины состояний` (`конечного автомата`, `state machine`), которую генерирует компилятор из асинхронного метода для понимания того, как работает асинхронность в C#.

# "Высокуровневый C#"
Это код на обычном C#.
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
        
        await Task.Delay(200);
    }
}
```

# "Низкоуровневый C#"
Далее следует код, который генерирует компилятор из высокоуровневого (обычного) C#.

Код, сгенерированный компилятором трудно читать, поэтому я [попросил](https://chat.openai.com/share/ea94242e-4a09-4f86-9ef5-d881ade02b73) нейросеть провести небольшой рефакторинг, чтобы сделать его более читаемым. Вот что получилось:
```csharp
[CompilerGenerated]
private sealed class AsyncStateMachine : IAsyncStateMachine
{
    // Определение состояний для машины состояний
    public enum State
    {
        NotStarted,               // Не начато
        WaitingAfterInitialDelay, // Ожидание после начальной задержки
        WaitingForFileRead,       // Ожидание чтения файла
        WaitingAfterFinalDelay,   // Ожидание после последней задержки
        Finished                  // Завершено
    }

    public State CurrentState; // Текущее состояние машины состояний

    public AsyncTaskMethodBuilder Builder; // Строитель задачи асинхронного метода

    public Program Instance; // Экземпляр программы

    private int DelayDuration; // Длительность задержки

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
                        goto case State.WaitingAfterInitialDelay;
                    }
                    CurrentState = State.WaitingAfterInitialDelay;
                    Builder.AwaitUnsafeOnCompleted(ref DelayAwaiter, ref this);
                    break;

                case State.WaitingAfterInitialDelay:
                    // Получение результата начальной задержки
                    DelayAwaiter.GetResult(); // Нужно для обработки исключений, на случай если в асинхронном методе произошла ошибка
                    DelayDuration = int.Parse(Console.ReadLine());
                    DelayAwaiter = Task.Delay(DelayDuration).GetAwaiter();
                    if (DelayAwaiter.IsCompleted)
                    {
                        goto case State.WaitingForFileRead;
                    }
                    CurrentState = State.WaitingForFileRead;
                    Builder.AwaitUnsafeOnCompleted(ref DelayAwaiter, ref this); // Запланирует, что указанная машина состояний будет продвинута вперед после завершения работы указанного awaiter (будет вызван метод MoveNext).
                    break;

                case State.WaitingForFileRead:
                    // Получение результата задержки перед чтением файла
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
                    DelayAwaiter = Task.Delay(200).GetAwaiter();
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
    stateMachine.Builder.Start(ref stateMachine); // Первый вызов MoveNext происходит прямо в Start
    // Исходный код этого метода Start доступен по ссылке: https://source.dot.net/#System.Private.CoreLib/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncMethodBuilderCore.cs,21
    return stateMachine.Builder.Task;
}
```
Кстати, этот код не запустится, потому что в нем нет некоторых вспомогательных методов, которые генерирует компилятор. Но он позволяет понять, как работает асинхронность в C#.

## Advanced
Еще интересно, что в Debug `AsyncStateMachine` для тасков представлен в виде класса, в Release - в виде структуры (`struct`). Но хоть это и структура, если выполнение пойдет действительно по асинхронному сценарию, под капотом в Runtime все-таки произойдет аллокация для `AsyncStateMachine`.

Когда выполнение идет по действительно асинхронному сценарию (вызов `DelayAwaiter.IsCompleted` возвращает `false`), CLR'у машину состояний необходимо переместить из стека в управляемую кучу, для этого она упаковывается (boxing) в [AsyncStateMachineBox<TStateMachine>](https://source.dot.net/#System.Private.CoreLib/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncTaskMethodBuilderT.cs,275) рантаймом. 
Для `Task` это происходит внутри [AsyncTaskMethodBuilder<TResult>.GetStateMachineBox<TStateMachine>](https://source.dot.net/#System.Private.CoreLib/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/AsyncTaskMethodBuilderT.cs,220). 
Для `ValueTask` это происходит внутри [PoolingAsyncValueTaskMethodBuilder<TResult>.GetStateMachineBox<TStateMachine>](https://source.dot.net/#System.Private.CoreLib/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/PoolingAsyncValueTaskMethodBuilderT.cs,212). Обратите внимание, что в CLR предусмотрена возможность [пулинга](https://source.dot.net/#System.Private.CoreLib/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/PoolingAsyncValueTaskMethodBuilderT.cs,212) `AsyncStateMachineBox` для минимизации аллокаций (метод [StateMachineBox<TStateMachine> RentFromCache()](https://source.dot.net/#System.Private.CoreLib/src/libraries/System.Private.CoreLib/src/System/Runtime/CompilerServices/PoolingAsyncValueTaskMethodBuilderT.cs,299)). Подробнее о пулинге машины состояний можно почитать в [статье Стивена Тауба](https://devblogs.microsoft.com/dotnet/async-valuetask-pooling-in-net-5/).

Скриншоты аллокация для тестового метода [RealAsyncScenarioYield](../AsyncAwaitAllocation/Program.cs#L15): ![dotMemory Screenshot](../AsyncAwaitAllocation/dotmemory_screenshots/RealAsyncScenario_Allocations.png)

Проверялось в декабре 2023 на .NET 8.

# Что еще почитать по теме

Например, [целый гайд от Стивена Тауба "How Async/Await Really Works in C#"](https://devblogs.microsoft.com/dotnet/how-async-await-really-works/) ("Как на самом деле работает Async/Await в C#"). Переводы [доступны](https://habr.com/ru/articles/732738/) на Хабре.

Или статью [Prefer ValueTask to Task, always; and don't await twice](https://blog.marcgravell.com/2019/08/prefer-valuetask-to-task-always-and.html), которая описывает не только особенности ValueTask, но и то, как реализовать даже асинхронную логику через `` или ``, минимизируя кол-во выделений памяти в куче.

# sharplab.io

Оригинальный код и машину состояний можно посмотреть в sharplab.io: https://sharplab.io/#v2:CYLg1APgAgTAjAWAFBQAwAIpwKwG5lqZwB0AkgPL5IEDMmMRA7OgN7LofoAOATgJYA3AIYAXAKZEMAfQBmfADZiAwgHsAduI1VO6dpyh0oADkwA2dAFkhfNQAoAlKz06OUAJxniAETHyhAT1s4VFR7bRcOZxcbEXRgXwD0AF50GOIABSEeAGcxW1U1bJVFYgAlMSFgABkbPPswqJ13Tx8/QPi2hqQIyO6e2QVldU1YlOaAMUGyiuAAQXl5ABUxAA8RWez/NQBjWwAiOUU4Pa6e3T6I5qhTbwTAmBDTzgBfZGegA=

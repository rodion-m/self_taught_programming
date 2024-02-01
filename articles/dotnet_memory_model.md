# Модель памяти .NET

В двух словах, модель памяти (memory model) — это довольно сложная, низкоуровневая и запутанная тема, которая, прежде всего, касается магии, которая может происходит в многопоточных приложениях. 
И в случае когда разработчики хотят достичь максимальной производительности (в обход относительного тяжелого `lock`), им приходится использовать `volatile` и порчие `барьеры памяти`. Скорее всего, среднестатическому .NET разработчику в большинстве случаев знания о тонкостях модели памяти в .NET вряд ли пригодятся. 

# Что самое важное нужно знать про модель памяти?

* То, что она существует и что значение переменной может кешироваться в кеше процессора (ядра), а также что комплиятор и процессор могут выполнять перестановки операций в целях оптимизации.
* То, что ключевое слово `volatile` лучше не использовать вообще, если вы хоть чуть-чуть не уверены в том, как это работает.
  * Комментарий от Eric Lippert ex. разработчика из Microsoft, в котором он рекомендует не надеется на низкоуровневые примитивы синхронизации: https://stackoverflow.com/a/19384758 https://stackoverflow.com/a/22458009
  * А вот комментарий от Jon Skeet автора книги "C# in Depth" о том, что он сам долгое время имел ошибочное представление о значении этого модификатора и даже приводил его в своих книгах: https://stackoverflow.com/a/2005237
  * И вот статья про atomics в общем и почему их следует избегать бизнесовым разработчикам с примерами ошибок от Dmitry Vyukov из Google (Intel® Black Belt Software Developers): https://abseil.io/docs/cpp/atomic_danger
* Если вам вдруг понадобился барьер памяти, то можно поставить полный барьер через `Thread.MemoryBarrier`, либо через методы из `Interlocked`.
* В условных 99% случаев в бизнесовой разработке для синхронизации будет достаточно `lock` либо `SemaphoreSlim.WaitAsync` для синхронизации асинхронного кода.
* Но кому тогда это нужно? Например, если вы разработываете свою СУБД, либо дорабатываете существующую. И в целом, это может пригодится когда нужен сверхперфоманс (в бизнесовой разработке почти всегда будет миллион других куда более существенных узких мест для оптимизации, прежде, чем дело дойдет до добавления какого-нибудь `volatile`).

# Где почитать про модель памятия в .NET?

Доступно несколько документов по этой теме.

Самый актуальный — это официальная спецификация: https://github.com/dotnet/runtime/blob/main/docs/design/specs/Memory-model.md

Еще документы:
* [ECMA 335: chapter "I.12.6 Memory Model and Optimizations"](https://github.com/rodion-m/articles/blob/main/documents/ecma_335_memory_model_and_optimizations.md)
* [Статья от Joe Duffy 2007 года "CLR 2.0 Memory Model"](http://joeduffyblog.com/2007/11/10/clr-20-memory-model/)
* Статья от  Igor Ostrovsky "C# - The C# Memory Model in Theory and Practice": [часть 1](https://learn.microsoft.com/en-us/archive/msdn-magazine/2012/december/csharp-the-csharp-memory-model-in-theory-and-practice), [часть 2](https://docs.microsoft.com/en-us/archive/msdn-magazine/2013/january/csharp-the-csharp-memory-model-in-theory-and-practice-part-2)
* [Memory model в mono](https://www.mono-project.com/docs/advanced/runtime/docs/atomics-memory-model/)
* [И небольшое чтиво на русском из репозитория dotEducation](https://github.com/skbkontur/dotEducation/blob/main/csharp/memory_model.md)

Бонус: [Доклад "A Gentle Introduction To Low-Level Concurrency In .NET (Szymon Kulec)"](https://www.youtube.com/watch?v=dtUrG--oMLo)

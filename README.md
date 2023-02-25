# Сборник материалов для изучения программирования
В этом репозитории мы собираем лучшие материалы и источники для самостоятельного изучения программирования от нулевого до продвинутого уровня.

# Навигация
- Для начинающих
- Информатика
- [Алгоритмы и структуры данных \(АиСД\)](#Алгоритмы-и-структуры-данных)
- [Многопоточность и асинхроннось](#Материалы-по-многопоточности-и-асинхронности)
- Базы данных
  - ORM
    - Entity Framework (.NET)
- [Проектирование, архитектура и System Design](#Проектирование-архитектура-и-System-Design)
  - SOLID
  - GoF-паттерны
  - GRASP
  - Чистый код, Best Practice
  - Domain-driven-design (DDD)
  - [Чистая архитектура](#Чистая-архитектура)
  - Распределенные системы
- [Тестирование для программиста](#Тестирование-для-программиста)
- Память и сборщик мусора (GC)
  - Общее
  - .NET
  - Go
- Прочее
  - YouTube-каналы
    - Уровень: Начальный
    - Уровень: Продвинутый
  - Подкасты
  - Telegram-каналы
  - Чаты (сообщества)

***

# Алгоритмы и структуры данных

# Материалы по многопоточности и асинхронности
## Записи курсов
* [Семинары CLRium "Concurrency и Parallelism" (Станислав Сидристый и Ко.)](https://www.youtube.com/playlist?list=PLBwwJL9lzKMY9Fpk1DAscywid1Xshp9NL)
* [Курс "Параллельное программирование" 2022 (CS Center, Евгений Калишенко)](https://www.youtube.com/playlist?list=PLlb7e2G7aSpTBs1GPt-4UygYxK3bVSyZe)
* [Курс "Теория и практика многопоточной синхронизации (ТПМС, Concurrency)" [Лекции] 2022 (ФМПИ, Роман Липовский)](https://www.youtube.com/playlist?list=PL4_hYwCyhAva37lNnoMuBcKRELso5nvBm)
* [Курс "Теория и практика многопоточной синхронизации (ТПМС, Concurrency)" [Семинары] 2022 (ФМПИ, Роман Липовский)](https://www.youtube.com/playlist?list=PL4_hYwCyhAvYTxm55RBm_HA5Bq5W1Nv-R)
* [Курс "Параллельные и распределённые вычисления" 2021 (ФПМИ, Ивченко О. Н.)](https://www.youtube.com/playlist?list=PL4_hYwCyhAvbof7wirWXeCH9wAfgDH0RI)
* [[JS] Сборник лекций и докладов на тему "Асинхронное программирование" (Тимур Шемсединов)](https://www.youtube.com/playlist?list=PLHhi8ymDMrQZ0MpTsmi54OkjTbo0cjU1T)
* [[EN] Бесплатные уроки курса Async Expert от сообщества Dotnetos](https://www.youtube.com/playlist?list=PLpUkQYy-K8Y_Xx_bFQSXwCmvL-Uin88zN)

## Доклады
* [Лекция "Многопоточное программирование в .NET ч. 1"  (Дмитрий Иванов)](https://youtu.be/GBCGL4GDgN4)
* [Лекция "Многопоточное программирование в .NET ч. 2" (Дмитрий Иванов)](https://www.youtube.com/watch?v=dH6ZW8KOGFY)
* ["ThreadPool для сервиса, адаптирующегося под внешнюю нагрузку" (Станислав Сидристый)](https://www.youtube.com/watch?v=LbiuLwNJd1I)
* ["Разграничение ответственности между процессорными ядрами" (Станислав Сидристый)](https://www.youtube.com/watch?v=bHX7lwttrCA)
* ["Тонкие настройки стандартного ThreadPool" (Станислав Сидристый)](https://www.youtube.com/watch?v=zeWhoFWGWKo)
* ["Здоровое || программирование – многопоточность vs асинхронность, зачем нам ThreadPool, контексты исполнения" (Родион Мостовой)](https://youtu.be/MqO1iMVbdOs?t=432)
* [[EN] "The C++ and CLR Memory Models" (Sasha Goldshtein)](https://www.youtube.com/watch?v=6wZVpg2SyJQ)

## Книги и статьи
* [Online-книга "DotNetBook" – глава про потоки (Стас Сидристый)](https://github.com/sidristij/dotnetbook/tree/master/book/ru/Execution/01-Threads)
* [О Thread и ThreadPool в .NET подробно (часть 1)](https://habr.com/ru/post/654101/)
* [О Thread и ThreadPool в .NET подробно (часть 2)](https://habr.com/ru/post/654111/)
* [ConfigureAwait: часто задаваемые вопросы (перевод статьи Stephen Toub)](https://habr.com/ru/post/482354/)
* [Барьеры памяти и неблокирующая синхронизация в .NET](https://habr.com/ru/post/130318/)
* [Многопоточность на низком уровне](https://habr.com/ru/company/jugru/blog/543380/)
* [Введение в lock-free программирование](https://habr.com/ru/company/wunderfund/blog/322094/)
* [.NET: Инструменты для работы с многопоточностью и асинхронностью. Часть 1](https://habr.com/ru/post/452094/)
* [.NET: Инструменты для работы с многопоточностью и асинхронностью. Часть 2](https://habr.com/ru/post/459514/)
* [Async/await в C#: концепция, внутреннее устройство, полезные приемы](https://habr.com/ru/post/470830/)
* [System.Threading.Channels — высокопроизводительный производитель-потребитель и асинхронность без аллокаций и стэк дайва](https://habr.com/ru/post/508726/)
* [Порт завершения (Completion Port)](https://habr.com/ru/post/59282/)
* [Что означает RISC и CISC?](https://habr.com/ru/company/selectel/blog/542074/)
* [[EN] Статья "ExecutionContext vs SynchronizationContext" (Stephen Toub)](https://devblogs.microsoft.com/pfxteam/executioncontext-vs-synchronizationcontext/)
* [[EN] Статья "There Is No Thread" (Stephen Cleary)](https://blog.stephencleary.com/2013/11/there-is-no-thread.html)
* [[EN] The CLR Thread Pool 'Thread Injection' Algorithm](https://mattwarren.org/2017/04/13/The-CLR-Thread-Pool-Thread-Injection-Algorithm/)

## Подкасты
* [DotNet&More #31: Многопоточность и не только](https://music.yandex.ru/album/18268100/track/91640946)
* [Podlodka #102 – Многопоточность (с Романом Елизаровым)](https://podlodka.io/102)
* [Podlodka #56 – Корутины, Промисы, Акторы](https://podlodka.io/56)
* [Mobile People Talks: Асинхронность – знаешь что об этом ты?](https://music.yandex.ru/album/9647763/track/61693076) [часть 2](https://music.yandex.ru/album/9647763/track/61693077)
* [Flutter Dev Podcast: Асинхронность](https://music.yandex.ru/album/11609672/track/69414771)
* [Moscow Python Podcast. Асинхронщина с базами данных: aiopg и другие звери](https://music.yandex.ru/album/6892837/track/53235626)

## Прочее
* [Публичное собеседование по многопоточности (Kotlin)](https://www.youtube.com/watch?v=2hLHgcsOV3)

## Платные курсы
* [[EN] Async Expert – Большой курс от сообщества Dotnetos (Konrad Kokosa, Łukasz Pyrzyk, Szymon Kulec)](https://asyncexpert.com/)

***

# Базы данных
## ORM
### Entity Framework
* [ORM с нуля | Как устроены Entity Framework и Dapper? | Как реализовать IQueryable? (Андрей Подколзин)](https://www.youtube.com/live/UvO-8p3JqW0)

***

# Проектирование, архитектура и System Design
## Чистый код, Best Practice
* [Node.js Лучшие практики](https://github.com/goldbergyoni/nodebestpractices/blob/master/README.russian.md)
* [[EN] The Node.js best practices list](https://github.com/goldbergyoni/nodebestpractices)

## Чистая архитектура
### Платные курсы
* [[EN] Clean Architecture: Patterns, Practices, and Principles (Matthew Renze)](https://app.pluralsight.com/library/courses/clean-architecture-patterns-practices-principles/)

***

# Тестирование для программиста
## Лекции

## Книги и статьи
* [[EN] Comprehensive and exhaustive JavaScript & Node.js testing best practices](https://github.com/goldbergyoni/javascript-testing-best-practices)

## Платные курсы
* [[EN] From Zero to Hero: Unit testing in C# (Nick Chapsas)](https://nickchapsas.com/p/from-zero-to-hero-unit-testing-in-c)
* [[EN] From Zero to Hero: Integration testing in ASP.NET Core [Nick Chapsas]](https://nickchapsas.com/p/from-zero-to-hero-integration-testing-in-asp-net-core)
* [[EN] Building a Pragmatic Unit Test Suite (Vladimir Khorikov)](https://app.pluralsight.com/library/courses/pragmatic-unit-testing/)
* [[EN] Unit Testing an ASP.NET Core 6 Web API (Kevin Dockx)](https://app.pluralsight.com/library/courses/asp-dot-net-core-6-web-api-unit-testing/)
* [[EN] NODE.JS Testing From A to Z (Yoni Goldberg)](https://testjavascript.com/)

***

# Прочее
## YouTube каналы
* [Сообщество youtube-каналов с IT контентом на любые темы и уровни](https://ityoutubers.com/)

***

## TODO
* Добавить обозначение сложности материала для каждой из ссылок
* Добавить краткое описание каждого курса/доклада/статьи
* Добавить подробную навигацию

***

#### Материалы будут пополняться и структурироваться. PR горячо приветствуются.

# База знаний по изучению программирования
В этом репозитории мы собираем лучшие материалы и источники для самостоятельного изучения программирования от нулевого до продвинутого уровня. Все материалы на **русском** языке, за исключением случаев, когда указан префикс [EN].

# Навигация
- [Начинающим](#начинающим)
- [Курсы разработки ПО](#курсы-разработки-ПО)
- Информатика
- [Алгоритмы и структуры данных \(АиСД\)](#алгоритмы-и-структуры-данных)
- [Многопоточность и асинхроннось](#многопоточность-и-асинхронность)
- [Базы данных](#базы-данных)
  - [ORM](#orm)
    - [Entity Framework (.NET)](#entity-framework)
- [Проектирование, архитектура и System Design](#проектирование-архитектура-и-System-Design)
  - SOLID
  - [GoF-паттерны](#gof-паттерны)
  - GRASP
  - Чистый код, Best Practice
  - [Domain-driven design (DDD)](#domain-driven-design-ddd)
  - [Чистая архитектура](#чистая-архитектура)
  - Распределенные системы
  - System Design
  - [Референсные проекты](#референсные-проекты)
    - [.NET](#референсные-проекты-на-NET)
- [Процессы разработки и методологи](#процессы-разработки-и-методологии)
- [Тестирование для программиста](#тестирование-для-программиста)
- Память и сборщик мусора (GC)
  - Общее
  - .NET
  - Go
- [CI/CD](#cicd)
- Собеседования
  - .NET
- [Инструментарий](#инструментарий)
  - [Git](#git)
  - [Регулярные выражения](#регулярные-выражения)
  - IDE
- [Прочее](#прочее)
  - [YouTube-каналы](#youtube-каналы)
    - Уровень: Начальный
    - Уровень: Продвинутый
  - Подкасты
  - Telegram-каналы
  - [Чаты (сообщества)](#чаты-сообщества)

***

# Начинающим
## Записи курсов
- [CS50 – Гарвардский курс по основам программирования](https://www.youtube.com/playlist?list=PLawfWYMUziZqyUL5QDLVbe3j5BKWj42E5)
- [Увлекательное программирование на C# (Дмитрий Сошников)](https://www.youtube.com/playlist?list=PL6XUtJhtlpPM3mTfgYBY5Zql4b4szL4KP)
- [Основы программирования и анализа данных на Python (Хирьянов Тимофей Фёдорович)](https://youtube.com/playlist?list=PLcsjsqLLSfND6vNUS4b13dHJHOxLm0nv5)
- ["C# 2022 с нуля до профи" (9 часовой курс)](https://www.youtube.com/watch?v=w8rRhAup4kg)

## Бесплатные онлайн курсы
- [Ulearn.me: Множество курсов по основам программирования на C#](https://ulearn.me/)

***

# Курсы разработки ПО
- [Курс "Разработка ПО" в CS Center о жизненном цикле, методологиях и прочем (Тимофей Брыксин)](https://www.youtube.com/playlist?list=PLlb7e2G7aSpSidTs7HuqUX_NeslBsg2Mb)
- [Курс "Проектирование программного обеспечения" в CS Center (Юрий Литвинов)](https://www.youtube.com/playlist?list=PLlb7e2G7aSpQwYFLXBG22XnKYXFzQ7-1K)
- ["Школа бекенд-разработки 2021" (Python) от Академии Яндекса](https://www.youtube.com/playlist?list=PLQC2_0cDcSKCMKnywAS8eI_EgCcE3yx0r)
- ["Школа бекенд-разработки 2022" (Python, Java) от Академии Яндекса](https://www.youtube.com/playlist?list=PLQC2_0cDcSKB0fq36NuDhbpcd20OVNPBp)

***

# Алгоритмы и структуры данных
- [Курс по оценке сложности алгоритмов на ulearn.me](https://ulearn.me/Course/complexity/)

***

# Многопоточность и асинхронность
## Записи курсов
* [Семинары CLRium "Concurrency и Parallelism" (Станислав Сидристый и Ко.)](https://www.youtube.com/playlist?list=PLBwwJL9lzKMY9Fpk1DAscywid1Xshp9NL)
* [Курс "Параллельное программирование" 2022 (CS Center, Евгений Калишенко)](https://www.youtube.com/playlist?list=PLlb7e2G7aSpTBs1GPt-4UygYxK3bVSyZe)
* [Курс "Теория и практика многопоточной синхронизации (ТПМС, Concurrency)" [Лекции] 2022 (ФМПИ, Роман Липовский)](https://www.youtube.com/playlist?list=PL4_hYwCyhAva37lNnoMuBcKRELso5nvBm)
* [Курс "Теория и практика многопоточной синхронизации (ТПМС, Concurrency)" [Семинары] 2022 (ФМПИ, Роман Липовский)](https://www.youtube.com/playlist?list=PL4_hYwCyhAvYTxm55RBm_HA5Bq5W1Nv-R)
* [Курс "Параллельные и распределённые вычисления" 2021 (ФПМИ, Ивченко О. Н.)](https://www.youtube.com/playlist?list=PL4_hYwCyhAvbof7wirWXeCH9wAfgDH0RI)
* [[JS] Сборник лекций и докладов на тему "Асинхронное программирование" (Тимур Шемсединов)](https://www.youtube.com/playlist?list=PLHhi8ymDMrQZ0MpTsmi54OkjTbo0cjU1T)
* [[EN] Бесплатные уроки курса Async Expert от сообщества Dotnetos](https://www.youtube.com/playlist?list=PLpUkQYy-K8Y_Xx_bFQSXwCmvL-Uin88zN)

## Доклады
* [Лекция "Многопоточное программирование в .NET ч. 1" (Дмитрий Иванов)](https://youtu.be/GBCGL4GDgN4)
* [Лекция "Многопоточное программирование в .NET ч. 2" (Дмитрий Иванов)](https://www.youtube.com/watch?v=dH6ZW8KOGFY)
* ["ThreadPool для сервиса, адаптирующегося под внешнюю нагрузку" (Станислав Сидристый)](https://www.youtube.com/watch?v=LbiuLwNJd1I)
* ["Разграничение ответственности между процессорными ядрами" (Станислав Сидристый)](https://www.youtube.com/watch?v=bHX7lwttrCA)
* ["Тонкие настройки стандартного ThreadPool" (Станислав Сидристый)](https://www.youtube.com/watch?v=zeWhoFWGWKo)
* ["Здоровое || программирование – многопоточность vs асинхронность, зачем нам ThreadPool, контексты исполнения" 2022 (Родион Мостовой)](https://youtu.be/MqO1iMVbdOs?t=432)
* ["Модель памяти .NET" 2017 (Валерий Петров)](https://youtu.be/m9_aBxdKrRI)
* [lock(_sync): иллюзия идеального выбора (Станислав Сидристый)](https://youtu.be/PEHQjSwp01k)
* [[EN] "The C++ and CLR Memory Models" (Sasha Goldshtein)](https://www.youtube.com/watch?v=6wZVpg2SyJQ)
* [[EN] "How Interlocked and Volatile works in .NET" 2020 (Dotnetos)](https://youtu.be/0xXeYzYFbrk)

## Книги и статьи
* [Online-книга "DotNetBook" – глава про потоки (Стас Сидристый)](https://github.com/sidristij/dotnetbook/tree/master/book/ru/Execution/01-Threads)
* [Полное понимание асинхронности в браузере](https://habr.com/ru/company/yandex/blog/718084/)
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
* [Публичное собеседование по многопоточности (Kotlin, Android)](https://youtu.be/2hLHgcsOV3s)

## Платные курсы
* [[EN] Async Expert – Большой курс от сообщества Dotnetos (Konrad Kokosa, Łukasz Pyrzyk, Szymon Kulec)](https://asyncexpert.com/)

***

# Базы данных
## ORM
### Entity Framework
* [ORM с нуля | Как устроены Entity Framework и Dapper? | Как реализовать IQueryable? (Андрей Подколзин)](https://www.youtube.com/live/UvO-8p3JqW0)

***

# Проектирование, архитектура и System Design

## GoF паттерны
- [Курс "Проектирование программного обеспечения" в CS Center (Юрий Литвинов)](https://www.youtube.com/playlist?list=PLlb7e2G7aSpQwYFLXBG22XnKYXFzQ7-1K)

## Domain-Driven Design (DDD)
- [Занятие "Предметно-ориентированное проектирование" из курса по проектированию ПО (Юрий Литвинов)](https://youtu.be/pAdC0JyqRJE)

## Чистый код, Best Practice
* [Node.js Лучшие практики](https://github.com/goldbergyoni/nodebestpractices/blob/master/README.russian.md)
* [[EN] The Node.js best practices list](https://github.com/goldbergyoni/nodebestpractices)

## Чистая архитектура
### Платные курсы
* [[EN] Clean Architecture: Patterns, Practices, and Principles (Matthew Renze)](https://app.pluralsight.com/library/courses/clean-architecture-patterns-practices-principles/)

## Референсные проекты
### Референсные проекты на .NET
- [Пример микросервисного приложения eShop по DDD от Microsoft](https://github.com/dotnet/eShop)
- [Пример веб-приложения на ASP.NET Core со слоенной архитектурой от Microsoft](https://github.com/dotnet-architecture/eShopOnWeb)
- [Пример микросервисного веб-приложения на ABP Framework, работает на Kubernetes, Helm, включает API-Gateway и приложения на Angular](https://github.com/abpframework/eShopOnAbp)
- [Шаблон проекта на ASP.NET Core с чистой архитектурой](https://github.com/jasontaylordev/CleanArchitecture)
- [Пример интернет-магазина, реализованного на Blazor Server](https://github.com/dotnet-architecture/eShopOnBlazor)
- [Шаблон проекта на ASP.NET Core + React + Redux + TypeScript + Hot Module Replacement ](https://github.com/based-ghost/aspnet-core-react-redux-playground-template)
- [Пример веб-приложение на Blazor WebAssembly с чистой архитектурой](https://github.com/jasontaylordev/RapidBlazor)
- [Шаблон "надежного ASP.NET Core веб-приложения, использующего сервисы Azure"](https://github.com/Azure/reliable-web-app-pattern-dotnet)

***

# Процессы-разработки-и-методологии
- [Курс "Разработка ПО" в CS Center о жизненном цикле, методологиях и прочем (Тимофей Брыксин)](https://www.youtube.com/playlist?list=PLlb7e2G7aSpSidTs7HuqUX_NeslBsg2Mb)

***

# Тестирование для программиста
## Записи курсов, занятия
- [EQSP 12/20: Философия автоматизированных тестов (Егор Бугаенко)](https://www.youtube.com/watch?v=ZPvGF9KtYO8&list=PLRslTbFwdzZ_rscv7zd9hwR21mgC_bAsk&index=1)
- [Занятие "Python - Тестирование" (Сергей Бочкарев) из курса "ШБР 2022" Академии Яндекса](https://www.youtube.com/watch?v=957lkNw-ThE)
- [Занятие "Java - Тестирование" (Сергей Волков) из курса "ШБР 2022" Академии Яндекса](https://www.youtube.com/watch?v=DVsd37jocZ4)
- [Занятие "Нагрузочное тестирование" (Григорий Липин) из курса "ШБР 2022" Академии Яндекса](https://youtu.be/rkDaMowYrUM)

## Доклады
- [Эффективное юнит-тестирование (Владимир Хориков)](https://www.youtube.com/watch?v=AAD9se2LjuI&list=PLRslTbFwdzZ_rscv7zd9hwR21mgC_bAsk&index=4&)
- [Юнит-тестирование в разработке (Сергей Немчинский)](https://youtu.be/KAny2OSYY3Y?list=PLRslTbFwdzZ_rscv7zd9hwR21mgC_bAsk)
- [Имитируем с Moq (Иван Кожин)](https://www.youtube.com/watch?v=XmVhRPZlj8g)
- [Мутационное тестирование в .NET (Николай Молчанов)](https://youtu.be/5gCvcUcctuU?list=PLRslTbFwdzZ_rscv7zd9hwR21mgC_bAsk)
- [[EN] Creating a QA/DEV Collaborative Testing Strategy (Roy Osherove)](https://www.youtube.com/watch?v=HUpj4YXI8Bs)

## Книги и статьи
* [Лучшие практики тестирования JavaScript и Node.js](https://github.com/goldbergyoni/javascript-testing-best-practices/blob/master/readme-ru.md)
* [[EN] Comprehensive and exhaustive JavaScript & Node.js testing best practices](https://github.com/goldbergyoni/javascript-testing-best-practices/)

## Платные курсы
* [[EN] From Zero to Hero: Unit testing in C# (Nick Chapsas)](https://nickchapsas.com/p/from-zero-to-hero-unit-testing-in-c)
* [[EN] From Zero to Hero: Integration testing in ASP.NET Core [Nick Chapsas]](https://nickchapsas.com/p/from-zero-to-hero-integration-testing-in-asp-net-core)
* [[EN] Building a Pragmatic Unit Test Suite (Vladimir Khorikov)](https://app.pluralsight.com/library/courses/pragmatic-unit-testing/)
* [[EN] Unit Testing an ASP.NET Core 6 Web API (Kevin Dockx)](https://app.pluralsight.com/library/courses/asp-dot-net-core-6-web-api-unit-testing/)
* [[EN] NODE.JS Testing From A to Z (Yoni Goldberg)](https://testjavascript.com/)

***

## Подкасты
- [[S01E10] Testing | BookClub DOTNET - Тестирование микросервисов (Владимир Хориков)](https://youtu.be/jmRCoi6-57Y?list=PLRslTbFwdzZ_rscv7zd9hwR21mgC_bAsk)

***

# CI/CD
- [Docker - Полный курс Docker Для Начинающих [3 ЧАСА]](https://www.youtube.com/watch?v=_uZQtRyF6Eg)

***

# Инструментарий
## Git
- [Краткие инструкции по Git (что делать, если что-то пошло не так)](https://github.com/k88hudson/git-flight-rules/blob/master/README_ru.md)

***

## Регулярные выражения
- [Небольшой учебник по изучению регулярных выражений на GitHub](https://github.com/ziishaned/learn-regex/blob/master/translations/README-ru.md)
- [Кроссворды на регулярках — геймификация в изучении регулярок](https://regexcrossword.com/)

***

# Прочее
## YouTube каналы
* [Сообщество YouTube-каналов с IT контентом на любые темы и уровни](https://ityoutubers.com/)

***

## Чаты (сообщества)
* [Большой сборник русскоязычных IT-чатов](https://github.com/mtdvio/ru-tech-chats)

***

## TODO
* Добавить обозначение сложности материала для каждой из ссылок
* Добавить краткое описание каждого курса/доклада/статьи
* Добавить подробную навигацию
* В виде спойлера вывести все темы из курсов после названия курса

***

#### Материалы будут пополняться и структурироваться. PR горячо приветствуются.

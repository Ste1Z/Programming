Назад к: [[Spring]]

---
## Содержание:

Общая информация
Ключевые моменты
Основные понятия
Как работает
Возможные вопросы

---
## Общая информация:


---
## Ключевые моменты:


---
## Основные понятия:


---
## Как работает:


---
## Возможные вопросы:
Реактивный манифест.
Разница между hot и cold publisher?
Реактивные типы в WebFlux.
Разница между Netty и Tomcat?
Что такое event loop?
Как работает Event Loop?

---

**Реактивный манифест.**
1. **Отзывчивость**
	Система способна быстро обрабатывать запросы пользователей даже при высокой нагрузке. Это требует соблюдения нескольких ключевых принципов проектирования.  
	- **Неблокирующий ввод/вывод:** Использование неблокирующего ввода-вывода, позволит минимизировать время, которое потоки тратят на ожидание завершения операций ввода-вывода. Более эффективное использование потоков снижает вероятность "голодания" потоков и увеличивает производительность сервиса.
	- **Балансировка нагрузки** гарантирует, что ни один экземпляр не будет перегружен трафиком. Таким образом увеличивая пропускную способность и производительность системы. Существуют различные алгоритмы распределения запросов, но их цель одна: распределить запросы равномерно на существующие и работоспособные экземпляры системы.
	- **Кэширование:** система должна использовать методы кэширования для сокращения времени, затрачиваемого на обработку запросов, и повышения общей производительности системы. Допустим, у вас есть сервис справочников, который содержит значения для различных списков: статусы заказа, названия категорий товаров. Нет смысла каждый раз обращаться в сервис справочной информации, если данные там меняются нечасто. Кэширование этой информации позволит снизить затраты на межсервисное взаимодействие, а также позволит сервису продолжить работу, даже если сервис справочников будет недоступен.
2. **Устойчивость**
	Система должна продолжать работать во время сбоев и автоматически восстанавливаться после ошибок, а не полностью выходить из строя.
	Существует несколько ключевых стратегий, которые могут помочь обеспечить устойчивость:  
	- **Отказоустойчивость:** Реактивная система должна быть спроектирована таким образом, чтобы выдерживать сбои на всех уровнях, включая аппаратные, сетевые и программные сбои. Это может быть достигнуто благодаря избыточности, репликации и механизмов автоматического распределения нагрузки с отказавших экземпляров сервисов на здоровые экземпляры.
	- **Автоматическое восстановление:** Реактивная система должна уметь автоматически обнаруживать и диагностировать ошибки, а также предпринимать корректирующие действия для восстановления после сбоев без вмешательства разработчиков.
	- **Вынужденная деградация (Graceful degradation):** Система должна продолжать работать, даже если некоторые её компоненты или функции недоступны или работают неправильно. Вместо того чтобы полностью разрушиться, система плавно деградирует, отключая несущественные функции, снижая функциональность или предоставляя запасные варианты. Это позволяет системе продолжать работать, хотя и с ограниченными возможностями, пока проблема не будет устранена.
	- **Предохранители (Circuit breakers):** Модель проектирования, которая может помочь предотвратить каскадные отказы в системе. Работает путём мониторинга количества отказов, происходящих в сервисе за определённый период времени, и автоматически отключает предохранитель, если количество отказов превышает заданный порог. Можно реализовать различные предохранители. Например, если сервис не отвечает на запрос, начать возвращать дефолтное значение или кэшированное значение. Все зависит от ваших сценариев.
	- **Контроль потока данных (Backpressure):** Механизм, позволяющий получателю управлять скоростью получения данных от отправителя. Иными словами, это метод контроля потока информации между отправителем и получателем.
	Включив эти стратегии в конструкцию реактивной системы, можно достичь высокого уровня устойчивости и гарантировать, что система способна быстро восстанавливаться после ошибок и продолжать бесперебойную работу.
3. **Эластичность**
	Реактивная система должна иметь возможность масштабирования для обработки растущих рабочих нагрузок без снижения производительности и доступности.
	Существует несколько методов, которые могут быть использованы для достижения эластичности в реактивной системе:
	- **Горизонтальное масштабирование:** Предполагает добавление дополнительных экземпляров сервиса для распределения рабочей нагрузки на несколько машин или узлов. Этот подход может использоваться для обработки растущего трафика или запросов пользователей и может быть достигнут при использовании технологий контейнеризации, таких как Docker, и инструментов оркестрации, таких как Kubernetes.
	- **Вертикальное масштабирование:** Предполагает добавление дополнительных ресурсов (процессор, память или дисковое пространство) к существующему сервису для увеличения его мощности. Этот подход может быть использован для обработки возросших объёмов данных или требований к обработке, и может быть реализован с помощью облачных инфраструктурных сервисов, таких как Amazon EC2 или Microsoft Azure.
	- **Автомасштабирование:** Предполагает использование автоматизированных инструментов или алгоритмов для динамической корректировки количества экземпляров или ресурсов, выделяемых сервису, на основе показателей, собираемых в реальном времени, таких как использование процессора, памяти или сетевого трафика. Динамическая корректировка может увеличить количество экземпляров сервиса при увеличении количества запросов, а также уменьшить количество экземпляров при уменьшении нагрузки. Увеличение позволяет обработать непредсказуемый рост нагрузки, а уменьшение позволяет эффективнее потреблять доступные ресурсы и не платить накладные расхода за не используемые.
	Достижение эластичности в реактивной системе требует сочетания архитектурного проектирования, управления инфраструктурой и инструментов автоматического масштабирования для обеспечения того, чтобы система могла адаптироваться к изменяющимся рабочим нагрузкам и поддерживать свою производительность и доступность в течение долгого времени.
4. **Управление сообщениями**
	Message Driven — шаблон проектирования, используемый в реактивных системах для обеспечения асинхронной связи между различными компонентами. Этот паттерн позволяет сервисам отправлять и получать сообщения без блокировки, ожидания или удержания ресурсов, что помогает минимизировать потребление ресурсов и максимизировать пропускную способность.
	Для обеспечения коммуникации на основе сообщений реактивные системы обычно используют брокер сообщений или очередь сообщений, которая выступает в качестве посредника между различными сервисами. Когда сервис посылает сообщение другому сервису, он помещает его в очередь сообщений, а принимающий компонент может получить сообщение асинхронно, когда он будет готов обработать его.
	Такой подход позволяет сервисам работать независимо друг от друга, без необходимости знать о состоянии или доступности других сервисов. Он также позволяет сервисам обрабатывать большие объёмы сообщений или событий, не перегружаясь и не блокируясь входящим трафиком.

Источник: [struchkov.dev](https://struchkov.dev/blog/ru/overview-of-reactive-programming/)

**Разница между hot и cold publisher?**
Cold-publisher — это поток данных, который начинает генерировать значения только после того, как подписчик на него подписался. 
Hot-publisher — это поток данных, который генерирует значения независимо от подписчиков. Новые подписчики присоединяются к уже идущему потоку и получают только те данные, которые сгенерированы с момента их подключения. 

**Реактивные типы в WebFlux?**
**Flux** - это Publisher, который может испускать от 0 до N элементов, а **Mono** может испускать от 0 до 1 элемента. Оба они завершаются либо сигналом завершения, либо ошибкой, и они вызывают методы onNext, onComplete и onError нижестоящего подписчика. Помимо реализации функций, описанных в спецификации Reactive Streams, Flux и Mono предоставляют набор операторов для поддержки преобразований, фильтрации и обработки ошибок.

**Разница между Netty и Tomcat?**
**Tomcat** и **Netty** — это веб-серверы и сетевые фреймворки, используемые в Java для обработки HTTP-запросов.
**Tomcat** — это классический контейнер сервлетов, основанный на блокирующей модели ввода-вывода (I/O), который управляет запросами с использованием потоков. Он ориентирован на блокирующую обработку и традиционно используется с **Spring MVC** и другими синхронными фреймворками. Tomcat можно настроить для неблокирующей обработки с помощью настроек **NIO**, но он остается преимущественно синхронным.
В синхронном режиме каждый входящий HTTP-запрос обрабатывается отдельным потоком.
Максимальное количество потоков определяется параметром `maxThreads` (по умолчанию — 200 потоков). Минимальное количество потоков можно настроить параметром `minSpareThreads` (по умолчанию — 10 потоков).
**Netty** — это асинхронный сетевой фреймворк, построенный на неблокирующем (non-blocking) вводе-выводе и реактивной модели. Netty более гибкий и эффективный, когда дело касается асинхронных запросов, так как может обрабатывать множество соединений с использованием ограниченного количества потоков. Netty использует пул потоков событий (event loop), в котором потоки выполняют обработку событий и взаимодействие с сетевыми сокетами. Число потоков в event loop обычно соответствует количеству процессоров (можно задать с помощью `availableProcessors`). То есть для большинства приложений требуется от 2 до 4 потоков на каждый event loop, что значительно уменьшает нагрузку по сравнению с Tomcat.

**Что такое event loop?**
Это реактивная асинхронная модель программирования для серверов. Она позволяет достичь более высокого уровня параллелизма при меньшем количестве потоков.
По сути, Event Loop - это реализация шаблона Reactor. Является неблокирующим потоком ввода-вывода, который работает непрерывно. Его основная задача — проверка новых событий. И как только событие пришло перенаправлять его тому, кто в данный момент может его обработать. Иногда их может быть несколько для увеличения производительности.

**Как работает Event Loop?**
1. Event loop запускается и ждёт события — это может быть новое подключение, запрос, завершение асинхронной операции или таймер.
2. Когда событие происходит, оно добавляется в очередь задач (tasks queue), которую обрабатывает event loop.
3. Event loop забирает задачу из очереди и выполняет её, при этом переключаясь между задачами и обрабатывая их по очереди. Это происходит быстро, без блокировки основного потока.
4. После обработки очередной задачи event loop снова ожидает следующих событий.
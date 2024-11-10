Назад к: [[Kotlin]]

---
## Содержание:

Общая информация
Ключевые моменты
Основные понятия
Как работает
Возможные вопросы

---
## Общая информация:

**Корутины** – это конструкция для написания асинхронного кода, которая позволяет выполнять задачи параллельно или управлять ими без блокировки главного потока. Они дают возможность приостановить и возобновить выполнение кода, чтобы более эффективно использовать ресурсы.

---
## Ключевые моменты:
- **Легковесность**: корутины менее ресурсоемки, чем потоки, и могут выполняться параллельно, что экономит ресурсы.
- **Асинхронность без блокировки**: корутины позволяют выполнять задачи без блокирования основного потока.
- **Приостановка и возобновление**: корутины можно приостанавливать и возобновлять в нужные моменты, управляя выполнением задач.
- **Обработка ошибок**: корутины предоставляют средства для удобной обработки исключений в асинхронном коде.

---
## Основные понятия:
- **Suspend function**: функция, которая может быть приостановлена и возобновлена. Используется для выполнения долгих операций без блокировки основного потока.
- **Coroutine Scope**: область, в которой создаются и управляются корутины. Определяет жизненный цикл корутин и позволяет отменить все корутины, запущенные в данном контексте.
- **Coroutine Context**: набор параметров, который включает `Dispatcher` и `Job`. Контекст управляет средой выполнения корутины, определяя такие аспекты, как поток или пул потоков.
- **Dispatcher**: определяет, на каком потоке или пуле потоков будет выполняться корутина. Например, `Dispatchers.IO` используется для операций ввода-вывода, `Dispatchers.Main` — для обновления UI в основном потоке.
- **Job**: дескриптор выполнения корутины, позволяющий управлять её состоянием, отменять её или проверять статус. `Job` является частью `Coroutine Context`.
- **Builder (Конструктор корутин)**: функции, которые инициализируют корутины, такие как `launch` и `async`. Они определяют, как будет запускаться корутина и каким будет её результат. Например, `launch` запускает корутину и возвращает `Job`, а `async` возвращает `Deferred`, который позволяет получить результат асинхронной операции.

---
## Как работает:
Корутина запускается в определенном `CoroutineScope` с помощью `builder`, такого как `launch` или `async`. `Builder` создаёт корутину и добавляет её в `Coroutine Scope`, который управляет её жизненным циклом. При запуске корутины ей назначается `Coroutine Context`, включающий `Dispatcher` (для определения потока) и `Job` (для управления её состоянием).
Во время выполнения, если корутина достигает `suspend`-функции, она приостанавливается, освобождая текущий поток и позволяя другим задачам использовать ресурсы. `Job` корутины отслеживает её состояние (например, активно ли выполнение, приостановлено или отменено). Как только выполнение корутины возобновляется, она может продолжить работу на том же потоке или переключиться на другой, если так указано в `Dispatcher`.
Если `Coroutine Scope` отменяется, все корутины в этом контексте также завершаются через отмену их `Job`, что помогает управлять их жизненным циклом и предотвращает утечки ресурсов.

---
## Возможные вопросы:


---
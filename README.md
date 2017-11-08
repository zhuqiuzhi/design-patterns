Go 语言设计模式集合 

面试中最常被问到的设计模式是：[单例模式](/creational/singleton.md)，[工厂模式](/creational/factory.md)

## Creational Patterns

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [Abstract Factory](abstract_factory.md) | Provides an interface for creating families of releated objects |  ✘|
| [Builder](/creational/builder.md) | Builds a complex object using simple objects | ✘ |
| [工厂方法](/creational/factory.md) | 定义一个创建对象的方法，但是使用不同的但实现了同一接口的类的构建方法 | ✔ |
| [Builder](/creational/builder.md) | Builds a complex object using simple objects | ✘ |
| [Object Pool](/creational/object-pool.md) | Instantiates and maintains a group of objects instances of the same type | ✘ |
| [单例模式](/creational/singleton.md) | 限制一个类型只能有一个对象 | ✔ |

## Structural Patterns

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [Bridge](/structural/bridge.md) | Decouples an interface from its implementation so that the two can vary independently | ✘ |
| [Composite](/structural/composite.md) | Encapsulates and provides access to a number of different objects | ✘ |
| [Decorator](/structural/decorator.md) | Adds behavior to an object, statically or dynamically | ✘ |
| [Facade](/structural/facade.md) | Uses one type as an API to a number of others | ✘ |
| [Flyweight](/structural/flyweight.md) | Reuses existing instances of objects with similar/identical state to minimize resource usage | ✘ |
| [Proxy](/structural/proxy.md) | Provides a surrogate for an object to control it's actions | ✘ |

## Behavioral Patterns

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [Chain of Responsibility](/behavioral/chain_of_responsibility.md) | Avoids coupling a sender to receiver by giving more than object a chance to handle the request | ✘ |
| [Command](/behavioral/command.md) | Bundles a command and arguments to call later | ✘ |
| [Mediator](/behavioral/mediator.md) | Connects objects and acts as a proxy | ✘ |
| [Memento](/behavioral/memento.md) | Generate an opaque token that can be used to go back to a previous state | ✘ |
| [Observer](/behavioral/observer.md) | Provide a callback for notification of events/changes to data | ✘ |
| [Registry](/behavioral/registry.md) | Keep track of all subclasses of a given class | ✘ |
| [State](/behavioral/state.md) | Encapsulates varying behavior for the same object based on its internal state | ✘ |
| [Strategy](/behavioral/strategy.md) | Enables an algorithm's behavior to be selected at runtime | ✘ |
| [Template](/behavioral/template.md) | Defines a skeleton class which defers some methods to subclasses | ✘ |
| [Visitor](/behavioral/visitor.md) | Separates an algorithm from an object on which it operates | ✘ |

## Synchronization Patterns

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [Condition Variable](/synchronization/condition_variable.md) | Provides a mechanism for threads to temporarily give up access in order to wait for some condition | ✘ |
| [Lock/Mutex](/synchronization/mutex.md) | Enforces mutual exclusion limit on a resource to gain exclusive access | ✘ |
| [Monitor](/synchronization/monitor.md) | Combination of mutex and condition variable patterns | ✘ |
| [Read-Write Lock](/synchronization/read_write_lock.md) | Allows parallel read access, but only exclusive access on write operations to a resource | ✘ |
| [Semaphore](/synchronization/semaphore.md) | Allows controlling access to a common resource | ✘ |

## Concurrency Patterns

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [N-Barrier](/concurrency/barrier.md) | Prevents a process from proceeding until all N processes reach to the barrier | ✘ |
| [Bounded Parallelism](/concurrency/bounded_parallelism.md) | Completes large number of independent tasks with resource limits | ✘ |
| [Broadcast](/concurrency/broadcast.md) | Transfers a message to all recipients simultaneously | ✘ |
| [Coroutines](/concurrency/coroutine.md) | Subroutines that allow suspending and resuming execution at certain locations | ✘ |
| [Generators](/concurrency/generator.md) | Yields a sequence of values one at a time | ✘ |
| [Reactor](/concurrency/reactor.md) | Demultiplexes service requests delivered concurrently to a service handler and dispatches them syncronously to the associated request handlers | ✘ |
| [Parallelism](/concurrency/parallelism.md) | Completes large number of independent tasks | ✘ |
| [Producer Consumer](/concurrency/producer_consumer.md) | Separates tasks from task executions | ✘ |

## Messaging Patterns

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [Fan-In](/messaging/fan_in.md) | Funnels tasks to a work sink (e.g. server) | ✘ |
| [Fan-Out](/messaging/fan_out.md) | Distributes tasks among workers (e.g. producer) | ✘ |
| [Futures & Promises](/messaging/futures_promises.md) | Acts as a place-holder of a result that is initially unknown for synchronization purposes | ✘ |
| [Publish/Subscribe](/messaging/publish_subscribe.md) | Passes information to a collection of recipients who subscribed to a topic | ✘ |
| [Push & Pull](/messaging/push_pull.md) | Distributes messages to multiple workers, arranged in a pipeline | ✘ |

## Stability Patterns

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [Bulkheads](/stability/bulkhead.md)  | Enforces a principle of failure containment (i.e. prevents cascading failures) | ✘ |
| [Circuit-Breaker](/stability/circuit-breaker.md) | Stops the flow of the requests when requests are likely to fail | ✘ |
| [Deadline](/stability/deadline.md) | Allows clients to stop waiting for a response once the probability of response becomes low (e.g. after waiting 10 seconds for a page refresh) | ✘ |
| [Fail-Fast](/stability/fail_fast.md) | Checks the availability of required resources at the start of a request and fails if the requirements are not satisfied | ✘ |
| [Handshaking](/stability/handshaking.md) | Asks a component if it can take any more load, if it can't, the request is declined | ✘ |
| [Steady-State](/stability/steady_state.md) | For every service that accumulates a resource, some other service must recycle that resource | ✘ |

## Profiling Patterns

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [Timing Functions](/profiling/timing.md) | Wraps a function and logs the execution | ✘ |

## Idioms

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [Functional Options](/idiom/functional-options.md) | Allows creating clean APIs with sane defaults and idiomatic overrides | ✘ |

## Anti-Patterns

| Pattern | Description | Status |
|:-------:|:----------- |:------:|
| [Cascading Failures](/anti-patterns/cascading_failures.md) | A failure in a system of interconnected parts in which the failure of a part causes a domino effect | ✘ |

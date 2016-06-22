---
layout: post
title:  TRANSACTION DSL 关键字入门
date:   2016-06-22
categories:
- Coding
tags:
- dsl
---


--------

> * 本文仅描述DSL关键字的定义与基本用途，是一份简介
> * 大部分内容摘选自袁英杰的《事务模型：TRANSACTION DSL（一份并不简短的简介）》，了解更多内容请参考原文

<br>

## 1. DSL有哪些关键字

 > * 基本操作： asyn / sequential / concurrent / timer_prot
 > * 过程控制： optional / procedure / prot_procedure
 > * 片段： def / apply / params/with
 > * 循环： loop0 / loop1 
 > * 多线程：fork / join
 > * 中止： stop / interrupt
 > * 事件： wait / peek
 > * 过程监控： TransactionListener / with_id

<br>

## 2. DSL关键字的详细描述
 
### 2.1 基本操作

> **sync**：任何无需进一步等待后续消息的Action，包括函数调用，以及只发消息、不等回应的操作等。为了更加清晰的区分究竟一个同步操作属于哪种具体类型，可以使用 __call 来说明一个操作是函数调用，用 __ind 来说明这是一个发送指示消息的操作，用 __rsp 来说明一个操作是对之前某个请求的一个应答。

> **asyn**：任何需要等待异步消息的操作，包括典型的请求－应答操作，消息触发的操作等。同样的，可以使用一些更加具体的修饰来明确一个异步操作的类型，比如可以使用__req 来说明这是一个消息触发型的请求操作。

```c++
// 同步与异步样例
__def_transaction
( __sequential
    ( __req(Action1)
    , __call(Action2)
    , __asyn(Action3)
    , __asyn(Action4)
    , __rsp(Action5))
) Transaction;
```
<br>

> **sequential**：多个操作顺序执行。

> **concurrent**：多个操作并发执行。

```c++
// 顺序与并发样例
__def_transaction
( __sequential
    ( __req(Action1)
    , __call(Action2)
    , __concurrent(__asyn(Action3), __asyn(Action4))
    , __rsp(Action5))
) Transaction;
```
<br>

> **timer_prot**：描述异步过程可以带有时间约束。

```c++
// timer_prot样例
const TimerId TIMER_1 = 1;
const TimerId TIMER_2 = 2;

__def_transaction
( __timer_prot(TIMER_1, __sequential
    ( __req(Action1)
    , __call(Action2)
    , __timer_prot(TIMER_2, __concurrent(__asyn(Action3), __asyn(Action4)))
    , __rsp(Action5)))
) Transaction;
```
<br>

### 2.2 过程控制： optional / procedure / prot_procedure

在一个较为复杂的事务中，一些操作只有在一定的条件下才会执行；或者，在不同的条件下执行的时机不同。这就需要使用到过程控制。

> **optional**：只有在一定的条件下才会执行这个操作。

```c++
// optional样例1
struct ShouldExecAction2
{
    bool operator()(const TransactionInfo&) const
    {
        return SystemConfig::shouldExecAction2();
    }
};

__def_transaction
( __sequential
    ( __req(Action1)
    , __optional(ShouldExecAction2, __asyn(Action2))
    , __asyn(Action3)
    , __asyn(Action4)
    , __rsp(Action5))
) Transaction;

// optional样例2：更复杂的路径选择
/* 首先获取到InstanceId，知道当前的Transaction属于哪个实例，通过Transaction获取到自身的运行时状态。
*/
__def_transaction
( __sequential
    ( __req(Action1)
    , __optional(IsAction2RunFirst, __asyn(Action2))
    , __asyn(Action3)
    , __optional(__not(IsAction2RunFirst), __asyn(Action3))
    , __rsp(Action5))
) Transaction;
```
<br>

> **procedure**：定义一个过程，无论这个过程中的所有操作全部成功，还是执行到某一步时发生了失败，都会进入结束模式。

```c++
// procedure样例
/* 在这个事务中，如果 Action2 发生了错误，那么 Action3 将会得到执行，然后会跳过 Action4 和 Action5，直接进入外层过程的__finally，执行 Action6。
*/
__def_transaction
( __procedure
    ( __sequential
        ( __asyn(Action1)
        , __procedure
            ( __asyn(Action2)
            , __finally(__sync(Action3)))
        , __sync(Action4)
        , __asyn(Action5))
    , __finally(__sync(Action6)))
) Transaction;
```
<br>

> **prot_procedure**：和 __procedure 不同的是，如果 __prot_procedure 的 __finally 里定义的操作执行成功，则整个操作将重新转入成功状态。

```c++
// prot_procedure样例
/* 在这个事务中，如果 Action1 发生失败，则会跳过 Action2，转入执行 Action3；如果 Action3 执行成功，则会继续执行 Action4 及后续过程。
*/
__def_transaction
( __prot_procedure
    ( __sequential
        ( __asyn(Action1)
        , __prot_procedure
            ( __asyn(Action2)
            , __finally(__sync(Action3)))
        , __sync(Action4)
        , __asyn(Action5))
    , __finally(__sync(Action6)))
) Transaction;
```
<br>

### 2.3 片段： def / apply / params / with

良好软件设计的核心原则之一就是清晰性，满足清晰性的一个重要措施就是允许用户定义更小的，职责更加单一的小函数或者小类。
对于 Transaction DSL 而言，同样可以将一个复杂过程的局部进行提取，以定义更小的，职责更加单一的子操作： 片段（Fragment）。

> **def**： 定义一个片段。

```c++
// def样例
__def(Fragment) __as
( __sequential
    ( __asyn(Action1)
    , __asyn(Action2))
);
```
<br>

> **apply**：定义一个片段后，可以在另外一段操作中调用它。

```c++
// apply样例1
__def_transaction
( __sequential
    ( __asyn(Action1)
    , __apply(Fragment))
) Transaction;

// apply样例2：片段是procedure
__def(Fragment1) __as_procedure
( __sequential
    ( __asyn(Action1)
    , __asyn(Action2))
, __finally(__rsp(Action3))
);

__def(Fragment2) __as_procedure
( __asyn(Action5)
, __finally(__sync(Action6))
);

__def_transaction
( __sequential
    ( __apply(Fragment1)
    , __asyn(Action4)
    , __apply(Fragment2))
) Transaction;
```
<br>

> **params**：两段操作相似但是细节不同，可以合二为一，将差异参数化。

```c++
// params样例
__timer_prot(TIMER_1, __sequential
                        ( __asyn(Action2)
                        , __asyn(Action3)))

__timer_prot(TIMER_2, __sequential
                        ( __asyn(Action4)
                        , __asyn(Action3)))

__def(Seq2, __params(__timer_id(TIMER), __action(ACTION))) __as
( __timer_prot(TIMER, __sequential
                        ( __asyn(ACTION)
                        , __asyn(Action3)))
);
```
<br>

> **with**： 使用 __apply调用一个带参数的片段，通过__with 来指明实参。

```c++
// with样例
__def_transaction
( __sequential
    ( __apply(Seq2, __with(TIMER_1, Action2))
    , __asyn(Action4))
) Transaction1;

__def_transaction
( __concurrent
    ( __asyn(Action1)
    , __apply(Seq2, __with(TIMER_2, Action4)))
) Transaction2;
```
<br>

### 2.4 循环： loop0 / loop1 

有的场景，需要循环的执行一些过程，直到某个条件不再满足。这就需要使用到循环控制。

> **loop0**：在循环开始之前就会执行一次谓词判断。

```c++
// loop0样例： 类似于while (pred) action;
struct HasMoreContents
{
    bool operator()(const TransactionInfo& info) const
    {
        return (not info.threadFailed()) and FileSender::hasMoreBlocks();
    }
};

__def_transaction
( __sequential
    ( __asyn(StartFileTransfer)
    , __loop0(HasMoreContents, __asyn(FileContentTransfer)))
) Transaction;
```
<br>

> **loop1**：在执行了一次操作之后，才进行谓词判断，以决定循环是否继续。

```c++
// loop1样例： 类似于 do { action; } while (pred);
struct ShouldRetrans
{
    ShouldRetrans() : retransTimes(0) {}
    bool operator()(const TransactionInfo& info) const
    {
        return (info.getAnyFailure() == TIMEDOUT) and ((retransTimes++) < 2);
    }
private:
unsigned int retransTimes;
};

__def(ContentSending) __as
( __loop1(ShouldRetrans, __timer_prot(TIMER_1, __asyn(FileContentTransfer)))
);
```
<br>

### 2.5 多线程： fork / join

新加入的操作需要脱离本来的控制序列，独自形成另外一条控制序列。
那么，如果我们将一条控制序列看作一个线程，我们已经在面对多线程的问题。

> **fork**：需要新增一条控制序列，可以创建一条新**操作线程** (Action	Thread)。

```c++
// fork样例1
const ActionThreadId ACTION7_THREAD = 1;
__def_transaction
(   // …
    __fork(ACTION7_THREAD, __asyn(Action7))
    // …
) Transaction1;

// fork样例2: 复杂的操作
__def_transaction
(   // …
    __fork(THREAD1, __sequential
                    ( __concurrent(__asyn(Action1), __asyn(Action2))
                    , __asyn(Action2)
                    , __timer_prot(TIMER2, __asyn(Action3)))
    // …
) Transaction2;
```
<br>

> **join**： 在__join 处一直等待，直到目标线程运行结束。

```c++
// join样例
const ActionThreadId ACTION7_THREAD = 1;

__def_tranaction
( __sequential
    ( __req(Action1)
    , __concurrent
        ( __sequential
            ( __asyn(Action2)
            , __fork(ACTION7_THREAD, __asyn(Action7)))
        , __asyn(Action3))
    , __asyn(Action4)
    , __join(ACTION7_THREAD)
    , __asyn(Action5)
    , __rsp(Action6))
) Transaction;
```
<br>

### 2.6 中止： stop / interrupt

当一个事务内部处理错误，或被外部事务抢占，需要中止事务的运行。
如果原因来自于内部，事务知道这一切，可以主动终止；但如果原因来自于外部，而事务内部一切正常，那么事务则对将要发生的事情一无所知。
所以必须提供外部接口，以通过外部的调用被动的终止运行。

> **stop**： 一旦一个事务的 stop 接口被调用，它就马上转入结束模式。在结束模式下，整个事务中所有并行运行的当前操作都会被中止，无论这个并行操作是一个有名线程还是一个匿名线程。

```c++
// stop样例1
/* 在下面的事务中，如果事务正出于 Action2， Action3 和 Action4 的并发阶段，其中， Action3 已经运行。此时如果对此事务执行 stop 操作，将会马上中止 Action2 和 Action4 的运行（Action2 和 Action4 的 stop 接口将会被调用），然后结束整个事务。
*/

__def_transaction
( __sequential
    ( __req(Action1)
    , __concurrent(__asyn(Action2), __asyn(Action3), __asyn(Action4))
    , __asyn(Action5))
) Transaction;

// stop样例2：procedure 的 stop
/* 对于下面的事务，如果系统正在 Action1，当事务的 stop 接口被调用时，则会首先调用Action1 的 stop 接口，然后，转入执行Action3，等它完成之后，整个事务将结束运行。
*/

__def_transaction
( __sequential
    ( __procedure
        ( __sequential
            ( __asyn(Action1)
            , __asyn(Action2))
        , __finally(__rsp(Action3)))
    , __asyn(Action4))
) Transaction;

// stop样例3： 线程的 stop
/* 存在两个线程——主线程和 THREAD1。假设，当THREAD1 正在执行 Action5，而主线程正在执行 Action1 时。
   突然，外部调用了stop，那么 Action5 和 Action1 的 stop 将会马上被调用，然后 THREAD1 将马上转入 Action6，而主线程则转入执行 Action3。
*/

__def(thread1) __as
( __fork(THREAD1,
    __procedure
        ( __asyn(Action5)
        , __finally(__asyn(Action6))))
);

__def_transaction
( __sequential
    ( __apply(thread1)
    , __procedure
        ( __sequential
            ( __asyn(Action1)
            , __asyn(Action2))
        , __finally(__rsp(Action3)))
    , __asyn(Action4))
) Transaction;

// stop样例4： 匿名线程的 stop
/* 假如，在 stop 时， Anon1 正在运行 Action1，Anon2 正在运行 Action3。
   那么， stop 调用将会马上出发 Action1 和 Action3 的 stop 方法被调用，然后各自转入 Action2 和 Action4；等它们都运行结束后，再转入主线程的 __finally 里的 Action6。
   等它结束之后，整个事务才运行结束。
*/

__def(Anon1) __as
( __procedure
    ( __asyn(Action1)
    , __finally(__asyn(Action2)))
);

__def(Anon2) __as
( __procedure
    ( __asyn(Action3)
    , __finally(__asyn(Action4)))
);

__def_transaction
( __procedure
    ( __concurrent
        ( __apply(Anon1)
        , __apply(Anon2))
    , __asyn(Action5)
    , __finally(__asyn(Action6)))
) Transaction;

// stop样例5： 结束模式中的 stop
/* 如果外围调用其 stop 接口时，其已经在运行 Action2，那么 stop 操作将对其不产生作用。
*/

__def_transaction
( __procedure
    ( __asyn(Action1)
    , __finally(__asyn(Action2)))
) Transaction;
```
<br>

> **interrupt**： interrupt 会立即中止所有正在运行的操作， 基本操作的 stop 接口同样会得到调用，但不同的是，如果一个操作是__procedure，则不会转入其 __finally。

```c++
// interrupt样例1
/* 在下面的事务中，当 Action1 正在运行时，如果 interrupt 整个事务，则 Action1 的 stop 会得到调用，然后，马上结束整个事务。
   Action3 并不会像 stop 时那样得到调用。
*/

__def_transaction
( __sequential
    ( __procedure
        ( __sequential
            ( __asyn(Action1)
            , __asyn(Action2))
        , __finally(__rsp(Action3)))
    , __asyn(Action4))
) Transaction;


// interrupt样例2：结束模式中的 interrupt
/* 在下面的事务中，如果它正在运行 Action2 时，收到了 interrupt 操作，此时 Action2 的 stop 方法会立即得到调用，然后整个事务运行结束。
*/

__def_transaction
( __procedure
    ( __asyn(Action1)
    , __finally(__asyn(Action2)))
) Transaction;

```
<br>

### 2.7 事件： wait / peek

正常情况下，对于具体的事件 Transaction DSL 层面不应该关注，它需要做的就是将收到事件派发给由用户编写的基本操作。
但在某些场景下，将对具体事件的关注放在 Transaction	DSL 内，可以大大方便用户的实现。

> **wait**： 当一个事务执行到某个点的时候，会期待某个事件的发生，但是 事件的内容并不重要（或干脆没有内容）。

```c++
// wait样例
__def_transaction
(   ...
    __wait(EV_SOMETHING)
    ...
) Transaction;
```
<br>

> **peek**： 来探测一个消息是否到达但它并不处理这个消息，调度器还会进一步将消息传递给后续的流程。

```c++
// peek样例
__def(Trans2) __as
( __sequential
    ( __sync(Action2)
    , __asyn(Action3))
) ;

__def_transaction
( __sequential
    ( __timer_prot(TIMER_1, __sequential
                            ( __sync(Action1)
                            , __peek(EV_FOO)))
    , __apply(Trans2))
) Transaction1;

__def_transaction
( __apply(Trans2)
) Transaction2;
```
<br>

### 2.8 过程监控： TransactionListener / with_id

在一个现实的系统中，对于单个事务的执行过程往往有各种类型的追踪，将相关的代码都放入每个操作会严重污染操作的实现。
从单一职责的角度，这种需求可以从具体的业务代码中分离出去，通过观察者模式实现。

> **TransactionListener**：通过观察者模式，执行对单个事务不同过程的监控，实现代码解耦。

```c++
// TransactionListener样例
struct TransactionListener
{
    virtual void onActionStarting(const ActionId) {}
    virtual void onActionStarted(const ActionId) {}
    virtual void onActionEventConsumed(const ActionId, const Event&) {}
    virtual void onActionDone(const ActionId, const Status) {}
    
    virtual void onActionStartStopping(const ActionId, const Status) {}
    virtual void onActionStoppingStarted(const ActionId) {}
    virtual void onActionEventConsumed(const ActionId, const Event&) {}
    virtual void onActionStopped(const ActionId, const Status) {}
    
    virtual void onActionInterrupted(const ActionId) {}
};
```
<br>

观察者接口被调用的时机如下表所示：  
| 方法       | 执行时机   |
| --------   | -----:  | 
|onActionStarting | 一个操作开始执行之前（exec 被调用之前） |
|onActionStarted | 一个操作的 exec 函数返回 CONTINUE |
|onActionEventConsumed | 一个事件被 handleEvent 接受（无论其返回 SUCCESS， CONTINUE 还是失败） |
|onActionDone | 一个操作已经运行结束（无论是成功还是失败） |
|onActionStartStopping | 一个操作开始停止之前（stop 被调用之前） |
|onActionStoppingStarted | 一个操作的 stop 函数返回 CONTINUE |
|onActionEventConsumed | 一个事件在 stop 过程中被 handleEvent 接受之后（无论其返回 SUCCESS， CONTINUE 还是失败） |
|onActionStopped | 一个操作已经被中止（无论成功或失败） |
|onActionInterrupted | 一个操作已经被打断 |

> **with_id**： 由用户自己指定选择观察哪一个操作。

```c++
// with_id样例1
const ActionId ID_ACTION1 = 1;
const ActionId ID_ACTION4 = 2;
__def_transaction
( __sequential
    ( __procedure
        ( __sequential
            ( __with_id(ID_ACTION1,__asyn(Action1))
            , __asyn(Action2))
        , __finally(__rsp(Action3)))
    , __with_id(ID_ACTION4, __asyn(Action4)))
) Transaction;

// with_id样例2：任意的粒度
__def_transaction
( __with_id( ID_TRANS,
    __sequential
        ( __procedure
            ( __sequential
                ( __asyn(Action1)
                , __asyn(Action2))
            , __finally(__rsp(Action3)))
        , __asyn(Action4)))
) Transaction;

// with_id样例3：嵌套
__def_transaction
( __with_id( ID_TRANS,
    __sequential
        ( __procedure
            ( __with_id( ID_SEQ,
                __sequential
                    ( __asyn(Action1)
                    , __asyn(Action2)))
            , __finally(__rsp(Action3)))
    , __asyn(Action4)))
) Transaction;
```
<br>

## 3. DSL关键字的使用注意事项

### 3.1 fork的使用陷阱
使用fork时Trans需要使用multithread进行包装。
<br>

### 3.2 stop与intertupt的差异

stop 会导致事务进入结束模式，在结束模式下，如果一个正在运行的操作
是 __procedure，则会转入其 __finally 中的操作，而这些可能是异步的，从而导致一个事务必须花更长时间才可以中止。
但 interrupt 会立即中止所有正在运行的操作， 基本操作的 stop 接口同样会得到调用，但不同的是，如果一个操作是 __procedure，则不会转入其 __finally。
<br>
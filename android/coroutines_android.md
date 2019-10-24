# Coroutines for Android

요번에 새로 코루틴을 쓰게되었다. 그래서 공부를 해야한다 ٩(•̀ᴗ•́*)و

## Intro

Unlike many other languages with similar capabilities, async and await are not keywords in Kotlin and are not even part of its standard library. Moreover, Kotlin's concept of suspending function provides a safer and less error-prone abstraction for asynchronous operations than futures and promises.

참고로 공통 플랫폼에서는 `org.jetbrains.kotlinx:kotlinx-coroutines-core:x.x.x`를 쓰지만 난 안드로이드를 하니까 `org.jetbrains.kotlinx:kotlinx-coroutines-android:x.x.x`를 사용한다.
뭔가 다른지는 [여기](https://github.com/Kotlin/kotlinx.coroutines#android)!

## Android Basic Usecases

```kotlin
class MainActivity : AppCompatActivity() {
    private val supervisor = SupervisorJob()
    private val scope = CoroutineScope(Dispatchers.Main + supervisor)

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContentView(R.layout.activity_main)

        val job = scope.launch {
            delay(5000)
            Toast.makeText(applicationContext, "Hello", Toast.LENGTH_SHORT).show()
        }
        job.invokeOnCompletion { error ->
            Toast.makeText(applicationContext, "Cancelled with error: ${error?.cause}", Toast.LENGTH_SHORT).show()
        }
    }

    override fun onStop() {
        super.onStop()
        supervisor.cancelChildren()
    }
}
```

요것이 안드로이드에서의 코루틴의 아주 간단한 형태인데 크게 2개의 중요 개념이 있다. **Job**과 **Scope**.
`scope`를 `CoroutineScope(Dispatchers.Main + supervisor)`로 정의했고, 요 `scope`객체를 사용해서 백그라운드에서 실행될 작업을 `scope.launch`로 발동시키는데,
`Dispatchers.Main`을 사용했기 때문에 `launch`안에서 도는 코드들은 메일 쓰레드에서 돌 것이고, `+ supervisor`를 정의했기 때문에
`onStop()`에서 보이듯이 `supervisor.cancelChildren()`을 하면 해당 `scope`를 이용해 실행된 작업들은 모두 캔슬될것이다.

위 코드 상으로는 5초동안 액티비티를 안끄면 Hello 라는 토스트가 뜰 것이고, 그 전에 종료하면 캔슬이되면서 `job.invokeOnCompletion`이 호출되어 Cancelled with error 어쩌구가 불릴것이다.

보다보니 알듯하구만.

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        runBlocking {
            launch {
                delay(300)
                Timber.d("Hansol")
            }

            coroutineScope {
                launch {
                    delay(500)
                    Timber.d("Yelyn")
                }

                delay(100)
                Timber.d("Mama")
            }

            Timber.d("Papa")
        }
    }
}
```

요건 어떤 순서로 불릴라나.

근데 참고로 이거처럼하면 이중 하나 딜레이가 몇초 이상이면 그 시간동안 UI는 블러킹됨.

```kotlin
class MainActivity : AppCompatActivity() {
    private val supervisor = SupervisorJob()
    private val scope = CoroutineScope(Dispatchers.Main + supervisor)

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        scope.launch {
            delayFunction()
        }
    }

    private suspend fun delayFunction(): Int = withContext(Dispatchers.IO) {
        Single.just(5).delay(5, TimeUnit.SECONDS).blockingGet()
    }

    override fun onStop() {
        super.onStop()
        supervisor.cancelChildren()
    }
}
```

suspend 함수를 위처럼 따로 뺄 수도 (당연히) 있는데, RxJava의 Single이나 아니면 그냥 `return 5`와 같은 코드로 구현하고 싶을 땐
`withContext`를 쓰면 되나부다. 아직 연구가 더 필요함.

## Cancelling coroutine

이제부턴 문서다.

```kotlin
val job = launch {
    repeat(1000) { i ->
        println("job: I'm sleeping $i ...")
        delay(500L)
    }
}
delay(1300L) // delay a bit
println("main: I'm tired of waiting!")
job.cancel() // cancels the job
job.join() // waits for job's completion 
println("main: Now I can quit.")
```

`job`이 나오는걸 받아서 `cancel()`해주면 된다. 근데 이게 마법처럼 자동으로 캔슬되는건 아니고, 저 `repeat`이라는 함수가 캔슬될 때의 행동을 잘 구현해놔서 되는거다.

그래서 아래와 같은 경우는 `cancel()`을 때려도 멈추지 않는다.

```kotlin
val startTime = System.currentTimeMillis()
val job = launch(Dispatchers.Default) {
    var nextPrintTime = startTime
    var i = 0
    while (i < 5) { // computation loop, just wastes CPU
        // print a message twice a second
        if (System.currentTimeMillis() >= nextPrintTime) {
            println("job: I'm sleeping ${i++} ...")
            nextPrintTime += 500L
        }
    }
}
delay(1300L) // delay a bit
println("main: I'm tired of waiting!")
job.cancelAndJoin() // cancels the job and waits for its completion
println("main: Now I can quit.")
```

위 코드가 멈출 수 있게 하려면 아래와 같이 수정해야함:

```kotlin
val startTime = System.currentTimeMillis()
val job = launch(Dispatchers.Default) {
    var nextPrintTime = startTime
    var i = 0
    while (isActive) { // cancellable computation loop
        // print a message twice a second
        if (System.currentTimeMillis() >= nextPrintTime) {
            println("job: I'm sleeping ${i++} ...")
            nextPrintTime += 500L
        }
    }
}
delay(1300L) // delay a bit
println("main: I'm tired of waiting!")
job.cancelAndJoin() // cancels the job and waits for its completion
println("main: Now I can quit.")
```

즉, `job.cancel()`이 호출되면 `isActive`값이 `false`가 되기 때문에 이를 체크해줘야한다. 안과 밖이 서로 협동을 해야한다.

호오.. 이렇게도 cancelling을 핸들링할수도있다:

```kotlin
val job = launch {
    try {
        repeat(1000) { i ->
            println("job: I'm sleeping $i ...")
            delay(500L)
        }
    } finally {
        println("job: I'm running finally")
    }
}
delay(1300L) // delay a bit
println("main: I'm tired of waiting!")
job.cancelAndJoin() // cancels the job and waits for its completion
println("main: Now I can quit.")
```

```
Output:
job: I'm sleeping 0 ...
job: I'm sleeping 1 ...
job: I'm sleeping 2 ...
main: I'm tired of waiting!
job: I'm running finally
main: Now I can quit.
```

`job.cancel()`을 하면 저 블럭 안에 `CancellationException`이 불리기 때문에 바로 finally로 넘어가는 것이다.
주의할점은 `job.join()`만 하면 캔슬이 안된다는거.

명시적으로 timeout을 설정할 수도 있음:

```kotlin
withTimeout(1300L) {
    repeat(1000) { i ->
        println("I'm sleeping $i ...")
        delay(500L)
    }
}
```

```
Output:
I'm sleeping 0 ...
I'm sleeping 1 ...
I'm sleeping 2 ...
Exception in thread "main" kotlinx.coroutines.TimeoutCancellationException: Timed out waiting for 1300 ms
```

## Composing Suspending Functions

두 개 이상의 job을 sequentially가 아닌 concurrently하게 돌리고 싶을 땐 `async`를 사용한다.

```kotlin
suspend fun doSomethingUsefulOne(): Int {
    delay(1000L)
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    delay(1000L)
    return 29
}

val time = measureTimeMillis {
    val one = async { doSomethingUsefulOne() }
    val two = async { doSomethingUsefulTwo() }
    println("The answer is ${one.await() + two.await()}")
}
println("Completed in $time ms")
```

```
Output:
The answer is 42
Completed in 1017 ms
```

async를 안쓰고 돌렸다면 sequentially 하게 돌아서 `Completed in 2017 ms`가 나왔을 것이다.

Conceptually, async is just like launch. It starts a separate coroutine which is a light-weight thread that works concurrently with all the other coroutines. The difference is that launch returns a Job and does not carry any resulting value, while async returns a Deferred — a light-weight non-blocking future that represents a promise to provide a result later. You can use .await() on a deferred value to get its eventual result, but Deferred is also a Job, so you can cancel it if needed.

**Note that concurrency with coroutines is always explicit.** Rx와 비슷한 원칙이네.

Optionally, async can be made lazy by setting its start parameter to CoroutineStart.LAZY. In this mode it only starts the coroutine when its result is required by await, or if its Job's start function is invoked.

```kotlin
val time = measureTimeMillis {
    val one = async(start = CoroutineStart.LAZY) { doSomethingUsefulOne() }
    val two = async(start = CoroutineStart.LAZY) { doSomethingUsefulTwo() }
    // some computation
    one.start() // start the first one
    two.start() // start the second one
    println("The answer is ${one.await() + two.await()}")
}
println("Completed in $time ms")
```

So, here the two coroutines are defined but not executed as in the previous example, but the control is given to the programmer on when exactly to start the execution by calling start. We first start one, then start two, and then await for the individual coroutines to finish.

## Coroutine Context and Dispatchers

Coroutines always execute in some context represented by a value of the CoroutineContext type.
The coroutine context is a set of various elements. The main elements are the Job of the coroutine, which we've seen before, and its dispatcher.

A coroutine dispatcher determines what thread or threads the corresponding coroutine uses for its execution. It can confine coroutine execution to a specific thread, dispatch it to a thread pool, or let it run unconfined.

All coroutine builders like launch and async accept an optional CoroutineContext parameter that can be used to explicitly specify the dispatcher for the new coroutine and other context elements.

```kotlin
launch(newSingleThreadContext("MyOwnThread")) { // will get its own new thread
    println("newSingleThreadContext: I'm working in thread ${Thread.currentThread().name}")
}
```

`newSingleThreadContext` creates a thread for the coroutine to run. A dedicated thread is a very expensive resource. In a real application it must be either released, when no longer needed, using the close function, or stored in a top-level variable and reused throughout the application.

The coroutine's Job is part of its context, and can be retrieved from it using the `coroutineContext[Job]` expression:

```kotlin
println("My job is ${coroutineContext[Job]}")
```

```
Output:
My job is "coroutine#1":BlockingCoroutine{Active}@6d311334
```

Note that `isActive` in `CoroutineScope` is just a convenient shortcut for `coroutineContext[Job]?.isActive == true`.

Automatically assigned ids are good when coroutines log often and you just need to correlate log records coming from the same coroutine. However, when a coroutine is tied to the processing of a specific request or doing some specific background task, it is better to name it explicitly for debugging purposes. The CoroutineName context element serves the same purpose as the thread name. It is included in the thread name that is executing this coroutine when the debugging mode is turned on.

```kotlin
val v1 = async(CoroutineName("v1coroutine")) {
    delay(500)
    log("Computing v1")
    252
}
```

Sometimes we need to define multiple elements for a coroutine context. We can use the `+` operator for that. For example, we can launch a coroutine with an explicitly specified dispatcher and an explicitly specified name at the same time:

```kotlin
launch(Dispatchers.Default + CoroutineName("test")) {
    println("I'm working in thread ${Thread.currentThread().name}")
}
```

## Asynchronous Flow

Suspending functions asynchronously returns a single value, but how can we return multiple asynchronously computed values? This is where Kotlin Flows come in.

To represent the stream of values that are being asynchronously computed, we can use a Flow<Int> type just like we would the Sequence<Int> type for synchronously computed values:

```kotlin
fun foo(): Flow<Int> = flow { // flow builder
    for (i in 1..3) {
        delay(100) // pretend we are doing something useful here
        emit(i) // emit next value
    }
}

fun main() = runBlocking<Unit> {
    // Launch a concurrent coroutine to check if the main thread is blocked
    launch {
        for (k in 1..3) {
            println("I'm not blocked $k")
            delay(100)
        }
    }
    // Collect the flow
    foo().collect { value -> println(value) } 
}
```

```
Output:
I'm not blocked 1
1
I'm not blocked 2
2
I'm not blocked 3
3
```

Flows are cold streams similar to sequences — the code inside a flow builder does not run until the flow is collected

```kotlin
fun foo(): Flow<Int> = flow { 
    println("Flow started")
    for (i in 1..3) {
        delay(100)
        emit(i)
    }
}

fun main() = runBlocking<Unit> {
    println("Calling foo...")
    val flow = foo()
    println("Calling collect...")
    flow.collect { value -> println(value) } 
    println("Calling collect again...")
    flow.collect { value -> println(value) } 
}
```

```
Output:
Calling foo...
Calling collect...
Flow started
1
2
3
Calling collect again...
Flow started
1
2
3
```

This is a key reason the foo() function (which returns a flow) is not marked with suspend modifier. By itself, foo() returns quickly and does not wait for anything.

Handling cancellation in flow works like this:

```kotlin
fun foo(): Flow<Int> = flow { 
    for (i in 1..3) {
        delay(100)          
        println("Emitting $i")
        emit(i)
    }
}

fun main() = runBlocking<Unit> {
    withTimeoutOrNull(250) { // Timeout after 250ms 
        foo().collect { value -> println(value) } 
    }
    println("Done")
}
```

## Reference

- https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html
- https://github.com/Kotlin/kotlinx.coroutines

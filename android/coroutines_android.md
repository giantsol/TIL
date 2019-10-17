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

## Reference

- https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html
- https://github.com/Kotlin/kotlinx.coroutines

# Coroutines Patterns & Anti-Patterns

## 1. Wrap async calls with coroutineScope or use SupervisorJob to handle exceptions

```kotlin
val job: Job = Job()
val scope = CoroutineScope(Dispatchers.Default + job)
// may throw Exception
fun doWork(): Deferred<String> = scope.async { ... }
fun loadData() = scope.launch {
    try {
        doWork().await()
    } catch (e: Exception) { ... }
}
```

`doWork()`안에서 exception이 날 수 있다고 했을 때, 위처럼 try catch로 감싸도 여전히 크래시가 날 수 있다.
왜냐믄 저 코루틴이 크래시나면 그 코루틴이 포함된 job 전체가 크래시나기 때문이다.

### 해결책 (1): Job 대신 SupervisorJob을 사용

A failure or cancellation of a child does not cause the supervisor job to fail and does not affect its other children.
즉, 맨 위에 한 라인만 바꾸면 됨.

```kotlin
val job = SupervisorJob()
... same
```

엌.. 근데 이렇게 해도 아래처럼 하면 여전히 크래시가 난다고 한다.. 알거같으면서도 아직 완전히 와닿지는 않는 기분.

```kotlin
val job = SupervisorJob()                               
val scope = CoroutineScope(Dispatchers.Default + job)

fun loadData() = scope.launch {
    try {
        async {
            // may throw Exception 
        }.await()
    } catch (e: Exception) { ... }
}
```

이유는 `scope.async`로 SupervisorJob의 scope가 아닌 저 안의 scope에서 실행되기 때문에. 알 것 같긴하다.

### 해결책 (2): async를 또다른 CoroutineScope로 감싼다

```kotlin
val job = SupervisorJob()                               
val scope = CoroutineScope(Dispatchers.Default + job)

// may throw Exception
suspend fun doWork(): String = coroutineScope {
    async { ... }.await()
}

fun loadData() = scope.launch {
    try {
        doWork()
    } catch (e: Exception) { ... }
}
```

이러면 저 async 안에서 크래시가 나도 그를 감싸는 coroutineScope까지만 크래시가 나기 때문에 try catch에 성공적으로 걸림.
이 글 저자분은 이 두번째 해결책이 더 preferable하다고 하네. 연관있는 로직들끼리, 긍까 크래시가 났을 때 다같이 멈춰야하는 로직들끼리 coroutineScope로 묶어주는게 더 낫다는걸까?

## 2. Prefer the Main dispatcher for root coroutine

```kotlin
val scope = CoroutineScope(Dispatchers.Main)

fun login() = scope.launch {
    view.showLoading()    
    withContext(Dispatcher.IO) { networkClient.login(...) }
    view.hideLoading()
}
```

한마디로, view를 건드리는 로직이 있을 때 Dispatchers.Main외에 Dispatchers.Default 이런 다른걸 쓸 이유가 없다는거.

Dispatchers.Default로 scope를 만들었으면 view 건드릴때마다 `withContext(Dispatcher.Main) { view.showLoading() }` 이런 식으로 쓸데없는 컨텍스트 스위칭을 해야함.

## 3. Avoid usage of unnecessary async/await

이것도 간단

```kotlin
launch {
    val data = async(Dispatchers.Default) { /* code */ }.await() // X
    val data = withContext(Dispatchers.Default) { /* code */ } // O
}
```

## 4. Avoid cancelling scope job

이거 내가 아까 의문이들어서 김토미한테 질문했었던거네.

// todo: To be continued...

## Reference

https://proandroiddev.com/kotlin-coroutines-patterns-anti-patterns-f9d12984c68e

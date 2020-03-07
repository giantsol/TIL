# canDrawOverlays

다른 Application 위에 그릴 수 있는가!!

```kotlin
private fun canDrawOverlays(context: Context): Boolean {
    return if (Build.VERSION.SDK_INT < Build.VERSION_CODES.M) {
        true
    } else if (Build.VERSION.SDK_INT > Build.VERSION_CODES.O) {
        // Android 8.1 (API 27) and above has bug fixed!
        Settings.canDrawOverlays(context)
    } else {
        // between 6.0 (API 23) and 8.0 (API 26)
        if (Settings.canDrawOverlays(context)) {
            true
        } else {
            // deal with Android 8.0 (API 26) bug
            canDrawOverlaysWorkAround(context)
        }
    }
}

/**
 * Android O에서 "다른 앱 위에 그리기"를 on하고 onActivityResult()에서 권한 체크를 할 때
 * 타이밍 이슈로 Settings.canDrawOverlays()가 false를 리턴한다.
 * 해결책으로 transparent view를 넣었다 빼는 식으로 해서 성공하면 권한이 주어졌다는 뜻이므로 true,
 * 권한이 없어서 exception이 나면 false를 리턴하는 처리를 한다.
 * 참고: https://stackoverflow.com/questions/46173460
 */
private fun canDrawOverlaysWorkAround(context: Context): Boolean {
    try {
        val windowManager = context.getSystemService(Context.WINDOW_SERVICE) as WindowManager
        val view = View(context)
        val type = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            WindowManager.LayoutParams.TYPE_APPLICATION_OVERLAY
        } else {
            WindowManager.LayoutParams.TYPE_SYSTEM_ALERT
        }
        val params = WindowManager.LayoutParams(0, 0, type,
            WindowManager.LayoutParams.FLAG_NOT_TOUCHABLE or WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
            PixelFormat.TRANSPARENT)
        view.layoutParams = params
        windowManager.addView(view, params)
        windowManager.removeView(view)
        return true
    } catch (e: Exception) {
        return false
    }
}
```
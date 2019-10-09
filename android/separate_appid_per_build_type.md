# Separating applicationId per buildTypes

안드로이드는 각각의 앱들이 고유한 applicationId들을 가진다.
근데 개발하면서 테스트 하다 보면 릴리즈버전과 디버그버전을 하나의 폰에 나란히 깔아두고 하고 싶을 때가 있는데,
즉 buildType에 따라 appId를 다르게 설정해야 나란히 깔 수가 있다.

app/build.gradle 파일의 buildTypes 부분을 아래와 같이 추가하면 된다:

```groovy
buildTypes {
    debug {
        applicationIdSuffix '.debug'
        resValue "string", "app_name", "Blue Diary Debug"
    }

    release {
        resValue "string", "app_name", "Blue Diary"
        // ... other values
    }
}
```

위와 같이 설정하고 Manifest.xml 파일의 application 부분의 label도 아래와 같이 설정해주면 빌드타입에 따라 앱 이름도 다르게 설정할 수 있다:
```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="<<package name>>">

    <application
        android:label="@string/app_name"
```

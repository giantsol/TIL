# Developing Flutter Plugin with EventChannel

Flutter에서 Android, iOS별 네이티브 코드를 호출하기 위한 통로는 [MethodChannel](https://api.flutter.dev/flutter/services/MethodChannel-class.html)과 [EventChannel](https://api.flutter.dev/flutter/services/EventChannel-class.html)이 있습니다.
MethodChannel은 단발성 호출을 할 때 사용되며(e.g. 화면 켜기, 화면 끄기 등),
EventChannel은 네이티브 코드로부터 지속적인 이벤트를 받아야 할 때 사용됩니다(e.g. 화면 상태가 바뀔 때 마다 이벤트 받기 등).

## EventChannel로 플러그인 개발하기

[EventChannel](https://api.flutter.dev/flutter/services/EventChannel-class.html)은 dart 코드에서 네이티브 프레임워크(Android/iOS)의 특정 이벤트를 감지해야 하는 플러그인을 작성할 때 사용됩니다.
예컨대 플러터 앱에서 네이티브 프레임워크의 화면 회전 이벤트를 감지해야 하는 상황을 들 수 있고, 이 상황을 예로 들겠습니다.

우선, 플러그인의 dart 파일 경로는 `lib/orientation.dart`라고 가정합니다.

MethodChannel을 사용할때와 마찬가지로 고유의 채널 이름을 설정합니다.
채널 이름은 앱 내에서 고유해야 하므로 보통 플러그인의 패키지명을 포함시킵니다.
이 값은 dart 코드와 android, iOS 코드 모두에서 동일해야 합니다. 아래 코드를 `lib/orientation.dart`에 추가합니다.

```dart
const ORIENTATION_CHANNEL_NAME = 'your.package.name.orientation';
```

다음으로 위 값을 사용하여 EventChannel을 생성합니다. 아래 코드를 `lib/orientation.dart`에 추가합니다.

```dart
import 'package:flutter/services.dart';

const EventChannel orientationEventChannel = EventChannel(ORIENTATION_CHANNEL_NAME);
```

이제 플러터 앱에서 해당 플러그인을 사용할 때 접근할 api(즉, 함수)를 정의합니다. 아래 코드도 `lib/orientation.dart`에 추가합니다.

```dart
import 'dart:async';

// 이벤트를 날릴 때 사용하는 더미 클래스
class OrientationEvent { } 

Stream<OrientationEvent> _orientationEvents;

/// 플러터 앱에서 접근할 함수
Stream<OrientationEvent> get orientationEvents {
  if (_orientationEvents == null) {
    // 플러터 앱에서 몇번을 접근하든 하나의 Stream만 생성하여 재사용합니다.
    _orientationEvents = _orientationEventChannel
        .receiveBroadcastStream()
        .map((dynamic event) => OrientationEvent());
  }
  return _orientationEvents;
}
```

아직 네이티브 프레임워크의 구현부를 작성하지 않았기 때문에 동작은 하지 않지만, 플러터 앱에서는 이제 아래와 같이 해당 이벤트를 사용할 수 있습니다.

```dart
import 'dart:async';
import 'package:<your-package-name>/orientation.dart';

StreamSubscription<OrientationEvent> _orientationEventSubscription;

_orientationEventSubscription = orientationEvents.listen((OrientationEvent event) {
    // orientation event occurred!
});
```

`lib/orientation.dart`의 풀 소스는 아래와 같습니다.

```dart
import 'dart:async';
import 'package:flutter/services.dart';

const ORIENTATION_CHANNEL_NAME = 'your.package.name.orientation';

const EventChannel orientationEventChannel = EventChannel(ORIENTATION_CHANNEL_NAME);

// 이벤트를 날릴 때 사용하는 더미 클래스
class OrientationEvent { } 

Stream<OrientationEvent> _orientationEvents;

/// 플러터 앱에서 접근할 함수
Stream<OrientationEvent> get orientationEvents {
  if (_orientationEvents == null) {
    // 플러터 앱에서 몇번을 접근하든 하나의 Stream만 생성하여 재사용합니다.
    _orientationEvents = _orientationEventChannel
        .receiveBroadcastStream()
        .map((dynamic event) => OrientationEvent());
  }
  return _orientationEvents;
}
```

dart 코드는 완성되었으니 이제 Android의 구현부를 완성시킵니다.

### Android

아래 예제는 코틀린으로 작성되었으며, 플러그인의 안드로이드 파일을 `OrientationPlugin.kt`라고 가정합니다.

우선, 위에서 사용한 이벤트 채널 이름을 똑같이 `OrientationPlugin.kt`에도 정의해줍니다.
그리고 dart 파일때와 마찬가지로 해당 채널 이름을 사용하여 EventChannel을 생성합니다.

```kotlin
import io.flutter.plugin.common.EventChannel
import io.flutter.plugin.common.PluginRegistry

class OrientationPlugin {
    companion object {
        private const val ORIENTATION_CHANNEL_NAME = "your.package.name.orientation"
        
        @JvmStatic
        fun registerWith(registrar: PluginRegistry.Registrar) {
            // EventChannel 생성
            val orientationChannel = EventChannel(registrar.messenger(), ORIENTATION_CHANNEL_NAME)
        }
    }
}
```

이제 위의 EventChannel에 [StreamHandler](https://api.flutter.dev/javadoc/io/flutter/plugin/common/EventChannel.StreamHandler.html)를 달아주어야 합니다.
StreamHandler는 dart 코드에서 해당 EventChannel에 접근했을 때, 해당 dart 코드로 이벤트를 보내주는 실제 구현부를 담게 됩니다.

```kotlin
// EventChannel 생성
val orientationChannel = EventChannel(registrar.messenger(), ORIENTATION_CHANNEL_NAME)
orientationChannel.setStreamHandler(OrientationStreamHandler(registrar.activity()))
```

마지막으로 `OrientationStreamHandler`를 구현해주면 완성입니다. 이 코드 또한 `OrientationPlugin.kt` 하단에 추가합니다.

```kotlin
import android.app.Activity

class OrientationStreamHandler(private val activity: Activity): EventChannel.StreamHandler {

    // dart 코드에서 이 StreamHandler가 달린 EventChannel에 처음 접근했을 때 호출됩니다.
    override fun onListen(args: Any?, sink: EventChannel.EventSink?) {
        watchOrientationChange {
            // sink?.success를 호출하면 dart 코드의 EventChannel로 결과가 전송됩니다.
            sink?.success(0)
        }
    }

    // dart 코드에서 EventChannel을 더이상 사용하지 않을 때 호출됩니다.
    override fun onCancel(args: Any?) {
        stopWatchingOrientationChange()
    }
    
    // 구현은 알아서.. ㅎㅎ
    private fun watchOrientationChange(callback: Callback) { ... }
    
    private fun stopWatchingOrientationChange() { ... }
    
}
```

`OrientationPlugin.kt`의 풀 코드는 아래와 같습니다.

```kotlin
import android.app.Activity
import io.flutter.plugin.common.EventChannel
import io.flutter.plugin.common.PluginRegistry

class OrientationPlugin {
    companion object {
        private const val ORIENTATION_CHANNEL_NAME = "your.package.name.orientation"
        
        @JvmStatic
        fun registerWith(registrar: PluginRegistry.Registrar) {
            // EventChannel 생성
            val orientationChannel = EventChannel(registrar.messenger(), ORIENTATION_CHANNEL_NAME)
            orientationChannel.setStreamHandler(OrientationStreamHandler(registrar.activity()))
        }
    }
}

class OrientationStreamHandler(private val activity: Activity): EventChannel.StreamHandler {

    // dart 코드에서 이 StreamHandler가 달린 EventChannel에 처음 접근했을 때 호출됩니다.
    override fun onListen(args: Any?, sink: EventChannel.EventSink?) {
        watchOrientationChange {
            // sink?.success를 호출하면 dart 코드의 EventChannel로 결과가 전송됩니다.
            sink?.success(0)
        }
    }

    // dart 코드에서 EventChannel을 더이상 사용하지 않을 때 호출됩니다.
    override fun onCancel(args: Any?) {
        stopWatchingOrientationChange()
    }
    
    // 구현은 알아서.. ㅎㅎ
    private fun watchOrientationChange(callback: Callback) { ... }
    
    private fun stopWatchingOrientationChange() { ... }
    
}
```
# Setting app icon and name in Android & iOS

By default, the name on the launcher is your Flutter project name. To change the name displayed on Android or iOS application launcher, you need to change AndroidManifest.xml and Info.plist respectively.

For Android, inside AndroidManifest.xml, find <application> tag. Change its android:label property with your desired app name.

```xml
<application
      android:name="io.flutter.app.FlutterApplication"
      android:label="<<appname>>"
      android:icon="@mipmap/launcher_icon">
```

For iOS, Inside Info.plist, find CFBundleName key and change its value which is inside string tag on the next line.

```xml
<key>CFBundleName</key>
<string><<appname>></string>
```

For Android, go to android/app/src/main/res folder. You should see some folders beginning with mipmap-* which represent different pixel densities. Replace launcher_icon.png file in each folders to use custom icons.

For iOS, the icon config and files are located inside ios/Runner/Assets.xcassets/AppIcon.appiconset. The Contents.json file defines a list of icon images with different sizes and scales. The files are located in the same folder. It's recommended to follow the guideline for designing iOS icon.

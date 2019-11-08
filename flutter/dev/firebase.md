# Firebase with Flutter

[Firebase](https://firebase.google.com/)는 원래 모바일 앱에서 사용하려면 Android, iOS 따로 세팅해줘야 하는데
크로스 플랫폼인 Flutter에서 한번에 해주는 플러그인도 존재한다. [공식 문서](https://firebase.google.com/docs/flutter/setup)에서도 친절하게 설명해줌!!

내가 일단 필요로하는 것들은..

1. Cloud Firestore: NoSQL DB가 필요할 떄 씀.
2. Cloud Storage: 앱 사용자들이 Image, Video 같은거 서버에 올릴 때 사용.
3. Cloud Functions: 가벼운 백엔드 API endpoint가 필요할 떄 씀.
4. Cloud Messaging: 알림들을 쏴야할 때 필요.

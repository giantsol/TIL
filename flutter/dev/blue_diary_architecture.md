# Blue Diary Architecture

어제 [Blue Diary](https://github.com/giantsol/Blue-Diary) 1.0.0 버전을 끝마쳤고 구글 플레이스토어에 등록 요청을 보내놨다.
이 앱의 아키텍처에 대해 정리!

---

This app is based on BLoC pattern, together with my own architectural practices.

Inside the lib folder, there are three main folders:

1. data: This folder contains Dart files that actually update/fetch data from Preferences, Databases, or Network (although we don't use Network here). Most of the files here are implementations of Repository interface declared in domain/repository folder.

2. domain: This folder contains the so called 'Business Logic' of this app. It is further divided into three main folders:
    - entity: contains pure data classes such as ToDo and Category.
    - repository: contains interfaces defining functions that update/fetch data. Actual implementations are located in data folder.
    - usecase: contains per-screen business logics that utilizes several repositories to achieve each screen's needs. This is the layer that presentation has access to to utilize app data. For instance, WeekScreen uses (well, actually WeekBloc uses) WeekUsecases to interact with data underneath without directly touching repositories.
    
3. presentation: This folder contains Screens, Blocs and States that are used to display UI. It is divided into further directories that correspond to each screens in the app.
    - **Screen: where Widget's build method is called to build the actual UI shown to the user. UI is determined by values inside State, and any interactions users make (e.g. clicking a button) are delegated to corresponding Blocs.
    - **Bloc: what this basically does is "User does something (e.g. click a button)" -> "Set/Get data using corresponding usecase and update the values inside State obect" -> "Notify Screen that State has changed and you have to rebuild".
    - **State: holds all the information Screen needs to draw UI. For instance, currentDate, todos, and isLocked kinds of things.
    
Above three directories are divided to as closely follow Uncle Bob's Clean Architecture pattern.

Besides these directories are flat Dart files inside lib folder:

1. AppColors.dart: just simple color constants.
2. Delegators.dart: I used delegators when children needed to call parent's methods. However, as I've become more familiar with Flutter now, I guess ancestorStateOfType can just do that job... researching on it!
3. Dependencies.dart: contains singleton objects such as repositories and usecases. Basically, it enables a very simple injection pattern like dependencies.weekUsecases as in WeekBloc.dart.
4. Localization.dart: where localization texts are declared.
5. Main.dart: the main entry point of this app.
6. Utils.dart: well, yeah.. Utils.

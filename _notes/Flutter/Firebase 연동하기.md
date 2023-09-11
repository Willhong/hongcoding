
1. Firebase-CLI 설치

```jsx
npm install -g firebase-tools
```

2. FlutterFire-CLI 설치

```jsx
dart pub global activate flutterfire_cli
```

3. FlutterFire 설정하기

```jsx
flutterfire configure
```

위 명령 실행시 lib/firebase_options.dart 파일이 자동으로 생성된다.

4. 필요 패키지 추가

```makefile
flutter pub add firebase_core
flutter pub add firebase_auth
flutter pub add google_sign_in

```

5. MultiDex 지원 활성화

Firebase를 추가하면 내부 라이브러리가 포함되기에 MultiDex 지원을 활성화 해야한다.

<aside> 💡 Android 앱 정책상 65536개 이상의 메서드를 포함하고 있을때 MultiDex 지원 활성화가 필요하다.

</aside>

Flutter 프로젝트에서 android/app/build.gradle 파일을 열어 defaultConfig 섹션을 찾아서 multiDexEnabled true 를 추가한다.

```jsx
defaultConfig {
        // TODO: Specify your own unique Application ID (<https://developer.android.com/studio/build/application-id.html>).
        applicationId "com.example.앱 이름"
        // You can update the following values to match your application needs.
        // For more information, see: <https://docs.flutter.dev/deployment/android#reviewing-the-gradle-build-configuration>.
        minSdkVersion flutter.minSdkVersion
        targetSdkVersion flutter.targetSdkVersion
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
        **multiDexEnabled true**
    }
```

6. Flutter 앱 진입부인 main() 에 다음 코드 추가

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );

  runApp(const MainApp());
}
```





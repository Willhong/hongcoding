1. 필요 패키지 추가

```makefile
flutter pub add firebase_auth
flutter pub add google_sign_in
flutter pub add sign_in_with_apple

```

1. Firebase console에서 Authentication 시작하기

<aside> 💡 [Firebase Console](https://console.firebase.google.com/project/) 접속해 자신의 프로젝트 클릭 후 왼쪽 메뉴에서 `빌드`>`Authentication` 클릭 후 시작하기

</aside>

`Authentication` 페이지에 들어가면 `Sign-in method` 탭에서 로그인 제공업체 추가(`구글` `애플` `페이스북` 등등

---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd075a1f-f05b-4d94-a830-365cca6a0617/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a7eafc96-4814-4e78-a78a-41b3bbf6fc31/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/79604afe-399a-4a98-a1ae-9ebba6da1359/Untitled.png)

왼쪽 메뉴에서 톱니바퀴 클릭 > 프로젝트 설정으로 이동

`google-services.json`,`GoogleService-Info.plist` 다운로드 후 프로젝트 경로에 해당 파일 업데이트

<aside> 💡 로그인 코드 추가

</aside>

- 구글 로그인 코드
    
    ```dart
    Future<User?> signInWithGoogle() async {
        final GoogleSignInAccount? googleUser = await GoogleSignIn().signIn();
        final GoogleSignInAuthentication googleAuth =
            await googleUser!.authentication;
        final credential = GoogleAuthProvider.credential(
          accessToken: googleAuth.accessToken,
          idToken: googleAuth.idToken,
        );
        return (await FirebaseAuth.instance.signInWithCredential(credential)).user;
      }
    ```
    
- 애플 로그인 코드
    
    ```dart
    Future<User?> signInWithApple() async {
        try {
          final AuthorizationCredentialAppleID appleCredential =
              await SignInWithApple.getAppleIDCredential(
            scopes: [
              AppleIDAuthorizationScopes.email,
              AppleIDAuthorizationScopes.fullName,
            ],
            webAuthenticationOptions: WebAuthenticationOptions(
              clientId: '여기에 클라이언트 ID',
              redirectUri: Uri.parse('여기에 Redirect URI'),
            ),
          );
    
          final OAuthProvider oAuthProvider = OAuthProvider('apple.com');
          final AuthCredential credential = oAuthProvider.credential(
            idToken: appleCredential.identityToken,
            accessToken: appleCredential.authorizationCode,
          );
    
          final UserCredential authResult =
              await _auth.signInWithCredential(credential);
          return authResult.user;
        } catch (e) {
          print('Apple Sign-In Error: $e');
          return null;
        }
      }
    ```
    

<aside> 💡 애플 로그인의 경우 `클라이언트 ID`, `리디렉션 URI` 정보를 포함해야 한다.

</aside>

- 로그아웃 코드
    
    ```dart
    Future<void> signOut() async {
        await _auth.signOut();
        await _googleSignIn.signOut();
      }
    ```
    

<aside> 💡 아래는 전체 코드이다.

</aside>

- 전체 코드
    
    ```dart
    class AuthService {
      final FirebaseAuth _auth = FirebaseAuth.instance;
      final GoogleSignIn _googleSignIn = GoogleSignIn(scopes: ['email']);
    
      Future<User?> signInWithEmail(String email, String password) async {
        try {
          final userCredential = await _auth.signInWithEmailAndPassword(
            email: email,
            password: password,
          );
          return userCredential.user;
        } catch (e) {
          print('Sign In Error: $e');
          return null;
        }
      }
    
      Future<User?> signInWithGoogle() async {
        final GoogleSignInAccount? googleUser = await GoogleSignIn().signIn();
        final GoogleSignInAuthentication googleAuth =
            await googleUser!.authentication;
        final credential = GoogleAuthProvider.credential(
          accessToken: googleAuth.accessToken,
          idToken: googleAuth.idToken,
        );
        return (await FirebaseAuth.instance.signInWithCredential(credential)).user;
      }
    
      Future<User?> signInWithApple() async {
        try {
          final AuthorizationCredentialAppleID appleCredential =
              await SignInWithApple.getAppleIDCredential(
            scopes: [
              AppleIDAuthorizationScopes.email,
              AppleIDAuthorizationScopes.fullName,
            ],
            webAuthenticationOptions: WebAuthenticationOptions(
              clientId: '여기에 클라이언트 ID',
              redirectUri: Uri.parse('여기에 Redirect URI'),
            ),
          );
    
          final OAuthProvider oAuthProvider = OAuthProvider('apple.com');
          final AuthCredential credential = oAuthProvider.credential(
            idToken: appleCredential.identityToken,
            accessToken: appleCredential.authorizationCode,
          );
    
          final UserCredential authResult =
              await _auth.signInWithCredential(credential);
          return authResult.user;
        } catch (e) {
          print('Apple Sign-In Error: $e');
          return null;
        }
      }
    
      Future<void> signOut() async {
        await _auth.signOut();
        await _googleSignIn.signOut();
      }
    }
    ```
    

1. SHA 인증서 지문 추가

다음 명령어를 cmd에 입력해서 키 SHA 인증서 지문을 가져온다.

```markdown
cd "C:\\Users\\여기에 사용자이름\\.android>"
```

`debug.keystore`의 위치로 이동

```markdown
keytool -list -v -keystore debug.keystore -alias androiddebugkey -storepass android -keypass android
```

위 커맨드를 실행하면

`SHA1: ~~어쩌구~~`

`SHA256: ~~저쩌구~~`

위와 같이 SHA 인증서 지문을 가져올 수 있다.

1. Firebase 콘솔에 SHA 인증서 지문 입력

2번에서 `google-services.json` 를 다운로드 받았던 페이지(프로젝트 설정)으로 이동해서 SHA1, SHA256 인증서 지문 추가한다.
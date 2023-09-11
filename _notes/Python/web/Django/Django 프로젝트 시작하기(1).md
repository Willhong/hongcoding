
1. **가상 환경 설정 (Optional but Recommended):** 가상 환경은 프로젝트별로 패키지를 격리하여 프로젝트 간 충돌을 방지하고 필요한 패키지를 관리하는 데 도움이 됩니다. 터미널에서 다음 명령을 실행하여 가상 환경을 생성합니다:
    
    ```bash
    python -m venv myenv
    
    ```
    
    가상 환경을 활성화합니다:
    
    - Windows: **`myenv\\Scripts\\activate`**
    - macOS 및 Linux: **`source myenv/bin/activate`**
2. **Django 설치:** 가상 환경을 활성화한 상태에서 다음 명령을 실행하여 Django를 설치합니다:
    
    ```bash
    pip install django
    
    ```
    
3. **프로젝트 생성:** 프로젝트를 생성할 디렉토리로 이동한 후, 다음 명령을 실행하여 Django 프로젝트를 생성합니다:
    
    ```bash
    django-admin startproject myproject
    
    ```
    
4. **개발 서버 실행:** 프로젝트 디렉토리로 이동한 후, 다음 명령을 실행하여 개발 서버를 실행합니다:
    
    ```bash
    cd myproject
    python manage.py runserver
    
    ```
    
    이제 **`http://127.0.0.1:8000/`** 주소로 웹 브라우저를 열어 Django 기본 화면을 확인할 수 있습니다.
    
5. **앱 생성:** 프로젝트 내에서 각각의 기능이나 모듈을 관리하기 위해 앱을 생성합니다. 앱 생성은 프로젝트 디렉토리로 이동한 후, 다음 명령을 실행합니다:
    
    ```bash
    python manage.py startapp 앱이름
    
    ```
    
6. **URL 설정:`myproject/urls.py`** 파일을 열어서 앱의 URL을 설정합니다. **`myapp/urls.py`** 파일을 생성하고 앱 내 URL을 정의합니다.
    
7. **뷰 및 템플릿 작성:** 앱 내의 **`views.py`** 파일에서 뷰 함수를 작성하고, **`templates/`** 디렉토리 내에 HTML 템플릿 파일을 작성합니다.
    
8. **모델 생성 및 마이그레이션:`models.py`** 파일에서 데이터베이스 모델을 정의한 후, 다음 명령으로 마이그레이션을 생성하고 데이터베이스에 적용합니다:
    
    ```bash
    python manage.py makemigrations 앱이름
    python manage.py migrate 앱이름
    python manage.py migrate
    
    ```
    
9. **관리자 계정 생성 (Optional but Recommended):** 관리자 페이지를 사용하려면 관리자 계정을 생성합니다:
    
    ```bash
    python manage.py createsuperuser
    
    ```
    
10. `[settings.py](<http://settings.py>)` 의 `INSTALLED_APPS` 에 `앱이름` 추가하기
    
    ```markdown
    INSTALLED_APPS = [
        ...
        ...
        ...
        ...
        '앱이름',
    ]
    ```
    
11. **개발 진행:** 이제 뷰, 템플릿, URL, 모델 등을 조합하여 프로젝트를 개발하고 확장해 나갈 수 있습니다.
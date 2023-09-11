
# **장고(Django)에서 HTML, CSS, JS 적용하기**

장고 프레임워크에서는 정적 파일 (static files) 라고 불리는 HTML, CSS, JS 파일을 특정 디렉터리에 저장하고 이를 웹 페이지에서 불러올 수 있습니다. 이 문서에서는 간단한 방법을 소개합니다.

## **프로젝트와 앱 설정**

장고 프로젝트와 앱이 이미 생성되어 있다고 가정합니다. 만약 아직 생성하지 않았다면 다음 명령어로 생성할 수 있습니다.

```bash
bashCopy code
django-admin startproject myproject
cd myproject
python manage.py startapp myapp

```

## **정적 파일 디렉터리 설정**

1. **`myapp`** 디렉터리 (또는 원하는 앱의 디렉터리) 내부에 **`static`** 폴더를 생성합니다.
    
    ```bash
    mkdir myapp/static
    
    ```
    
2. **`static`** 폴더 내부에 **`css`**, **`js`**, **`images`** 등의 폴더를 생성합니다.
    
    ```bash
    cd myapp/static
    mkdir css js images
    
    ```
    

## **정적 파일 추가**

- **`css`** 폴더에 **`.css`** 파일을,
- **`js`** 폴더에 **`.js`** 파일을,
- **`images`** 폴더에 이미지 파일들을 추가합니다.

예를 들면,

```css
/* myapp/static/css/main.css */
body {
    background-color: #f0f0f0;
}

```

```jsx
// myapp/static/js/main.js
alert("Hello, Django!");

```

## **HTML 템플릿에 적용**

1. **`myapp/templates`** 폴더에 **`.html`** 파일을 생성하거나 이미 있는 **`.html`** 파일을 수정합니다.
2. **`{% load static %}`** 태그를 사용하여 정적 파일을 불러옵니다.

예를 들면,

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Django App</title>
    {% load static %}
    <link rel="stylesheet" type="text/css" href="{% static 'css/main.css' %}">
</head>
<body>
    <h1>Hello, World!</h1>
    <script src="{% static 'js/main.js' %}"></script>
</body>
</html>

```

## **[settings.py](http://settings.py) 설정 확인**

**`settings.py`** 파일에서 **`STATIC_URL`**이 정의되어 있는지 확인하세요.

```python
# settings.py
STATIC_URL = '/static/'

```

이제 장고 서버를 실행하고 웹 브라우저에서 확인하면, 지정한 CSS와 JS가 적용되어 있는 것을 확인할 수 있습니다.

```bash
python manage.py runserver

```

웹 브라우저에서 **`http://127.0.0.1:8000/`** 주소에 접속해 보세요.
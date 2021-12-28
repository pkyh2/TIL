# Django

url: http://127.0.0.1:8000/index/html

request: GET/index.html HTTP/1.1

최상위 URLconf

path('', include('app.url'))

하위 URLconf

view(control)

tamplates



### 게시판 실습하기

1. 장고 프로젝트 스타트
2. 장고 앱 생성
3. `setting.py`파일에 앱추가
4. 최상위 URLconf에서 요청하는 URL과 앱을 먼저 연결
   - `urls.py`에서 `path(route, view)`함수 추가
   - `route`는 URL패턴을 가진 문자열
   - `view`는 `route`에서 일치하는 패턴을 찾으면, `view`함수 호출
   - `include()`를 통해 인자로 하위 URLconf에 대한 설정을 호출한다.

5. 앱폴더 경로에서 `urls.py`를 만들어 하위 URLconf를 설정

   - 하위 URLconf에서는 `views.py`의 함수들을 호출한다.

6. `python manage.py runserver`를 통해`admin/` URL로 들어가 서버 연결을 확인한다.

7. `appFolder/templates/appName` 경로와 같이 앱 폴더안에 templates/'appName' 폴더를 생성해 준다. (폴더이름 주의!)

8. `appName` 폴더 안에서 `.html`파일을 작성

9. css 연결 

   - `setting.py` 파일에 `STATIC_URL` 쪽에`static`폴더의 경로를 설정하는 코드를 추가

     ```code
     STATICFILES_DIRS = [
         BASE_DIR / 'static',
     ```

   - project폴더 아래 `static`폴더 생성(app폴더 안에 아님!)

   - bootstrap 링크에서 위에 `load static` 템플릿 태그를 추가해 `static` 폴더로 부터 css파일을 import하고, 링크 경로를 템플릿 태그로 변경하고 `static` 폴더에 css파일을 추가해준다.

10. `.html` 과 `css` 연동 까지 확인 후 데이터베이스 정의 `models.py` 파일 수정

    - 데이터베이스 설정
    - 테이블 형태 정의(모델정의)
    - 데이터의 형태 정의(클래스에서)

11. `model.py` 에서 생성한 클래스는 장고의 `models` 패키지의 `Board`클래스를 상속으로 받는다. 
    - 테이블 모델을 정의하였으면 서버에 적용해주기 위해
    - `python manage.py makemigrations` 명령어를 통해 데이터 베이스에 초기화 정보를 전달
    - `python manage.py migrate`를 통해 데이터베이스 테이블을 생성한다.
    - dbshell에 접속하여 DB query날려보기

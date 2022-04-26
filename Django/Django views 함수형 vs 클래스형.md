# Django views 함수형 vs 클래스형

## 함수형 뷰

- 신속한 개발이 가능하지만, 로직이 복잡해진다.

- `if request.method == 'GET'`과 같은 조건을 달고 로직 구성

  ```python
  # 함수형뷰 예제
  from django.http import HttpResponse
  
  def my_view(request):
      if request.method == 'GET' :
          # 로직작성
          return HttpResponse('result')
          
      if request.method == 'POST' :
      	# 로직작성
          return HttpResponse('result')
  ```

  

## 클래스형 뷰

- 상속과 믹스인 기능을 사용하여 코드 재사용이 용이

- 뷰를 체계적으로 구성할 수 있음

- **제네릭뷰** 역시 클래스형 뷰

- <u>urls.py에 `.as_view()` 메서드와 함께 사용</u>

  ```python
  # 클래스형뷰 예제
  from django.http import HttpResponse
  from django.views import View
  
  class MyView(View):
      def get(self, request):
          # 로직작성
          return HttpResponse('result')
      
      def post(self, request):
          # 로직작성
          return HttpResponse('result')
      
      def head(self, *args, **kwargs):
          # 로직구현
          return HttpResponse('')
  ```

- `MyView`클래스는 View 클래스를 상속받음

- View 클래스에는 `as_view()` 메서드와 `dispatch()` 메서드가 정의되어 있음.

## .as_view(인자)

- URL 패턴에서 클래스 View로 넘어갈때 호출되는 메서드

- `as_view()`는 클래스의 인스턴스 생성

- 생성된 인스턴스는 `dispatch()` 메서드 호출

- `dispatch()`는 GET, POST등 HTTP method를 구분하여 해당 인스턴스 내의 get, post 이름과 매칭되는 메서드로 연결

- 해당 메서드가 구현되지 않았을 경우 `HttpResponseNotAllowed` 에러 발생

  ```
  # urls.py
  
  from django.urls import path
  
  urlpatterns = [
     path('', MyView.as_view(인자), name='my_view'),
  ]
  ```



## 제네릭 뷰

장고에서 자주쓰는 뷰를 미리 만들어 제공, 제네릭 뷰라고 한다. 클래스 형으로 구현되어 있기 때문에 상속받아 사용한다.

Django 에서 제공하는 제네릭 뷰는 다음과 같이 4가지로 분류할 수 있다.

- Base View: 뷰 클래스를 생성하고, 다른 제네릭 뷰의 부모 클래스를 제공하는 기본 제네릭 뷰
- Generic Display View: 객체의 리스트를 보여주거나, 특정 객체의 상세 정보를 보여준다.
- Generic Edit View: 폼을 통해 객체를 생성, 수정, 삭제하는 기능을 제공한다.
- Generic Date View: 날짜 기반 객체의 년/월/일 페이지로 구분해서 보여준다.

아래는 위 4가지 분류에 따른 구체 뷰 클래스에 대한 설명을 보여준다.

- Base View
  - View: 가장 기본이 되는 최상위 제네릭 뷰
  - TemplateView: 템플릿이 주어지면 해당 템플릿을 렌더링한다.
  - RedirectView: URL이 주어지면 해당 URL로 리다이렉트 시켜준다.
- Generic Display View
  - DetailView: 객체 하나에 대한 상세한 정보를 보여준다.
  - ListView: 조건에 맞는 여러 개의 객체를 보여준다.
- Generic Edit View
  - FormView: 폼이 주어지면 해당 폼을 보여준다.
  - CreateView: 객체를 생성하는 폼을 보여준다.
  - UpdateView: 기존 객체를 수정하는 폼을 보여준다.
  - DeleteView: 기존 객체를 삭제하는 폼을 보여준다.
- Generic Date View
  - YearArchiveView: 년도가 주어지면 그 년도에 해당하는 객체를 보여준다.
  - MonthArchiveView: 월이 주어지면 그 월에 해당하는 객체를 보여준다.
  - DayArchiveView: 날짜가 주어지면 그 날짜에 해당하는 객체를 보여준다.
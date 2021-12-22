# DOM(Document Object Model)

## 문서 객체 모델

- 문서 객체 모델(DOM)은 XML이나 HTML 문서에 접근하기 위한 일종의 인터페이스입니다.

### Document 객체

- Document 객체는 웹 페이지 그 자체를 의미
- 웹 페이지에 존재하는 HTML 요소에 접근하고자 할 때는 반드시 Document 객체부터 시작해야 한다.
- 자바스크립트는 document를 통해서 웹의 모든 내용을 전부 제어
- 모든 요소에 대한 정의와 접근 방법들이 전부 명시되어 있다.

### 요소의 선택

- `document.getElementByTagName('태그이름')`
- `document.getElementById('Id 속성')`
- `document.getElementByClassName('class 속성')`
- `document.querySellectAll('CSS 선택자')`



# BOM(Browser Object Model)

## 브라우저 객체 모델

- 브라우저 객체 모델은 브라우저와 컴퓨터 스크린에 접근 할 수 있는 객체의 모음.

### window 객체모델

- `navigator` : 브라우저명과 버전정보를 속성으로 가짐
- `window` : 최상위 객체로, 각 프레임별로 하나씩 존재
- `document` : 현재문서에 대한 정보
- `location` : 현재 URL에 대한 정보, 브라우저에서 사용자가 요청하는 URL
- `history` : 현재의 브라우저가 접근했던 URL history
- `screen` : 브라우저의 외부환경에 대한 정보를 제공


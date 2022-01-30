# Crawling & Scraping

## 크롤링 주의사항

### 사전 기초지식

- `/robots.txt` 를 크롤링하고자 하는 사이트뒤에 붙여보면 크롤링에 대한 허용 가능을 알 수 있다.
- 크롤러 분류 - 로그인을 해야 가져올 수 있는건지, javascript 유무
- `Request`요청 주의 - 서버 부하 고려(많은 데이터를 크롤링하면 문제된다.)
- 콘텐츠 저작권 문제
- 페이지 구조 변경 가능성 숙지

### urllib

- `.urlretrieve` : 매개변수로 첫번째는 url경로, 두번째는 다운받을 폴더 경로를 입력해 준다.
  - return 값으로 filename과 header 정보를 내보낸다.
- `req.urlopen(url)` : 매개변수로 url을 받아 `.info()`, `.getcode()` 와 같은 메서드를 통해 url 정보와 http status를 받아 올 수 있다.
  - 그래서 여러 url을 한번에 처리할때 반복문에서 인자로 url을 받아와 `.urlopen(url)` 인자 값으로 넣어준다.  
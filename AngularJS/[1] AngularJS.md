# AngularJS 기초

DATE : 2020-03-24

<br /><br />

## 1. AngularJS의 특징

- SPA (Single Page Application) framework 이다.
- MVC/MVVM 패턴이 적용된다.
- Model과 View간 양방향 데이터 바인딩이 가능하다.
- 의존성 주입(Dependency Injection)을 활용한다.

<br /><br />

## 2. AngularJS 주요 개념

### 2.1. Scope

- Scope는 모델 변경을 감지하고 표현하기 위한 책임을 갖는다.
- Scope는 DOM 구조와 가깝게 하이어라키 구조를 갖는다.

### 2.2. Model

- 모델은 화면 템플릿에 합쳐지는 데이터를 가지고 있는 일반 자바스크립트 객체이다. (데이터)
- Scope는 항상 모델을 참조한다.

### 2.3. View

- Templete String과 Model이 합쳐져 보여지며 DOM 구조를 가지고 있다.
- Angular는 템플릿이 HTML이어서 바로 DOM으로 해석되고 DOM 안에 directive가 템플릿 엔진인 $complie을 통해 $watch를 설정, 모델의 변경을 감시한다.
- View는 템플릿으로 Scope의 투영체이고, Scope는 Model과 View를 연결하며 controller로 이벤트를 보내는 역할을 한다.

### 2.4. Controller

- Controller는 View 뒤에서 반드시 수행하는 코드이다.
- Controller 역할은 모델을 생성하고 콜백 메소드를 가지고 View로 퍼블리싱을 담당한다.
- Controller는 자바스크립트로 이루어진 제어 로직이다. 또한 DOM rendering 정보가 일체 없다.

### 2.5. Directives (지시어)

- 지시어는 HTML 을 확장하여 주고 행위를 일으키는 주체이다.
- 예를들어 앞의 예제에서 보았듯이 데이터 바인딩을 위한 이중 중괄호 표기{{}}, 컨트롤러가 뷰의 어느 부분을 감독할지를 지정하는 ng-controller, 인풋을 해당 모델의 구성물에 바인딩하는 ng-model 모두 Directive를 이용한 확장 문법이다.

### 2.6. Service

- 특정 기능을 담당하는 객체
- 싱글톤 객체로 하나의 인스턴스만 존재
- $를 앞에 붙여서 표기함

### 2.7. Expression

- {{}} 로 사용되며 표현식 내에 자바스크립트 문법을 사용한다.
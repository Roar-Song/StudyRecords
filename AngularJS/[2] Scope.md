# Scope 기초 및 활용

## 1. Scope

AngularJS에서 Scope는 Controller, Directive의 유효범위 내의 저장공간이라고 생각하는게 이해하기 쉬움.
View는 Scope를 통해 Controller 내부에 정의된 Model(Data)이나 Handler 함수에 접근이 가능함.

### EX) Scope 기본 사용

```js
// script.js
function Ctrl($scope) {
    $scope.name = "Roar-Song";

    $scope.say = function() {
        $scope.showme = "Hi, " + $scope.name;
 };
}
```

```html
<!-- index.html -->
<div ng-controller="Ctrl">
    My name :
        <input type="text" ng-model="name">
        <button ng-click='say()'>click</button>
    <hr>
    {{showme}}
</div>
```

**Ctrl** 이라는 Controller의 $scope는 **name** 이라는 Model과 **say()** 라는 Handler 함수가 정의되어 있다. 위 코드는 input에 텍스트를 넣고 버튼을 누르면 줄 하단에 **Hi, [사용자입력값]** 를 출력하는 코드이다.

<br /><br />

## 2. Scope Hierarchies

AngularJS에서 scope는 최상위 root scope와 여러개의 child scope로 구성된다. 즉, scope는 **계층형 구조**를 가진다. 최상위 scope인 "root scope" 는 **ng-app** Directive가 정의된 DOM에 할당된다.

### EX) 계층 구조 

```html
<html ng-app>                           <!-- <html>에 할당된 root scope -->  
<div ng-controller="Ctrl">              <!-- <div>에 할당된 Ctrl의 scope -->
    <div ng-controller="Ctrl2">         <!-- <div>에 할당된 Ctrl2의 scope -->
    </div>
</div>  
</html>  
```

scope는 JavaScript의 상속구조로 인해 같이 상속구조를 띄게 된다.

아래의 예제를 보면 자식 scope에서 부모 scope로 접근이 가능한 것을 알 수 있다.

### EX) 상속 구조

```js
// script.js
function Ctrl($scope) {         // name : Roar-Song  
  $scope.name = 'Roar-Song';
}
function Ctrl2($scope) {        // name : 존재안함.  
  $scope.year = '2014';
}
```

```html
<!-- index.html -->
<html ng-app>  
<div ng-controller="Ctrl">  
    {{name}}                    <!-- Roar-Song 출력 -->
    <br />
    <div ng-controller="Ctrl2">
         {{name}}               <!-- Roar-Song 출력 -->
    </div>
</div>  
</html>
```

Ctrl2는 'name' 이 없지만 자신의 부모 scope로 이동하여 'name' 을 찾아냈습니다. 이와 같이 scope는 프로퍼티를 찾을 때 자기 자신부터 찾고, 없다면 부모 scope로 이동하여 올라가다 최종적으로 root scope까지 거슬러 올라갑니다.

이런 계층 구조는 유용하기도 하지만 의도하지 않은 값이 출력되는 경우가 종종 발생합니다. 이러한 경우를 위해 AngularJS는 **isolate scope** 를 지원하여 자기 자신에서만 검색하도록 제어합니다.

<br /><br />

## 3. Isolate Scope

isolate scope는 앞서 설명한 scope와 동일하지만, 모델이나 핸들러를 검색할 경우 그 검색이 상위로 진행되지 않는다는 것이 유일한 차이점입니다. 일반 scope와 달리 isolate scope는 **오직 자신의 scope 내에서만 모델과 핸들러를 찾습니다.** isolate scope를 이용하면 자신만의 독립적인 사용자정의 디렉티브를 만드는데 활용할 수 있습니다.

<br /><br />

## 4. Scope의 LifeCycle

1. 브라우저가 Javascript를 로딩할 때, AngularJS의 **$injector를 통해 정의된 ng-app에 $scope를 할당**한다.
2. 링킹 과정에서 **$scope 하위에 $watch 오브젝트들이 등록**된다.
3. 연결된 model의 변화가 감지되면 **\$scope.$apply()** 메서드가 AngularJS 내부에서 호출된다. 작업은 $http, $timeout, $interval 모듈을 이용해서 비동기적으로 이루어진다.
4. 변화가 감지되면 AngularJs는 $digest cycle을 rootScope 에서 수행한다. 변경 내용은 모든 child Scope로 전파되며 당연히 각 $scope에 정의된 $watch가 이벤트를 전달받는다.
5. scope 혹은 child Scope가 더이상 필요가 없어지면 \$scope.$destroy가 이루어지며 root Scope에 의한 전파가 중단된다. 중단된 $scope는 향후 가비지 컬렉팅된다.

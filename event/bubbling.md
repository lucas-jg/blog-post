# Javascript 이벤트 (이벤트 등록 방법, 버블링, 캡처, 위임)

> C/C++ 개발자 였던 당시에 자바스크립트 프로젝트를 갑자기 진행하며 이벤트 버블링으로 인한 버그를 경험이 있습니다. 
> 당시에 아무것도 모르는 상태에서 시작하여 이런저런 현상을 몸으로 겪고 일정에 쫓기며 개발했던 때를 생각하며
> 자바스크립트의 이벤트에 대한 내용을 정리해보려고 합니다. 많은 도움이 되길 바랍니다.

### 버그의 시작
내가 원하지 않았던 부분에서 onClick 이벤트가 발생하여 버그가 발생했어요.
로그를 찍어가며 확인을 한 결과 onClick 이벤트가 상위 엘리먼트까지 역순으로 발생하는 것을 확인했습니다.

## 이벤트 버블링?
> Event bubbling is a type of event propagation where the event first triggers on the innermost target element, and then successively triggers on the ancestors (parents) of the target element in the same nesting hierarchy till it reaches the outermost DOM element or document object (Provided the handler is initialized).  - Wikipedia(Event bubbling)

간단하게 이야기해서 이벤트 버블링이란 이벤트 전파 방식중 일부이며 최초 이벤트가 발생하면 그 위 부모 엘리먼트에게 전달되는 방식을 이야기합니다.

(출처 : Wikipedia - Event bubbling)
![EventBubbling](https://upload.wikimedia.org/wikipedia/commons/8/87/Event_bubbling.jpg "Event Bubbling")

 
 ### 이벤트 버블링의 간단한 예제
동작하는 코드는 아래 Codesandbox에서 확인해보세요.
[JS - Event Bubbling Code](https://4qw7p9zrx9.codesandbox.io/) 
 
### HTML
 ```html
<div class="b-one">
    b-one
	<div class="b-two">
        b-two
	    <div class="b-three">
            b-three
	    </div>
    </div>
</div>
 ```
### Javascript Add event
```javascript
var divs = document.querySelectorAll("div");
divs.forEach(function(div) {
  div.addEventListener("click", logEvent);
});

function logEvent(event) {
  alert(event.currentTarget.className);
}
```
 div을 클릭하는 경우 onclick 이벤트가 버블링되어 순차적인 알림창을 확인 할 수 있습니다.
 b-three div를 클릭 : b-three -> b-two -> b-one
 최초 발생한 위치에서 부모로 전파되는걸 확인 할 수 있었습니다.
 
### 버블링 되는 것을 막기 위해서는?
> event.stopPropagation();
> 이벤트가 버블링 되는 것을 막을 수 있습니다.

### 이벤트 버블링을 막는 간단한 예제
동작하는 코드는 아래 Codesandbox에서 확인해보세요.
[JS - Event Bubbling block Code](https://3r5kv1rjw1.codesandbox.io/) 

### HTML
 ```html
<div class="b-one">
    b-one
	<div class="b-two">
        b-two
	    <div class="b-three">
            b-three
	    </div>
    </div>
</div>
 ```
### Javascript Add event
```javascript
var divs = document.querySelectorAll("div");
divs.forEach(function(div) {
  div.addEventListener("click", logEvent);
});

function logEvent(event) {
  event.stopPropagation(); // 추가된 부분
  alert(event.currentTarget.className);
}
```
### 이벤트 버블링 정리
> 최초 발생한 위치에서 부모로 전파되는 이벤트 전파 방식입니다.
> 이벤트 버블링에 대한 지식이 없는 상태에서 코딩을 하다보면 생각지도 못한 이벤트가 발생하는 버그를 발견할 수 있겠죠.
> 이벤트 버블링에 대해 잘 이해하고 사용한다면 문제가 없겠지만 급한 프로젝트나 버블링에 대해 생각하지 못하고 구조를 잡은
> 프로젝트의 경우 event.stopPropagation();를 사용하여 이벤트 버블링을 막는 방법이 있습니다.

## 이벤트 캡처?
> 위에서 알아본 이벤트 버블링의 정반대 개념이라고 이해하면 쉽게 이해할 수 있습니다.
> 바로 예제를 통해서 어떻게 동작하는지 알아보도록 하겠습니다.

### 이벤트 캡처 구현 방법
addEventListener()의 세번째인자에 true를 넣으면 적용되며, 기본값은 false입니다.
3번째 인자를 넣지 않는 경우 자동으로 이벤트 버블링 방식의 전파가 이루어집니다.

### 이벤트 캡처 예제
동작하는 코드는 아래 Codesandbox에서 확인해보세요.
[JS - Event Capturing Code](https://j33v4l7kx9.codesandbox.io/) 

### HTML
 ```html
<div class="b-one">
    b-one
	<div class="b-two">
        b-two
	    <div class="b-three">
            b-three
	    </div>
    </div>
</div>
 ```
### Javascript Add event
```javascript
var divs = document.querySelectorAll("div");
divs.forEach(function(div) {
  div.addEventListener("click", logEvent, true); // 변경된 부분 addEventListener 세번째 인자에 true를 넣음
});

function logEvent(event) {
  alert(event.currentTarget.className);
}
```

이번 포스트에서는 자바스크립트의 이벤트 전파방식인 버블링과 캡처링에 대해 알아보았습니다.
이벤트 전파에 대해 모르고 개발하다보면 생각지도 못한곳에서 이벤트가 발생하는 버그를 볼 수 있을꺼에요.
그런 일이 있기전에 제 포스트를 보고 조금의 시간이라도 벌었으면 좋겠네요.

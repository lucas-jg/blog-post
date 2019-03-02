# Javascript 이벤트 (이벤트 등록 방법, 버블링, 캡처, 위임)

> C/C++ 개발자 였던 당시에 자바스크립트 프로젝트를 갑자기 진행하며 이벤트 버블링으로 인한 버그를 경험이 있습니다. 
> 당시에 아무것도 모르는 상태에서 시작하여 이런저런 현상을 몸으로 겪고 일정에 쫓기며 개발했던 때를 생각하며
> 자바스크립트의 이벤트에 대한 내용을 정리해보려고 합니다. 많은 도움이 되길 바랍니다.

### 버그의 시작
내가 원하지 않았던 부분에서 onClick 이벤트가 발생하여 버그가 발생했어요.
로그를 찍어가며 확인을 한 결과 onClick 이벤트가 상위 엘리먼트까지 역순으로 발생하는 것을 확인했습니다.

### 이벤트 버블링?
> Event bubbling is a type of event propagation where the event first triggers on the innermost target element, and then successively triggers on the ancestors (parents) of the target element in the same nesting hierarchy till it reaches the outermost DOM element or document object (Provided the handler is initialized).  - Wikipedia(Event bubbling)

간단하게 이야기해서 이벤트 버블링이란 이벤트 전파 방식중 일부이며 최초 이벤트가 발생하면 그 위 부모 엘리먼트에게 전달되는 방식을 이야기합니다.

(출처 : Wikipedia - Event bubbling)
![EventBubbling](https://upload.wikimedia.org/wikipedia/commons/8/87/Event_bubbling.jpg "Event Bubbling")

 
 ### 이벤트 버블링의 간단한 예제
동작하는 코드는 아래 Codesandbox에서 확인해보세요.
[JS - Event Bubbling Code](https://4qw7p9zrx9.codesandbox.io/) 
 ```javascript
<div class="Element1" onclick="alert('Element1')">
  <span class="Element2" onclick="alert('Element2')">
    <button class="Element3" onclick="alert('Element3')" >
      버튼
    </button>
  </span>
</div>
 ```
 위 코드에서 버튼을 클릭하는 경우 onclick 이벤트가 버블링되어 3개의 알림창을 확인 할 수 있습니다.
 Element3 -> Element2 -> Element1
 최초 발생한 위치에서 부모로 전파되는걸 확인 할 수 있었습니다.

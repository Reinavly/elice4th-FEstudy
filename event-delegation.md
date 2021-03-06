
> 이벤트 위임을 알기 위해선 선수지식으로 이벤트 버블링과 캡쳐링의 동작 방식을 알아야 한다.

## 이벤트 버블링 (Event Bubbling) 

**이벤트 버블링** 이란, 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 특성을 의미한다.

```html
<div>
  <ul>
    <li>예시</li>
  </ul>
</div>
```

```javascript
document.querySelector('li').addEventListener('click', () => console.log('li 클릭'));
document.querySelector('ul').addEventListener('click', () => console.log('ul 클릭'));
document.querySelector('div').addEventListener('click', () => console.log('div 클릭'));
```

이렇게 각 엘리먼트들에 이벤트 핸들러를 달고 `li` 태그를 클릭해보면 콘솔에는 아래와 같이 찍히게 된다.

```
li 클릭
ul 클릭
div 클릭
```

여기서 주의해야 할 점은 각 태그마다 이벤트가 등록되어 있기 때문에 상위 요소로 이벤트가 전달되는 것을 확인할 수 있는 것이다.

만약 이벤트가 특정 태그에만 달려 있다면 위와 같은 동작 결과는 확인할 수 없다.

## 이벤트 캡쳐(Event Capture)

**이벤트 캡쳐** 란, 특정 화면 요소에서 이벤트가 발생했을 때 상위의 화면 요소에서 해당 하위 요소로 이벤트가 전달되어 가는 특성을 의미한다. 
즉, 이벤트 버블링의 반대라고 이해하면 된다.

"이벤트 버블링"을 "이벤트 캡쳐"로 바꾸기 위해선 `addEventListener()` 의 마지막 인자로 `{ capture: true }` 를 전달해주면 된다.

```javascript
document.querySelector('li').addEventListener('click', () => console.log('li 클릭'),
{ capture: true });
document.querySelector('ul').addEventListener('click', () => console.log('ul 클릭'),
{ capture: true });
document.querySelector('div').addEventListener('click', () => console.log('div 클릭'),
{ capture: true });
```

```
div 클릭
ui 클릭
li 클릭
```

이 때 유의할 점은, 이벤트 캡쳐를 적용한 경우 적용된 엘리먼트부터 콘솔에 찍히고, 적용하지 않은 요소들이 있을 경우 그다음부터는 다시 "이벤트 버블링" 방식을 동작한다는 점이다.
`li` 부터는 다시 "이벤트 버블링" 이 동작해서 `div` 에 이벤트가 발생하는 것이다.

```javascript
document.querySelector('li').addEventListener('click', () => console.log('li 클릭'));
document.querySelector('ul').addEventListener('click', () => console.log('ul 클릭'),
{ capture: true });
document.querySelector('div').addEventListener('click', () => console.log('div 클릭'));
```

```
ul 클릭
li 클릭
div 클릭
```

이렇게 하위 요소에 일일이 태그를 수정해줘야 하는 불편함 때문에, 효율성을 높이고자 '이벤트 위임'을 사용한다.


<br>

## 이벤트 위임 (Event Delegation)

**이벤트 위임** 이란  하위 요소에 각각 이벤트를 붙이지 않고 상위 요소에서 하위 요소의 이벤트들을 제어하는 방식이다.

* 동적으로 엘리먼트를 추가할 때마다 핸들러를 고려할 필요가 없다.
* 상위 엘리먼트에 하나의 이벤트 핸들러만 추가하면 되기 때문에 코드가 훨씬 깔끔해진다.
* 메모리에 있게되는 이벤트 핸들러가 적어지기 때문에 퍼포먼스 측면에서 이점이 있다.

예시를 살펴보자.

```javascript
var ulist = document.querySelector('ul');
ulist.addEventListener('click', function(event) {
    alert(event.type + '!');    
});
```

li태그의 상위 요소인 ul 태그를 ulist로 정의하고, ulist에 이벤트 리스너를 달아 하위에서 발생한 클릭 이벤트를 감지한다. (=이벤트 버블링 방식)

무조건 이벤트 위임이 좋은 것은 아니기 때문에 상황에 맞게 잘 사용하도록 하자.

<br>

## 참고
https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/

https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/javascript/event-delegation.md


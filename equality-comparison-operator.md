# 동등/일치 비교 연산자 (loose equality/strict equality comparison operator)

좌항과 우항의 피연산자가 같은 값으로 평가되는지를 비교해 불리언 값(true/false)을 반환한다.

동등 비교 연산자는 느슨하고, 일치 비교 연산자는 엄격하다.

  
+ \== : 동등 비교 : x==y : x와 y의 값이 같음  
+ \=== : 일치 비교 : x==y : x와 y의 값이 같고 타입도 같음

+ != : 부동등 비교 : x !=y : x와 y의 값이 다름  
+ !== : 불일치 비교 : x !==y : x와 y의 값과 타입이 다름

둘 다 동일한 비교를 하지만, 엄격한(Strict) 동등 비교 연산자(===)의 경우, **타입변환(Type conversion)이 일어나지 않으며 타입이 일치**해야 한다.


## 동등 비교 연산자
**동등 비교 연산자**는 좌항과 우항의 피연산자를 비교할 때,  
먼저 암묵적 타입 변환을 통해 타입을 일치 시킨 후 같은 값인지 비교한다.

좌항과 우항의 피연산자가 타입이 다르더라도 '암묵적 타입 변환'후에 같은 값일 수 있다면 true를 반환한다.

```
'' == '0'           // false : ''는 0으로 타입 변환
0 == ''             // true
0 == '0'            // true

false == 'false'    // false : false는 0으로 타입 변환
false == '0'        // true

false == undefined  // false : false == true
false == null       // false : false == true
null == undefined   // true : true == true

' \t\r\n ' == 0     // true
```

동등 비교 연산자(==) 를 사용해서 여러가지 원시타입(변경 불가능한 값)을 비교해보았는데, 타입변환이 발생함을 볼 수 있다.

단, 객체/배열의 경우는 객체(참조) 타입(변경 가능한 값)이기 때문에 두 연산자 모두 동일하게 동작한다.

```
var a = {}
var b = {}

a == b // false
a === b // false

var c = [];
var d = [];

c == d // false
c === d // false
```

문자열의 경우는 좀 특별한데, 자바스크립트에서 문자열은 원시타입(변경 불가능한 값)이지만 객체로도 만들 수 있기 때문에 그 동등 비교가 다르다.

```
var a = "string" // typeof a 했을때, string type
var b = new String("string") // typeof b 했을 때, object type 
a == b // true   string = string
a === b // false  string(string) != string(object)
```

**안전한 타입 체크와 더 좋은 코드를 위해 엄격한 일치 비교 연산자(===)를 사용하는 것이 바람직하다.** 


## 일치 비교 연산자

**일치 비교 연산자**는 좌황과 우항의 피연산자가 타입도 같고 값도 같은 경우에 한하여 true를 반환하여,  
암묵적 타입 변환을 하지 않기에 예측하기 쉽다.

일치 비교 연산자에서 주의할 것은 NaN이다.  
NaN은 자신과 일치 하지 않는 유일한 값이다.

```
NaN === NaN // false
```

  
NaN은 자신과 일치 하지 않는 유일한 값이기에, 숫자가 NaN인지 조사하려면 빌트인 함수 Number.isNaN 을 사용해야 한다.

```
Number.isNaN
```

Number.isNaN 함수는 지정한 값이 NaN인지 확인하고 그 결과를 불리언 값으로 반환한다.

```
Number.isNaN(NaN); // true
Number.isNaN(10); // false
Number.isNaN(1 + undefined); // true
```

  
숫자 0도 주의해야 한다. 자바 스크립트에는 양의 0과 음의 0이 있는데 이들을 비교하면 모두 true를 반환한다.

```
0 === -0; //true
0 == -0; // true
```

\* ES6부터 도입된 Object.is 메서드는 예측 가능한 정확한 비교 결과를 반환.  
그 외에는 일치 비교 연산자(===)와 동일하게 동작한다.

```
-0 === +0; // true
Object.is(-0, +0); // false
NaN === NaN; // false
Object.is(NaN, NaN); // true
```

**부동등 비교 연산자와 불일치 비교 연산자는 각각 동등 비교 연산자와 일치 비교 연산자의 반대 개념이라 이해하면 된다.**

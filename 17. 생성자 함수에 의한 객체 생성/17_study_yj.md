## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 17.1 Object 생성자 함수

new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환

생성자 함수 : new 연산자와 함꼐 호출하여 객체(인스턴스)를 생성하는 함수

인스턴스 : 생성자 함수에 의해 생성된 객체

```javascript
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = "Lee";
person.sayHello = function () {
  console.log("Hi! My name is " + this.name);
};

console.log(person); // {name: "Lee", sayHello: ƒ}
person.sayHello(); // Hi! My name is Lee
```

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성

동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적

```javascript
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle2.getDiameter()); // 20
```

- 객체의 프로퍼티 값은 다르지만 메서드는 동일

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 마치 템플릿(클래스)처럼 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있음

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

**this**

this : 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수

this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/f8866b94-6f73-44c6-8733-a7b24a2ee896)

```javascript
// 함수는 다양한 방식으로 호출될 수 있다.
function foo() {
  console.log(this);
}

// 일반적인 함수로서 호출
// 전역 객체는 브라우저 환경에서는 window, Node.js 환경에서는 global을 가리킨다.
foo(); // window

// 메서드로서 호출
const obj = { foo }; // ES6 프로퍼티 축약 표현
obj.foo(); // obj

// 생성자 함수로서 호출
const inst = new foo(); // inst
```

일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작

만약 new 연산자와 함꼐 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작

```javascript
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

### 17.2.3 생성자 함수의 인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩
   - 암묵적으로 빈 객체가 생성
   - 이 빈 객체, 즉 인스턴스는 this에 바인딩 됨
   - **바인딩** : 식별자와 값을 연결하는 과정을 의미

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

2. 인스턴스 초기화
   - 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행
   - this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

3. 인스턴스 반환
   - 생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환
   - 만약 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시된 객체가 반환
   - 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환
   - 따라서 생성자 함수 내부에서 return 문을 반드시 생략해야 함

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 암묵적으로 this를 반환한다.
  // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
  return {};
}

// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // {}
```

### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출

```javascript
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();
```

[[Call]]을 갖는 함수 객체 → callable : 호출할 수 있는 객체, 즉 함수

[[Construct]]를 갖는 함수 객체 → constructor : 생성자 함수로서 호출할 수 있는 함수

[[Construct]]를 갖지 않는 함수 객체 → non-constructor : 객체를 생성자 함수로서 호출할 수 없는 함수

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/e28bc863-a2f8-4cb3-a04c-259148c8df5a)

모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아님

### 17.2.5 constructor와 non-constructor의 구분

함수가 어디에 할당되어 있는지에 따라 메서드인지를 판단하는 것이 아니라 함수 정의 방식에 따라 constructor와 non-constructor를 구분

- constructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수)
- non-constructor : 메서드(ES6 메서드 축약 표현), 화살표 함수

```javascript
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
  x: function () {},
};

// 일반 함수로 정의된 함수만이 constructor이다.
new foo(); // -> foo {}
new bar(); // -> bar {}
new baz.x(); // -> x {}

// 화살표 함수 정의
const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만을 메서드로 인정한다.
const obj = {
  x() {},
};

new obj.x(); // TypeError: obj.x is not a constructor
```

- non-constructor인 함수 객체를 생성자 함수로서 호출하면 에러 발생

### 17.2.6 new 연산자

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

- Circle 함수를 new 연산자와 함께 생성자 함수로서 호출하면 함수 내부의 this는 Circle 생성자 함수가 생성할 인스턴스를 가리킴
- Circle 함수를 일반적인 함수로서 호출하면 함수 내부의 this는 전역 객체 window를 가리킴
- 생성자 함수는 일반적으로 첫 문자를 대문자로 기술하는 파스칼 케이스로 명명하여 일반 함수와 구별

### 17.2.7 new.target

함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인 가능

new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킴

new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined

```javascript
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

- 함수 내부에서 new.target을 사용하여 new 연산자와 생성자 함수로서 호출했는지 확인
- 그렇지 않은 경우 new 연산자와 함께 재귀 호출을 통해 생성자 함수로서 호출

## 🤔궁금한 점

예제 17-17 코드

```javascript
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
  return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하지 않았으므로 반환문이 무시된다. 따라서 빈 객체가 생성되어 반환된다.
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
  return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser("Lee", "admin");
// 함수가 생성한 객체를 반환한다.
console.log(inst); // {name: "Lee", role: "admin"}
```

- 무엇을 설명하려는 코드인지 모르겠음
- 위 코드에서 일반 함수를 new 연산자와 함께 사용해서 생성자 함수가 된건지?
- 17.2.3에서 return에 명시된 값이 있으면 생성자 함수의 동작을 훼손하기 때문에 생략해야한다고 했는데 this가 없으면 괜찮은 것인지?

## 📌중요한 점

생성자 함수는 프로퍼티 구조가 동일한 객체를 여러 개 생성하기에 좋음, 템플릿처럼 사용

생성자 함수의 인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩 : 인스턴스는 this에 바인딩
2. 인스턴스 초기화 : this에 바인딩 되어있는 인스턴스를 초기화
3. 인스턴스 반환 : 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환

함수 객체는 `callable이면서 constructor`이거나 `callable이면서 non-constructor`임

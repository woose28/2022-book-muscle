## 25장. 클래스

자바스크립트는 `프로토타입 기반 객체지향 언어`이다.
프로토타입 기반 객체지향 언어는 `클래스가 필요 없는` 객체지향 프로그래밍 언어다.
`ES5`에서는 클래스 없이도 다음과 같이 `생성자함수` 와 `프로토타입` 을 통해 객체지향 언어의 `상속` 을 구현할 수도 있다.

```jsx
// ES5 생성자 함수
const Person = (function () {
  //생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log("Hi! My name is " + this.name);
  };

  // 생성자 함수 반환
  return Person;
})();

//인스턴스 생성
const me = new Person("Marco");
me.sayHi(); //"Hi! My name is Marco"
```

`ES6` 에서 도입된 클래스는 사실 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 `문법적 설탕` 이라고 볼 수도 있으나, 클래스의 extends와 super 키워드는 상속 관계 구현을 더욱 간결하고 명료하게 하므로 `새로운 객체 생성 메커니즘`으로 보는 것이 좀 더 합당하다.

- 클래스는 생성자 함수보다 엄격하며 생성자 함수에서는 제공하지 않는 기능도 제공한다.
  - 클래스를 new 연산자 없이 호출하면 에러가 발생한다. 하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.
  - 클래스는 상속을 지원하는 extends와 super 키워드를 제공한다. 하지만 생성자 함수는 extends와 super키워드를 지원하지 않는다.
  - 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이 발생하고, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
  - 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다. 하지만 생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.
  - 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다. 다시 말해, 열거되지 안흔ㄴ다.
- 클래스 정의 : class 키워드 사용, 파스칼 케이스 사용(첫글자 대문자), 일급 객체(표현식으로 정의 가능, 값으로 사용 가능)

  - 클래스 몸체에 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드의 세 가지가 있다.

  ```jsx
  //클래스 선언문
  class Person {
    //생성자
    constructor(name) {
      //인스턴스 생성 및 초기화
      this.name = name;
    }

    //프로토타입 메서드
    sayName() {
      console.log(`Hi! My name is ${this.name}`);
    }

    //정적 메서드
    static sayHello() {
      console.log("Hello!");
    }
  }

  //인스턴스 생성
  const me = new Person("Marco");

  //인스턴스의 프로토타입 참조
  console.log(me.name); //"Marco"

  //프로토타입 메서드 호출
  me.sayName(); //"Hi! My name is Marco"
  me.sayHello(); //"TypeError: me.sayHello is not a function

  //정적 메서드 호출
  Person.sayHello(); //"Hello!"
  ```

- 클래스 호이스팅
  - 클래스는 let, const 키워드로 선언한 변수처럼 `호이스팅된다`. 클래스 선언문 이전에 일시적 사각지대에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다. 사실 var, let, const, function, function\*, class 키워드를 사용하여 선언된 모든 식별자는 호이스팅된다. 모든 선언문은 런타임 이전에 먼저 실행되기 때문이다.
- 인스턴스 생성
  - 클래스는 생성자 함수이며 `new 연산자`와 함께 호출되어 인스턴스를 생성한다.
- 메서드

  - 클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다. 클래스 몸체에서 정의할 수 있는 메서드는 `constructor(생성자)`, `프로토타입 메서드`, `정적 메서드`의 세 가지가 있다.

    - `constructor` : constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다. constructor는 클래스 내에 최대 한 개만 존재할 수 있고 생략할 수 있다. constructor 내부에서 return 문은 반드시 생략한다.
      - `클래스의 constructor 메서드`와 `프로토타입의 constructor 프로퍼티`는 이름이 같아 혼동하기 쉽지만 `직접적인 관련이 없다`. 프로토타입의 constructor 프로퍼티는 모든 프로토타입이 가지고 있는 프로퍼티이며, 생성자 함수를 가리킨다.
    - `프로토타입 메서드` : 클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식(ex, 클래스명.prototype.메서드명 = function() {)과 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도(메서드명() { ) 기본적으로 프로토타입 메서드가 된다.
    - `정적(static) 메서드` : 인스턴스를 생성하지 않아도 호출할 수 있는 메서드이다. 클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드(클래스 메서드)가 된다. 정적 메서드는 프로토타입 메서드처럼 인스턴스로 호출하지 않고 클래스로 호출한다.

      - 정적 메서드와 프로토타입 메서드의 차이

        - 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 `프로토타입 체인`이 다르다.
        - 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
        - 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

        ```jsx
        class Square {
          // 정적 메서드
          static area(width, height) {
            return width * height;
          }
        }

        console.log(Square.area(10, 10)); //100
        ```

        ```jsx
        class Square {
          constructor(width, height) {
            this.width = width;
            this.height = height;
          }
          //프로토타입 메서드
          area() {
            return this.width * this.height;
          }
        }

        const square = new Square(10, 10);
        console.log(square.area()); //100
        ```

- 클래스의 인스턴스 생성 과정

  ```jsx
  class Person {
    // 생성자
    constructor(name) {
      //1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
      console.log(this);
      console.log(Object.getPrototypeOf(this) === Person.prototype);

      //2. this에 바인딩되어 있는 인스턴스를 초기화한다.
      this.name = name;

      //3. 완성된 인스턴스가 바인딩된 this로 암묵적으로 반환된다.
    }
  }
  ```
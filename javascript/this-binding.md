# this 바인딩(This Binding)이란?

`this`는 **함수가 호출될 때 그 함수가 속한 객체를 가리키는 키워드**이다. `this`는 함수가 어디서 선언되었는지가 아니라, **어떻게 호출되었는지**에 따라 동적으로 결정된다.

## 호출 방식에 따른 this 바인딩

### 1. 일반 함수 호출

```javascript
function greet() {
  console.log(this); // window (또는 global)
}

greet();
```

### 2. 메서드 호출

```javascript
const person = {
  name: 'Alice',
  greet() {
    console.log(this.name); // this === person
  },
};

person.greet(); // "Alice" 출력
```

### 3. 생성자 함수 호출 (new 키워드)

```javascript
function Person(name) {
  this.name = name; // this는 새로운 인스턴스
}

const alice = new Person('Alice');
```

### 4. 화살표 함수

```javascript
const person = {
  name: 'Alice',
  greet: () => {
    console.log(this); // 전역 객체 (상위 스코프의 this 상속)
  },
};

person.greet(); // window (또는 global)
```

화살표 함수는 자신만의 `this`를 갖지 않고, **선언 시점의 상위 스코프 this를 상속받는다.**

## call(), apply(), bind() - 명시적 바인딩

함수의 `this`를 직접 지정할 수 있다.

```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}

const person = { name: 'Alice' };

greet.call(person, 'Hello'); // "Hello, Alice" - 즉시 실행
greet.apply(person, ['Hello']); // "Hello, Alice" - 배열로 인자 전달
const bound = greet.bind(person); // 새 함수 반환
bound('Hello'); // "Hello, Alice" - 나중에 실행
```

| 메서드    | 특징                             |
| --------- | -------------------------------- |
| `call()`  | 즉시 실행, 인자를 쉼표로 분리    |
| `apply()` | 즉시 실행, 인자를 배열로 전달    |
| `bind()`  | 새 함수 반환, 인자를 쉼표로 분리 |

## 자주하는 실수

### 콜백에서 this 손실

```javascript
const person = {
  name: 'Alice',
  hobbies: ['reading', 'gaming'],
  printHobbies() {
    this.hobbies.forEach(function (hobby) {
      console.log(this.name); // undefined (this 손실)
    });
  },
};
```

**해결: 화살표 함수 또는 bind() 사용**

```javascript
// 방법 1: 화살표 함수 (상위 this 상속)
this.hobbies.forEach((hobby) => {
  console.log(this.name);
});

// 방법 2: bind()
this.hobbies.forEach(
  function (hobby) {
    console.log(this.name);
  }.bind(this),
);
```

### 화살표 함수를 메서드로 사용하면 안됨

```javascript
const person = {
  name: 'Alice',
  greet: () => console.log(this.name), // this는 전역 객체
};

// 대신 일반 함수 사용
const person = {
  name: 'Alice',
  greet() {
    console.log(this.name);
  }, // "Alice" 출력
};
```

## 정리

| 상황            | this 바인딩        |
| --------------- | ------------------ |
| 일반 함수       | 전역 객체          |
| 메서드          | 메서드 호출 객체   |
| new (생성자)    | 새로운 인스턴스    |
| call/apply/bind | 명시적 지정        |
| 화살표 함수     | 상위 스코프의 this |

## 면접 답변용 한 줄 정리

this는 선언 위치가 아니라 호출 방식으로 결정되며, 일반 호출·메서드 호출·new 호출·명시적 바인딩(call/apply/bind)·화살표 함수 상속 규칙을 구분하면 된다.

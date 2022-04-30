# 9장 스택과 큐로 간결한 코드 생성

스택과 큐는 제약을 갖는 배열이고, 제약 덕분에 자료 구조가 매우 간결해진다. 임시 데이터를 처리하되 데이터를 처리하는 순서에 특히 중점을 둔다. 

## 9.1 스택

스택이 데이터를 저장하는 방법은 배열과 같지만, 세 가지 제약을 갖는다. 

- 데이터는 스택의 끝에만 삽입할 수 있다.
- 데이터는 스택의 끝에서만 삭제할 수 있다. (pop)
- 스택의 마지막 원소만 읽을 수 있다.

즉 Last In, First Out(LIFO)다!

## 9.2 추상 데이터 타입

스택 데이터 구조는 배열과 달리 추상 데이터 타입에 속한다. 배열은 대부분의 언어에 내장되어 있고 컴퓨터 메모리와 직접 상호작용하지만, 스택은 다른 내장 데이터 구조를 사용하는 이론적 규칙 집합으로 이뤄진 데이터 구조다. 

## 9.3 스택 다뤄보기

임시 데이터를 다뤄야 하는 다양한 알고리즘에서 스택은 유용한 도구가 될 수 있다. 

코드 줄을 검사해서 괄호 문법 오류가 없는지 확인하는 린터 알고리즘을 구현해보자. 

문법 오류는 세 가지 상황에서 발생한다.

1. 여는 괄호는 있는데 닫는 괄호가 없는 경우
    - (var x = 2;
2. 여는 괄호가 앞에 나오지 않았는데 닫는 괄호가 나오는 경우
    - var x = 2);
3. 닫는 괄호가 바로 앞에 나온 여는 괄호와 종류가 다를 경우
    - (var x = [1, 2, 3)]

```jsx
class Stack {
  constructor() {
    this.data = [];
  }
  push(item) {
    this.data.push(item);
  }
  pop() {
    return this.data.pop();
  }
  read() {
    return this.data[this.data.length - 1];
  }
}

class Linter extends Stack {
  lint(text) {
    for (let i = 0; i < text.length; i++) {
      if (this.isOpeningBrace(text[i])) {
        super.push(text[i]);
      } else if (this.isClosingBrace(text[i])) {
        let poppedOpeningBrace = this.pop();

        if (!poppedOpeningBrace) {
          return `${text[i]} doesn't have opening brace`;
        }

        if (this.isNotAMatch(poppedOpeningBrace, text[i])) {
          return `${text[i]} has  mismatched opening brace`;
        }
      }
    }
    if (super.read()) {
      return `${super.read()} does not have closing brace`;
    }

    return true;
  }

  isOpeningBrace(char) {
    return ["(", "{", "["].includes(char);
  }

  isClosingBrace(char) {
    return [")", "}", "]"].includes(char);
  }

  isNotAMatch(openingBrace, closingBrace) {
    return closingBrace !== { "(": ")", "{": "}", "[": "]" }[openingBrace];
  }
}
```

## 9.4 제약을 갖는 데이터 구조의 중요성

배열 말고 스택처럼 제약을 갖는 데이터 구조가 중요한 이유는 무엇일까? 

1. 제약을 갖는 데이터 구조를 사용하면 잠재적 버그를 막을 수 있다. 
2. 스택 같은 데이터 구조는 문제를 해결하는 새로운 사고 모델을 제공한다. 

## 9.5 큐

큐 역시 추상 데이터 타입이다. 하지만 스택과 달리 First In, Fist Out(FIFO)로 동작한다. 스택과 마찬가지로 세 가지 제약을 갖는다. 

- 데이터는 큐의 끝에만 삽입할 수 있다. (스택과 동일)
- 데이터는 큐의 앞에서만 삭제할 수 있다. (스택과 정반대 동작)
- 큐의 앞에 있는 원소만 읽을 수 있다. (스택과 정반대 동작)

```jsx
class Queue {
  constructor() {
    this.data = [];
  } 
  enqueue(element) {
    this.data.push(element);
  }
  dequeue() {
    this.data.shift();
  }
  read() {
    return this.data[0]
  }
}
```

## 9.6 큐 다뤄보기

프린터가 네트워크상에 있는 여러 컴퓨터로부터 출력 잡을 받아들일 수 있도록 프로그래밍해보자. 요청받은 순서대로 각 문서를 출력해야 한다. 

```jsx
class Queue {
	constructor() {
    this.data = [];
  } 
  enqueue(element) {
    this.data.push(element);
  }
  dequeue() {
    return this.data.shift();
  }
  read() {
    return this.data[0]
  }
}

class PrintManager extends Queue {
  queuePrintJob(document) {
    super.enqueue(document)
  }
  run() {
    while (super.read()) {
      this.print(super.dequeue())
    }
  }
  print(document) {
    console.log(document)
  }
}
```

이처럼 큐는 요청받은 순서대로 요청을 처리하므로 비동기식 요청을 처리하는 완벽한 도구이기도 하다. 

## 오늘의 Quiz

스택을 사용해 문자열을 거꾸로 만드는 함수를 작성하라. 

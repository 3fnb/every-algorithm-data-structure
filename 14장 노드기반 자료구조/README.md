## 연결리스트(Linked List)

연결리스트는 배열과 마찬가지로 리스트 형식의 자료구조이지만, 메모리 상에서 연속된 메모리를 차지하지 않는다는 점에서 차이점이 있다. 연결리스트는 리스트 요소가 노드로 구성되어 있고 해당 노드는 다음 노드의 주소를 가리키는 `링크(Link)`라는 포인터를 가지고 있다. 덕분한 메모리가 순차적으로 나열되어있지 않아도 다음 요소의 값에 접근할 수 있다. 가장 첫 노드를 `헤드(Head)`, 마지막 노드를 `테일(Tail)`이라고도 부른다.

연결리스트는 단일 연결리스트(Singly Linked List)와 이중 연결리스트(Doubly Linked List)로 나뉜다. 이중연결 리스트는 단일과 다르게 이전 노드의 주소값 도한 가지고 있다. 따라서 앞쪽에서 부터 순차적으로 넘어가면서 값을 찾는 것 뿐만 아니라 가장 뒤에서 부터 값을 찾아 나갈 수도 있다.

<br>

### 구현 코드

구현 방법은 보통 클래스를 이용한다.

```jsx
class NodeS {
  constructor(data) {
    this.data = data;
    this.nextNode = '';
  }
}

class SinglyLinkedList {
  constructor(firstNode) {
    this.firstNode = firstNode;
  }

  consoleList() {
    let currentNode = this.firstNode;
    let currentIndex = 0;

    do {
      console.log(currentIndex, currentNode.data);
      currentNode = currentNode.nextNode;
      currentIndex++;
    } while (currentNode);
  }

  read(index) {
    let currentNode = this.firstNode;
    let currentIndex = 0;

    while (currentIndex < index) {
      currentNode = currentNode.nextNode;
      currentIndex++;
      if (!currentNode) return null;
    }

    return currentNode.data;
  }

  search(keyWord) {
    let currentNode = this.firstNode;
    let currentIndex = 0;

    do {
      if (currentNode.data === keyWord) return currentIndex;
      currentNode = currentNode.nextNode;
      currentIndex++;
    } while (currentNode);

    return null;
  }

  insertAtList(index, value) {
    const newNode = new NodeS(value);

    if (index === 0) {
      newNode.nextNode = this.firstNode;
      return;
    }

    let currentNode = this.firstNode;
    let currentIndex = 0;

    while (currentIndex < index - 1) {
      currentNode = currentNode.nextNode;
      currentIndex++;
    }

    newNode.nextNode = currentNode.nextNode;
    currentNode.nextNode = newNode;
    return;
  }

  deleteAtIndex(index) {
    if (index === 0) {
      this.firstNode = this.firstNode.nextNode;
      return;
    }

    let currentNode = this.firstNode;
    let currentIndex = 0;

    while (currentIndex < index - 1) {
      currentNode = currentNode.nextNode;
      currentIndex++;
    }

    currentNode.nextNode = currentNode.nextNode.nextNode;
  }
}
```

<br>

## 연결리스트 연산의 효율성

단순히 읽기만 한다면 해시테이블을 사용하는 배열에 비해서 매리트가 없다. 하지만 연결리스트를 사용하는 이유는 삽입과 삭제에 있다. 배열은 가장 앞에 요소를 추가해주는 과정에서 값을 하나씩 미뤄야하는 바람에 비효율적이었다. 반면 연결 리스트는 앞에 값을 추가할 때에도 1단계로 끝낼 수 있다. 물론 끝에 값을 추가하기 위해서는 해당 노드까지 접근하기 위해 모든 리스트를 순회하여야 하지만 이중연결리스트를 사용하면 이 또한 어느정도 개선이 가능하다.

# 15장 이진 탐색 트리로 속도 향상

정렬된 배열은 삽입과 삭제시 다른 값들을 한 셀씩 옮겨야 하기 때문에 느리다. 

해시 테이블은 전반적으로 빠른 속도를 내지만 순서를 유지하지 못한다. 

순서를 유지하면서도 빠른 검색과 삽입, 삭제가 가능한 자료 구조로 ‘이진 탐색 트리'가 있다. 

### 15.1 트리

- 트리는 노드 기반 자료 구조이지만 연결 리스트와 달리 각 노드는 여러 노드로의 링크를 포함할 수 있다.
- 가장 상위 노드는 ‘루트'라고 하며, 자손 부모 조상의 관계를 갖는다.
- 같은 줄은 같은 레벨이다.
- 트리의 프로퍼티는 균형 잡힌 정도를 말하며, 모든 노드에서 하위 트리의 노드 개수가 같으면 균형 트리다.

### 15.2 이진 탐색 트리

- 이진 트리: 각 노드에 자식이 0개나 1개, 2개다.
- 이진 탐색 트리: 각 노드의 자식은 최대 왼쪽 하나 오른쪽 하나며, 왼쪽 자손은 그 노드보다 작은 값만 포함하고 오른쪽 자손은 그 노드보다 큰 값만 포함할 수 있다.

```jsx
class Node {
    constructor(data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }
}
```

### 15.3 검색

1. 루트 노드부터 검색한다.
2. 찾고 있는 값이 현재 노드보다 작으면 왼쪽 하위 트리, 크면 오른쪽 하위 트리를 검색한다. 
3. 트리 바닥에 닿을 때까지 반복한다. 

즉, 각 단계마다 절반을 제거한다. 따라서 이진 탐색 트리 검색은 O(logN)이다.(포화 균형 이진 탐색 트리에서)

거꾸로 생각하면, 트리의 모든 줄이 빈칸 없이 노드로 채워져 있을 때 트리 레벨을 새로 추가할 때마다 트리 크기가 두 배가 된다. 따라서 노드가 N개인 트리에서 모든 자리마다 노드를 두려면 log(N) 레벨이 필요하다. 

```jsx
class Node {
    constructor(value){
        this.value = value
        this.left = null
        this.right = null
    
    }

}
class BinarySearchTree {

    constructor(){
      this.root = null
    }
 
  find(value){
      if(!this.root) return false
      
      let current = this.root //현재 노드
      let found = false
      while(current && !found){
            if(value < current.value){ //찾고 있는 값이 현재 노드보다 작으면 왼쪽 자식 검색
              current = current.left
             } else if(value > current.value){ //찾고 있는 값이 현재 노드보다 작으면 오른쪽 자식 검색
                current = current.right
             } else {
                  found = current
             } 
            
            }
    
        if(!found) return undefined;
        return found
      
  
  }
}
```

검색의 효율성은 정렬된 배열의 이진 검색과 효율성이 같다. 하지만 삽입에 있어서는 이진 트리가 훨씬 뛰어나다. 

 

### 15.4 삽입

삽입에는 logN(검색) + 1(삽입) 단계가 걸리며 빅 오는 상수를 무시하므로O(logN)이다. 

정렬된 배열은 값을 삽입할 공간을 마련하기 위해 데이터를 계속 시프트해야 하기 때문에 삽입에 O(N)이 걸린다. 

따라서 데이터를 많이 변경하려면 이진 탐색 트리가 매우 효율적이다. 

```jsx
class Node {
  constructor(value){
      this.value = value
      this.left = null
      this.right = null
  }
}

class BinarySeachTree {
      constructor(){
        this.root = null
      }
  
  insert(value){
        var newNode = new Node(value);
        if(this.root === null){
            this.root = newNode;
            return this;
        }
        let current = this.root;
        while(current){
            if(value === current.value) return undefined;
            if(value < current.value){
                if(current.left === null){ //왼쪽 자식이 없으면 왼쪽 자식으로서 값을 삽입
                    current.left = newNode;
                    return this;
                }
                current = current.left;
            } else {
                if(current.right === null){ //오른쪽 자식이 없으면 오른쪽 자식으로서 값을 삽입
                    current.right = newNode;
                    return this;
                } 
                current = current.right;
            }
        }
    }
}
```

오로지 균형 트리일 때만 검색에 O(logN)이 걸린다. 이를 주의하자. 따라서 정렬된 배열을 이진 탐색 트리로 변환하고 싶을 대는 먼저 데이터 순서를 무작위로 만드는게 좋다. 

### 15.5 삭제

가장 어려운 연산…!!!!!이다. 

- 삭제할 노드에 자식이 없으면 그냥 삭제한다.
- 삭제할 노드에 자식이 하나면 노드를 삭제하고 자식을 삭제된 노드가 있던 위치에 넣는다.
- 자식이 둘인 노드를 삭제할 때는 삭제된 노드를 후속자 노드로 대체한다. 후속자 노드란 삭제된 노드보다 큰 값 중 최솟값을 갖는 자식 노드다.
- 후속자 노드를 찾으려면 삭제된 값의 오른쪽 자식을 방문해서 그 자식의 왼쪽 자식을 따라 계속해서 방문하며 더 이상 왼쪽 자식이 없을 때까지 내려간다. 바닥 값이 후속자 노드다.
- 만약 후속자 노드에 오른쪽 자식이 있으면 후속자 노드를 삭제된 노드가 있던 자리에 넣은 후, 후속자 노드의 오른쪽 자식을 후속자 노드의 원래 부모의 왼쪽 자식으로 넣는다.

```jsx
class Node {
    constructor(value){
        this.value = value
        this.left = null
        this.right = null
    
    }

}
class BinarySearchTree {

    constructor(){
      this.root = null
    }
    
    remove(value){
        this.root = this.removeNode(this.root, value)
    }
    
    removeNode(current, value) {
        
        // 노드가 실제로 존재하지 않을 때가 기저 조건
        
       if(current === null) return current
        
        // 현재 노드가 삭제하려는 노드인 경우
        
        if (value === current.value) {
            
            if (current.left === null && current.right === null){
                
                    return null
                
                }else if(current.left === null){
                // 현재 노드에 왼쪽 자식이 없으면 오른쪽 자식을 그 부모의 새 하위 트리로 반환함으로써 현재 노드 삭제
                
                    return current.right 
             
                }else if(current.right === null){
                
                    return current.left
                
                }else{
                //현재 노드에 자식이 둘이면 현재 노드의 값을 후속자 노드의 값으로 바꾸는 함수 호출함으로써 현재 노드 삭제
                    
                    let tempNode = this.kthSmallestNode(current.right)
                        current.value = tempNode.value                    
                        current.right = this.removeNode(current.right, tempNode.value)
                    return current
            }
            // 삭제하려는 값이 현재 노드보다 작거나 크면 현재 노드 왼쪽 혹은 오른쪽 하위 트리에 대한
            // 재귀 호출의 반환 값을 각각 왼쪽 혹은 오른쪽 자식에 할당한다.
            
        }else if(value < current.value) {
            
            current.left = this.removeNode(current.left, value)
            // 현재 노드(와 존재한다면 그 하위 트리)를 반환해서
            // 현재 노드의 부모의 왼쪽 혹은 오른쪽 자식의 새로운 값으로 쓰이게 한다.
            return current
            
        }else{
            
            current.right = this.removeNode(current.right, value)
            return current
        }
    }
    
    kthSmallestNode(node) {
        while(!node.left === null)
            node = node.left
            // 현재 노드에 왼쪽 자식이 없으면 
            // 이 함수의 현재 노드가 후속자 노드라는 뜻이므로 현재 노드의 값을 삭제하려는 노드의 새로운 값으로 할당한다.

        return node
    }

    
}
```

삭제도 일반적으로 O(logN)이다. 삭제에는 검색 한 번과 연결이 끊긴 자식을 처리하는 단계가 추가로 필요하기 때문이다. 

### 15.7 이진 탐색 트리 순회

중위 순회를 수행해 트리를 알파벳 오름차순으로 순회해보자. 

```jsx
const inOrderHelper = (root) => {
   if (root !== null) {
      inOrderHelper(root.left);
      console.log(root.data);
      inOrderHelper(root.right);
   }
}
```

순회는 트리의 노드 N개를 모두 방문하므로 O(N)이 걸린다.

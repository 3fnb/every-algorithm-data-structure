알고리즘은 특정 과제를 달성하기 위해 컴퓨터에 제공되는 명령어 집합을 뜻한다. 

# 정렬된 배열
- 정렬된 배열: 값이 항상 순서대로 있어야 한다. 

정렬된 배열에 삽입을 할 때는 실제 삽입 전에 검색을 수행해 삽입할 올바른 위치를 정해야 한다. 

## 선형 검색
1. 원하는 값을 찾을 때까지 왼쪽에서 오른쪽으로 한 번에 한 셀씩 확인한다. 
2. 삽입하려는 수보다 큰 수를 만난다. 
3. 마지막 값부터 만난 수까지 오른쪽으로 데이터를 옮긴다. 
4. 올바른 위치에 삽입한다. 

삽입에 필요한 단계 수는 새 값이 정렬된 배열 어디에 놓이게 되든 비슷하다. 비교와 이동은 반비례 관계다.
- 원소가 N개일 경우 N+1(비교와 이동) + 1(삽입) = 'N + 2' 단계가 걸린다. 
- 새 값이 배열 맨 끝에 놓이는 경우 이동이 없다. 따라서 'N(비교) + 1(삽입)' 단계가 걸린다. 

# 이진 검색
정렬된 배열이 전형적인 배열에 비해 좋은 점은 이진 검색 알고리즘을 사용해 훨씬 빠른 연산을 할 수 있다는 것이다. 

1. 가운데 셀부터 검색한다. 배열의 길이를 2로 나누어 접근한다. 
2. 가운데 셀을 기준으로 오른쪽 또는 왼쪽 셀을 제거한다. 
3. 선택한 쪽의 가운데 값을 확인한다.(두 개이면 임의로 선택한다)
4. 2-3을 반복하여 남은 셀을 확인한다. 

이진 검색은 정렬된 배열에만 사용할 수 있다. 

```
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const target = 3;
 
const binarySearch = (arr, target) => {
    let min = 0;
    let max = arr.length - 1;
    let count = 0;
 
    while (min <= max){
        count += 1;
        let mid = Math.floor((min + max) / 2);
 
        if (arr[mid] === target){
            return "찾았당!", count;
        }else{
            if (arr[mid] < target){
                min = mid + 1;
            }else{
                max = mid - 1;
            }
        }
    }
    return "없당!";
}

binarySearch(arr, target);
```
재귀함수
```
cosnt arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const target = 3;

const min = 0;
const max = arr.length - 1;
 
const binarySearch = (start, end, arr, target) => {
    if (start > end){
        return "없어요!";
    }
 
    let mid = Math.floor((start + end) / 2);
 
    if (arr[mid] === target){
        return "찾았당!";
    }else{
        if (arr[mid] < target){
            binarySearch(mid + 1, end, arr, target);
        }else{
            binarySearch(start, mid - 1, arr, target);
        }
    }
}

binarySearch(min, max, arr, target)
```

# 이전 검색 대 선형 검색
이진 검색은 배열의 크기를 두 배로 늘릴 때마다 이진 검색에 필요한 단계 수가 1씩 증가하는 패턴을 보인다. 
하지만 선형 검색은 원소 수만큼의 단계가 더 필요하다. 
따라서 정렬된 배열은 검색이 개발에 중대한 기능일 때 유리하다.

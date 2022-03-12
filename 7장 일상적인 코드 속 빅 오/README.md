# 우리가 접하는 실전 코드속의 빅 오

최적화의 시작: 현재 코드가 얼마나 빠른지 알아야 한다.  
빅 오 표기법 관점에서 어떤게 효율적일까?

<br>

## 짝수의 평균

숫자 배열을 받아 짝수의 평균을 반환

```jsx
const averageOfEvenNumbers = array => {
  let sum = 0;
  let countOfEvenNumbers = 0;

  for (let i = 0; i < array.length; i++) {
    if (array[i] % 2 === 0) {
      sum += array[i];
      countOfEvenNumbers += 1;
    }
  }

  return sum / countOfEvenNumbers;
};
```

1. 알고리즘의 핵심은 얼마나 순회하는가이다.
2. 최악일 경우 데이터 원소가 N일때 3N+3이다.  
   (3N은 비교하고, 짝수일경우 두 단계를 더 실행하고, +3은 0을 할당하는 변수 두개, 나누기 단계)

O(N)

<br>

## 단어 생성기

문자 배열을 받아 두 글자짜리 모든 문자열 조합을 반환

```jsx
const wordBuilder = array => {
  let collection = [];
  for (let i = 0; i < array.length; i++) {
    for (let j = 0; j < array.length; j++) {
      if (i !== j) collection.push(array[i] + array[j]);
    }
  }
};
```

중첩이 두번. O(N^2)

만약 세단어를 조합하는 함수였다면 O(N^3)일것이다. 중첩루프가 네 다섯개라면 기하수급적으로 효율이 떨어지기에 한단계만 낮춰도 대단한 성과.

<br>

## 배열 예제

아주 큰 배열을 받아 맨 앞,맨뒤,중간 값을 반환하는 함수

```jsx
const makeSample = array => {
  first = array[0];
  middle = array[Math.floor(array.length / 2)];
  last = array[array.length - 1];
  return [first, middle, last];
};
```

배열의 조회는 한번에 가능하므로 O(1)

<br>

## 평균 섭씨 온도 구하기

화씨 -> 섭씨 -> 평균

```jsx
const averageCelsius = fahrenheitReadings => {
  const celsiusNumbers = [];
  for (const fahrenheitReading of fahrenheitReadings) {
    const celsiusConversion = (fahrenheitReading - 32) / 1.8;
    celsiusNumber.push(celsiusConversion);
  }

  let sum = 0;
  for (const celsiusNumber of celsiusNumbers) {
    sum += celsiusNumber;
  }

  return sum / celsiusNumbers.length;
};
```

N+N -> O(N)

<br>

## 의류 상표

문자열을 입력받아 배열에 맵핑시키는 함수

```jsx
const markInventory = clothingItems => {
  const clothingOptions = [];
  for (const item in clothingItems) {
    for (let i = 0; i < 6; i++) {
      clothingOptions.push(`${item} Size ${i}`);
    }
  }
  return clothingOptions;
};
```

이중 중첩 -> O(N^2)  
하지만 위의 예제에서는 아니다.  
N번씩에서 N번 루프가 돌아야 N^2이지만 중첩되어 있는 루프는 무조건 5번만 실행하기에 알고리즘은 5N번 실행된다.  
따라서 O(N)

<br>

## 1 세기

언뜻 보기와 다른것들이 있다  
숫자 배열들이 들어있는 배열을 받아 1의 갯수를 반환하는 함수

outerArray 는 [ [1,0,1],[0,0,1,0,0],[1,1,1] ]와 같이 주어진다.

```jsx
const countOnes = outerArray => {
  let count = 0;
  for (const innerArray in outerArray) {
    for (const number in innerArray) {
      if (number === 1) sum += 1;
    }
  }
  return count;
};
```

바깥 배열은 무조건 한번이므로,,, O(N)이다.

<br>

## 팰린드롬 검사기

팰린드롬은 앞으로 읽으나 뒤로 읽으나 똑같은 단어,구절.  
"racecar" , "tenet"과 같다.  
팬린드롬 구분 함수.

```jsx
const isPalindrome = string => {
  let leftIndex = 0;
  let rightIndex = string.length - 1;

  while (leftIndex < string.length / 2) {
    if (string[leftIndex] !== string[rightIndex]) return false;

    leftIndex++;
    rightIndex--;
  }

  return true;
};
```

상수를 무시하는 빅 오 표기법에 의하면 O(N)이다. N/2이기 떄문.

## 모든 곱 구하기

숫자 배열을 받아 모든 두 숫자 조합의 곱을 반환하는 함수.

```jsx
const twoNumberProducts = array => {
  let products = [];
  for (let i = 0; i < array.length - 1; i++) {
    for (let j = i + 1; j < array.length; j++) {
      products.push(array[i] * array[j]);
    }
  }
  return products;
};
```

직관적으로 봤을때 두번의 루프를 거치지 N^2이다.  
자세히 본다면 array.length가 5일때,  
i=0) j= 1,2,3,4 네번 실행  
i=1) j=2,3,4 세번 실행  
...  
루프는 N + N-1 + ... 1 번 실행되는데, 이것은 공식으로 1부터 n까지의 합은 n(n+1)/2  
상수를 무시한다면 O(N^2)이다.

## 여러 세트의 데이터 다루기

두개의 숫자배열을 받아 모든 가짓수의 곱 계산 수 배열로 리턴하는 함수

```jsx
const twoNumberProducts = (array1, array2) => {
  let products = [];
  for (let i = 0; i < array1.length; i++) {
    for (let j = 0; j < array2.length; j++) {
      products.push(array1[i] * array2[j]);
    }
  }
  return products;
};
```

퉁 쳐서 arr1,arr2의 크기가 같다고 생각한다면 N^2 이겠지만 ..  
O(N X M)으로 나타내는게 최선이다. N과 N^2 사이이다.

## 암호 크래커

브루트 포스방식으로 암호를 푸는 함수  
주어진 길이의 모든 문자열 조합을 생성하는 코드  
n이 3으로 주어지면 aaa 부터 zzz까지 문자열을 생성한다.

한자리부터 시작해보자  
1자리라면 26번이 걸린다.
2자리라면 26^2번이 걸린다. 두글짜짜리 조합을 생각해보자.  
3자리라면 26^3번이 걸린다.  
따라서 O(26^N)이다.

O(logN)과 역함수 관계에 있다. 엄청나게 비효율적인 알고리즘으로 브루트 포스가 왜 비효율적인지 알 수 있다.

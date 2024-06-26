## Problem 2: Even Fibonacci Numbers

Each new term in the Fibonacci sequence is generated by adding the previous two terms. By starting with 1 and 2, the first 10 terms will be:

1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...  

By considering the terms in the Fibonacci sequence whose values do not exceed `n`, find the sum of the even-valued terms.

### 조건
```
fiboEvenSum(10) should return a number.
Your function should return an even value.
Your function should sum the even-valued Fibonacci numbers: fiboEvenSum(8) should return 10.
fiboEvenSum(10) should return 10.
fiboEvenSum(34) should return 44.
fiboEvenSum(60) should return 44.
fiboEvenSum(1000) should return 798.
fiboEvenSum(100000) should return 60696.
fiboEvenSum(4000000) should return 4613732.
```

### 답변
```javascript
function fiboEvenSum(n) {
  const fibonacci = [1, 2];
  makeFibonacci(fibonacci, n);
  // 짝수 값만 골라서 합한다. 
  // 현재 요소 cur이 홀수라면 이전 반환값(0 또는 짝수의 합)을 반환하고, 즉 아무것도 안 하고
  // 짝수라면 값을 더해 반환한다. 
  return fibonacci.reduce((arr, cur) => cur % 2 ? arr : (arr + cur), 0);
}
// 피보나치 배열을 만드는 재귀 함수
// [1, 2]를 처음에 제공, 여기서 요소를 추가해나갈 것
// n이 재귀를 끊는 역할
// 피보나치의 마지막 요소가 n을 초과하지 말아야 한다
// => 뒤에서 마지막 두 가지 요소의 합이 n을 초과하지 않으면 재귀 호출, 
// 초과한다면 그대로 배열을 반환하며 재귀 종료
function makeFibonacci(arr, n) {
  if(arr[arr.length - 2] + arr[arr.length - 1] > n) {
    return arr;
  } else {
    arr[arr.length] = arr[arr.length - 2] + arr[arr.length - 1];
    return makeFibonacci(arr, n);
  }
}
```

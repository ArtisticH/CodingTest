## Problem 1: Multiples of 3 or 5
If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below the provided parameter value `number`.

### 조건
```
multiplesOf3Or5(10) should return a number.
multiplesOf3Or5(49) should return 543.
multiplesOf3Or5(1000) should return 233168.
multiplesOf3Or5(8456) should return 16687353.
multiplesOf3Or5(19564) should return 89301183.
```

### 답변
```javascript
function multiplesOf3Or5(number) {
  const arr = [];
  // 1부터 (주어진 수 - 1)까지 반복하면서 3으로 나누어떨어지거나 5로 나누어떨어지면 배열에 추가
  for(let i = 1; i < number; i++) {
    if(i % 3 === 0 || i % 5 === 0 ) {
      arr[arr.length] = i
    }
  }
  // reduce로 합계 추출
  return arr.reduce((arr, cur) => arr + cur, 0);
}

multiplesOf3Or5(19564);
```

## Problem 3: Largest prime factor
The prime factors of 13195 are 5, 7, 13 and 29.

What is the largest prime factor of the given `number`?

## 조건
```
largestPrimeFactor(2) should return a number.
largestPrimeFactor(2) should return 2.
largestPrimeFactor(3) should return 3.
largestPrimeFactor(5) should return 5.
largestPrimeFactor(7) should return 7.
```

## 답변
```javascript
// 소인수: 약수 중에 소수인 수
// 소수: 1과 자기 자신만을 약수로 가지는 수, 3, 5, 7, 11...

function largestPrimeFactor(number) {
  // 2. 약수들의 배열에서 소인수를 걸러낸다.
  console.log(divisor(number))
  const primes = divisor(number).filter(isDivisor);
  // 최대 값 찾기
  return Math.max(...primes);
}

largestPrimeFactor(13195);

// 1. 약수들을 배열로 만든다.
/*
만약 number = 24일때
1부터 시작해서 number로 나눌때 나머지가 0이면(딱 나누어 떨어지면)
splice를 통해 (length / 2)의 자리에 [나누는 수, 몫]을 삽입한다. 
만약 이제 2라면, 2는 24를 나눌때 나머지가 0이고 몫은 12이다. 
그럼 [1, 24]의 배열의 (length / 2)인 1의 자리에 [2, 12]을 삽입한다. 
이 반복문은 나누는 값인 i가 이미 배열에 있을때 멈춘다.
예를 들어 [1, 2, 3, 4, 6, 8, 12, 24] 일때 
i = 5는 조건에 안 맞아 i = 6이 되고,
6이 이미 배열에 존재하므로 반복문을 멈춘다. 

그리고 4의 약수를 [1, 2, 2, 4]가 아닌 
[1, 2, 4]로 나타내기 위해 
i ** 2 === number 조건을 추가한다. 
*/
function divisor(number) {
  const divisors = [];
  for(let i = 1; i <= number; i++) {
    if(divisors.includes(i)) return divisors;
    if(number % i === 0) {
      const middle = divisors.length / 2;  
      if(i ** 2 === number) {
        divisors.splice(middle, 0, i);
      } else {
        divisors.splice(middle, 0, i, number / i);
      }
    }
  }
}

// 2. 소인수 거르기
/*
약수들의 배열 예를 들면 [1, 2, 3, 4, 6, 8, 12, 24]를 
하나씩 돌면서
우선 1은 소수가 아니므로 제외
각 요소에 대해 다시 약수들을 만들때 그 배열 값이 2라면 소수이다.
2는 [1, 2], 3은 [1, 3]
4는 [1, 2, 4], 6은 [1, 2, 3, 6]...
*/
function isDivisor(item) {
  if(item === 1) return false;
  if(divisor(item).length === 2) {
    return true;
  } else {
    return false;
  }
}
```

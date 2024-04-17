## Problem 5: Smallest multiple
2520 is the smallest number that can be divided by each of the numbers from 1 to 10 without any remainder.

What is the smallest positive number that is evenly divisible by all of the numbers from 1 to `n`?

## 조건
```
smallestMult(5) should return a number.
smallestMult(5) should return 60.
smallestMult(7) should return 420.
smallestMult(10) should return 2520.
smallestMult(13) should return 360360.
smallestMult(20) should return 232792560.
```

## 답변
```javascript
/*
인수: 12일때 1, 2, 3, 4, 6, 12가 인수들
소인수: 인수 중에서 소수 인 것: 2, 3
소인수분해: 자연수를 소인수들의 곱으로 표현

소인수분해를 이용해 최소 공배수 구하는 법
1부터 5까지를 소인수분해하면
2: 2
3: 3
4: 2 ** 2
5: 5

여기서 공통인 소수 중에서 지수가 더 큰걸 쓴다: 2와 2 ** 2 중에서 2 ** 2 
공통이 아닌 소수는 다 쓴다: 3, 5
그래서 3 * 5 * (2 ** 2) = 60

[2, 3, 4, 5, 6, ...]에서
요소 6을 => [1, 2, 3, 6]처럼 약수의 배열로 만들고
거기서 => [2, 3]처럼 소수만 뽑는다. 
[ 2, 3, 4, 5, 6, 7, 8, 9, 10 ] => 
[ [ 2 ], [ 3 ], [ 2 ], [ 5 ], [ 2, 3 ], [ 7 ], [ 2 ], [ 3 ], [ 2, 5 ] ]
*/

// n = 10일때
function smallestMult(n) {
  // [2, 3, 4, 5, 6, 7, 8, 9, 10]
  const numbers = makeRange(n); // 📍1
  // [ [ 2 ], [ 3 ], [ 2 ], [ 5 ], [ 2, 3 ], [ 7 ], [ 2 ], [ 3 ], [ 2, 5 ] ]
  const primes = numbers.map(rangeToPrimes); // 📍2
  const factorization = numbers.map(primesTofactorization, primes); // 📍3
  const reulstObj = makeObj(factorization); // 📍4
  const multiple = [];
  // 📍5. {'2': 3..}를 [8,..]로 만드는 과정
  for(let key in reulstObj) { 
    multiple[multiple.length] = Number(key) ** reulstObj[key];
  }
  // 📍6. 그리고 그것들의 최종 곱 => 최소공배수
  return multiple.reduce((arr, cur) => arr * cur, 1);
}

smallestMult(10);

// 📍1. 2부터 n까지 배열 만들기
function makeRange(n) {
  const arr = [];
  for(let i = 2; i <= n; i++) {
    arr[arr.length] = i;
  }
  return arr;
}

// 📍2. 소인수분해하기
// 6 => [1, 2, 3, 6] => [2, 3]
function rangeToPrimes(item) {
  // 2-1. 약수 구하기
  // 2-2. 약수 중 소수 뽑기
  return divisor(item).filter(onlyPrimes);
}

// 📍2-1. 약수 배열 구하기
// 6을 [1, 2, 3, 6]으로
function divisor(item) {
  const arr = [];
  for(let i = 1; i <= item; i++) {
    if(arr.includes(i)) return arr;
    if(i ** 2 === item && item % i === 0) {
      arr.splice(arr.length / 2, 0, i);
    } else if(item % i === 0) {
      arr.splice(arr.length / 2, 0, i, item / i);
    }
  }
}

// 📍2-2. 배열에서 소수만 남기기
// [1, 2, 3, 6] => [2, 3]
function onlyPrimes(item) {
  if(item === 1) return false;
  if(divisor(item).length === 2) {
    return true;
  } else {
    return false;
  }
}

let rests = [];
// 📍3. 요소(8)와 소수([2])를 보고 요소를 소수들의 배열([2, 2, 2])로 나타내기
// 기존의 numbers 배열을 바꿀 것이고
// this로 primes 배열이 주어진다. 
function primesTofactorization(item, index) {
  for(let i = 0; i < this[index].length; i++) {
    if(item % this[index][i] === 0 && item === this[index][i]) {
      // 2, 3, 7처럼 하나의 소수 배열만 가지고 자연수와 소수가 같은 경우
      return item;
    } else if(item % this[index][i] === 0 && divisor(item / this[index][i]).length === 2) {
      // 10과 4처럼 몫이 소수인 경우 => [소수, 몫]이 기존의 소수 배열과 같다.
      // 예를 들면 [2, 5]와 [2, 5]가 같은 것처럼
      return [this[index][i], item / this[index][i]];
    } else if(item % this[index][i] === 0 && divisor(item / this[index][i]).length !== 2) {
      // 처음 나눈 몫이 소수가 아닌 경우 소수가 될때까지 나누고 배열로 나타낸다.
      // 8이나 12같은 경우
      // 8은 [2], 12는 [2, 3]을 소수 배열로 갖고 있음. 이걸 [2, 2, 2]와 [2, 2, 3]으로 나타내야
      // 18의 경우 [2, 3]이고 [2, 3, 3]으로 나타내야 한다.
      rests = [];
      let dividing = this[index][i];
      let share = item / this[index][i];
      for(let j = 0; j < this[index].length; j++) {
        // 몫을 현재의 소수로 나눴을때도 나머지가 0인 경우 계속 그 소수로 나누고
        // 그렇지 않으면 다음 요소의 소수를 나누는 값으로 결정
        // 18의 경우 [2, 3]이고 18 / 2의 몫인 9를 또 2로 나눌 수 없다. 
        // 그래서 다음 소수인 3이 적당하다. 
        if(share % this[index][j] === 0) {
          dividing = this[index][j];
          break;
        }
      }
      return [this[index][i], ...notPrime(share, dividing)];
    }
  }
}

// 📍3-1. 8을 [2, 2, 2], 16을 [2, 2, 2, 2]로 만들기 위해
// 18을 [2, 3, 3]으로 만들기 위해
function notPrime(share, prime) {
  if(share % prime === 0 && divisor(share / prime).length === 2) {
    return [...rests, prime, share / prime];
  } else if(share % prime === 0 && divisor(share / prime).length !== 2) {
    rests.push(prime);
    return notPrime(share / prime, prime);
  }
}

// 📍4. 소인수 분해 객체 만들기
/*
- 배열을 돌면서 배열이 아닐 때는 그 값이 하나이므로 숫자를 프로퍼티 이름으로, 1을 그 값으로 정한다. 
- 배열인 경우에는 요소가 a.다 같고 기존의 값보다 많은 경우와 b. 요소가 다 같지 않은 경우로 나뉜다. 

a.의 경우 기존 요소의 프로퍼티 값을 갱신한다. 
b.의 경우 같은 요소끼리 다시 객체로 나눈다. 
처음 프로퍼티 값인 경우엔 1을 할당하고, 
그렇지 않으면 1을 더한다. 
그리고 그렇게 만든 객체를 기존의 객체와 비교하며 숫자 값이 크다면 갱신한다. 

10의 경우
{
'2': 3,
'3': 2,
'5': 1,
'7': 1, 
}
*/
function makeObj(arr) {
  const obj = {};
  arr.forEach(item => {
    if(typeof item === 'number') {
      obj[item] = 1;
    } else {
      // 요소가 다 같고 기존의 값보다 많은 경우
      if(allSame(item) && item.length > obj[item[0]]) {
        obj[item[0]] = item.length;
      } else {
        // 요소가 다 같지 않은 경우 같은 요소끼리 나눈다. 
        const diverse = {};
        item.forEach(itemitem => {
          if(!diverse[itemitem]) {
            diverse[itemitem] = 1;
          } else {
            diverse[itemitem] += 1;
          }
        });
        for(let key in diverse) {
          if(diverse[key] > obj[key]) {
            obj[key] = diverse[key];
          }
        }
      }
    }
  })
  return obj;
}
// 📍4-1. 배열의 값이 다 같은 경우인지 확인
function allSame(arr) {
  return arr.every((item, index, arr) => {
    if(index === (arr.length - 1)) {
      // 마지막 요소
      return item === arr[index - 1];
    } else {
      return item === arr[index + 1];
    }
  });
}
```

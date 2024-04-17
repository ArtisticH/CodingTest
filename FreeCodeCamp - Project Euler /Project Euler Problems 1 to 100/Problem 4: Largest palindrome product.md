## Problem 4: Largest palindrome(대칭수) product
A palindromic number reads the same both ways. The largest palindrome made from the product of two 2-digit numbers is 9009 = 91 × 99.

Find the largest palindrome made from the product of two `n`-digit numbers.

### 조건
```
largestPalindromeProduct(2) should return a number.
largestPalindromeProduct(2) should return 9009.
largestPalindromeProduct(3) should return 906609.
```

### 답변
```javascript
/*
625
625 + 526 = 1151
1151 + 1511 = 2662..처럼 하나의 숫자를 이런 식으로 더해 대칭수를 찾는건
너무 시간이 오래 걸릴 것.. 

그래서 90일때 90-99
91일때 90-99.. 이런식으로 숫자를 만들고
그 숫자가 대칭수인지 판별
그리고 그 대칭수들 중에서 가장 큰 수
*/

function largestPalindromeProduct(n) {
  // n만큼을 배열의 길이로 갖는 희소 배열 만든다. 
  // 예를 들어 n = 2라면 [empty * 2];
  const arr = new Array(n);
  const multiply = [];
  // n = 2라면 num = 90, maxNum = 99, 반복문에서 쓴다. 
  const num = makeNum(arr.slice(), '', false);
  const maxNum = makeNum(arr.slice(), '', true);

  /*
  예를 들어 두 자리수이면 90일때 90-99, 91일때 90-99를 돌며 둘의 곱을 
  배열로 만든다. 
  근데 90 * 91과 91 * 90같은 중복을 제거하기 위해 둘의 곱이
  이미 배열에 있다면 추가하지 않고 다음 증감식으로 이동
  */
  for(let i = num; i <= maxNum; i++) {
    for(let j = num; j <= maxNum; j++) {
      if(multiply.includes(i * j)) continue;
      multiply[multiply.length] = i * j;
    }
  }
  // 대칭수만 골라서 담는다.
  const onlyPalindrome = multiply.filter(isPalindrome);
  // 최대값을 고른다. 
  return Math.max(...onlyPalindrome);
}

console.log(largestPalindromeProduct(2));
console.log(largestPalindromeProduct(3));

function makeNum(arr, num, isMax) {
  if(isMax) {
    // [9, 9], [9, 9, 9]처럼 채우기
    arr.fill(9);
    arr.forEach(item => {
      num += item;
    });
  } else {
    arr[0] = 9;
    // [9, 0], [9, 0, 0]처럼 0으로 채우기
    arr.fill(0, 1);
    // 배열을 문자열로
    arr.forEach(item => {
      num += item;
    });
  }
  num = Number(num);
  return num;
}

/*
원래 숫자와 그 숫자를 뒤집은 숫자를 비교해서 같다면 대칭수
*/
function isPalindrome(num) {
  const reversed = Number(String(num).split('').reverse().join(''));
  return num === reversed;
}
```

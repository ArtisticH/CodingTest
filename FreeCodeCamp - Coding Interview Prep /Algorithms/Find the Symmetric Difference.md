## Find the Symmetric Difference(대칭차)
The mathematical term symmetric difference (△ or ⊕) of two sets is the set of elements which are in either of the two sets but not in both. For example, for sets A = {1, 2, 3} and B = {2, 3, 4}, A △ B = {1, 4}.

Symmetric difference is a binary operation, which means it operates on only two elements. So to evaluate an expression involving symmetric differences among three elements (A △ B △ C), you must complete one operation at a time. Thus, for sets A and B above, and C = {2, 3}, A △ B △ C = (A △ B) △ C = {1, 4} △ {2, 3} = {1, 2, 3, 4}.

Create a function that takes two or more arrays and returns an array of their symmetric difference. The returned array must contain only unique values (no duplicates).

### 조건
```
sym([1, 2, 3], [5, 2, 1, 4]) should return [3, 4, 5].
sym([1, 2, 3, 3], [5, 2, 1, 4]) should return [3, 4, 5].
sym([1, 2, 3], [5, 2, 1, 4, 5]) should return [3, 4, 5].
sym([1, 2, 5], [2, 3, 5], [3, 4, 5]) should return [1, 4, 5].
sym([1, 1, 2, 5], [2, 2, 3, 5], [3, 4, 5, 5]) should return [1, 4, 5].
sym([3, 3, 3, 2, 5], [2, 1, 5, 7], [3, 4, 6, 6], [1, 2, 3]) should return [2, 3, 4, 6, 7].
sym([3, 3, 3, 2, 5], [2, 1, 5, 7], [3, 4, 6, 6], [1, 2, 3], [5, 3, 9, 8], [1]) should return [1, 2, 4, 5, 6, 7, 8, 9].
```

### 답변
```javascript
function sym(...args) {
  const unique = [];
  const length = args.length;
  // 1. 인수 배열을 돌면서 중복 제거
  for(let arg of args) {
    unique.push(onlyUnique(arg));
  }
  // 2. 처음 두 가지 배열에 대해서 교집합 검사하고 각각에서 교집합 제거
  // 제거한 두 배열을 합치고
  // 합친 하나의 배열을 처음의 자리에 삽입
  // 그리고 첫 인수 배열의 갯수 - 1 만큼 진행
  // 끝났다면 오름차순으로 정리후 반환
  for(let i = 1; i < length; i++) {
    const symArr = notIntersection(unique[0], unique[1]);
    unique.splice(0, 2, symArr);
  }
  return unique.flat().sort();
}

// 중복 제거
function onlyUnique(arr) {
  return arr.filter((item, index, arr) => arr.indexOf(item) === index);
}

// 교집합 제거
function notIntersection(arr, comparison) {
  return [...arr.filter(item => !comparison.includes(item)), ...comparison.filter(item => !arr.includes(item))];
}
```

### solution1
```javascript
function sym() {
  const args = [];
  for (let i = 0; i < arguments.length; i++) {
    args.push(arguments[i]);
  }
  // reduce에 넣을 콜백 따로 정의
  // arrayOne과 arrayTwo는 acc, cur의 역할
  // 초기값을 제공하지 않았으므로 처음에 arrayOne은 args[0]이 된다.
  function symDiff(arrayOne, arrayTwo) {
    const result = [];

    arrayOne.forEach(function (item) {
      // arrayTwo에도 속하지 않고 result에도 속하지 않는다면(중복 제거) 추가
      if (arrayTwo.indexOf(item) < 0 && result.indexOf(item) < 0) {
        result.push(item);
      }
    });

    arrayTwo.forEach(function (item) {
      if (arrayOne.indexOf(item) < 0 && result.indexOf(item) < 0) {
        result.push(item);
      }
    });

    return result;
  }

  // Apply reduce method to args array, using the symDiff function
  return args.reduce(symDiff);
}
```

### solution2
```javascript
function sym(...args) {
  // Return the symmetric difference of 2 arrays
  // reduce에서 교집합 제거
  const getDiff = (arr1, arr2) => {
    // Returns items in arr1 that don't exist in arr2
    function filterFunction(arr1, arr2) {
      return arr1.filter(item => arr2.indexOf(item) === -1);
    }

    // Run filter function on each array against the other
    return filterFunction(arr1, arr2).concat(filterFunction(arr2, arr1));
  };

  // Reduce all arguments getting the difference of them
  const summary = args.reduce(getDiff, []);

  // Run filter function to get the unique values
  // 중복 제거 따로
  const unique = summary.filter((elem, index, arr) => index === arr.indexOf(elem));
  return unique;
}
```

### solution3
```javascript
const diff = (arr1, arr2) => [
  ...arr1.filter(e => !arr2.includes(e)),
  ...arr2.filter(e => !arr1.includes(e))
];
// 중복 제거를 Set으로
const sym = (...args) => [...new Set(args.reduce(diff))];

// test here
sym([1, 2, 3], [5, 2, 1, 4]);
```

## No Repeats Please
Return the number of total permutations(순열) of the provided string that don't have repeated consecutive letters.(연속된 글자가 아닌) Assume that all characters in the provided string are each unique.

For example, `aab` should return 2 because it has 6 total permutations (`aab`, `aab`, `aba`, `aba`, `baa`, `baa`), but only 2 of them (`aba` and `aba`) don't have the same letter (in this case `a`) repeating.

### 조건
```
permAlone("aab") should return a number.
permAlone("aab") should return 2.
permAlone("aaa") should return 0.
permAlone("aabb") should return 8.
permAlone("abcdefa") should return 3600.
permAlone("abfdefa") should return 2640.
permAlone("zzzzzzzz") should return 0.
permAlone("a") should return 1.
permAlone("aaab") should return 0.
permAlone("aaabb") should return 12.
```

### 답변
```javascript
function permAlone(str) {
  const num = [];
  const length = str.length;
  for(let i = 0; i < length; i++) {
    num.push(i);
  }

  if(length < 3) {
    // underThree();
  } else if(length === 3) {
    three();
  } else {
    overThree();
  }
}

// permAlone('aab');
// three는 총 6개의 배열을 만든다. 
function three(numArr) {
  const result = [];

  numArr.forEach((item, _, arr) => {
    const rest = arr.filter(i => item !== i);
    result[result.length] = [item, ...rest];
    result[result.length] = [item, ...rest.reverse()];
  });
  return result;
}

function overThree(numArr) {
  const result = [];

  numArr.forEach((item, _, arr) => {
    const rest = arr.filter(i => item !== i);
    if(rest.length === 3) {
      for(let threeArr of three(rest)) {
        result[result.length] = [item, ...threeArr];
      }
    } else if(rest.length > 3) {
      for(let overThreeArr of overThree(rest)) {
        result[result.length] = [item, ...overThreeArr];
      }
    }
  });
  return result;
}

console.log(overThree([1, 2, 3, 4, 5]));

```

### solution1
```javascript
```

### solution2
```javascript
```

### solution3
```javascript
```

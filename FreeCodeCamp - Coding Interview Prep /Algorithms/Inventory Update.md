## Inventory Update

Compare and update the inventory stored in a 2D array against a second 2D array of a fresh delivery. Update the current existing inventory item quantities (in `arr1`). If an item cannot be found, add the new item and quantity into the inventory array. The returned inventory array should be in alphabetical order by item.

### 조건
```
The function updateInventory should return an array.
updateInventory([[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]], [[2, "Hair Pin"], [3, "Half-Eaten Apple"], [67, "Bowling Ball"], [7, "Toothpaste"]]) should return [[88, "Bowling Ball"], [2, "Dirty Sock"], [3, "Hair Pin"], [3, "Half-Eaten Apple"], [5, "Microphone"], [7, "Toothpaste"]].
updateInventory([[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]], []) should return [[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]].
updateInventory([], [[2, "Hair Pin"], [3, "Half-Eaten Apple"], [67, "Bowling Ball"], [7, "Toothpaste"]]) should return [[67, "Bowling Ball"], [2, "Hair Pin"], [3, "Half-Eaten Apple"], [7, "Toothpaste"]].
updateInventory([[0, "Bowling Ball"], [0, "Dirty Sock"], [0, "Hair Pin"], [0, "Microphone"]], [[1, "Hair Pin"], [1, "Half-Eaten Apple"], [1, "Bowling Ball"], [1, "Toothpaste"]]) should return [[1, "Bowling Ball"], [0, "Dirty Sock"], [1, "Hair Pin"], [1, "Half-Eaten Apple"], [0, "Microphone"], [1, "Toothpaste"]].
```

### 답변
```javascript
function updateInventory(arr1, arr2) {
    // 일단 arr1에서 텍스트만 빼와 배열을 만들고
    const arr1Name = arr1.map(item => item[1]);
    /*
    arr2를 돌면서 arr2의 텍스트가 arr1에 포함되어 있다면
    arr1Name 인덱스로 arr1에서 찾아 숫자를 갱신하고
    없다면 arr1에 arr2의 요소를 추가
    */
    arr2.forEach(arr2item => {
        if(arr1Name.indexOf(arr2item[1]) !== -1) {
            arr1[arr1Name.indexOf(arr2item[1])][0] = arr1[arr1Name.indexOf(arr2item[1])][0] + arr2item[0];
        } else {
            arr1.push(arr2item);
        }
    });
    // 텍스트 오름차순
    return arr1.sort((a, b) => a[1].localeCompare(b[1]));
}
```

### solution1
```javascript
function updateInventory(currentInventory, newInventory) {
  // Check each item of new inventory
  /*
  new를 하나씩 돌면서 new를 각각의 old와 비교해본다.
  new의 물품이 old에 있다면(newItem[1] === oldItem[1]) 재고 숫자 갱신하고 다음 new 반복문으로
  만약 없다면(if (!found)) old에 새로 추가하고 다음 new 반복문으로
  */
  for (let newItem of newInventory) {
    let found = false;
    // Check against current inventory
    for (let oldItem of currentInventory) {
      // Update value if new item is found
      if (newItem[1] === oldItem[1]) {
        oldItem[0] += newItem[0];
        found = true;
        break;
      }
    }
    // Otherwise add item to the old inventory
    if (!found) currentInventory.push([...newItem]);
  }
  // ----------------------------------------------
  // Alphabetize and put into original array format
  // 비교함수, 1은 두번째 인수를 먼저(바꿔) -1은 첫 번째 인수를 먼저(현상유지) => 오름차순
  // (a, b) => a[1] > b[1] ? 1 : ( a[1] < b[1] ? -1 : 0);
  return currentInventory
    .sort((a, b) => {
        if (a[1] < b[1]) return -1;
        if (a[1] > b[1]) return 1;
        return 0;
    });
}
```

### solution2
```javascript
function updateInventory(currentInventory, newInventory) {
  const combinedInventory = {};
  // Account for all old inventory
  // old를 돌면서 { "Bowling Ball" : 21, ...} 형식의 객체 만들어
  for (let item of currentInventory) {
    combinedInventory[item[1]] = item[0];
  }
  // Account for all new inventory
  // new를 돌면서 
  for (let item of newInventory) {
    /*
    combinedInventory[item[1]]의 값이 null이거나 undefined라면(nullish) 일단 0을 할당 하고 그 다음에 new의 재고 숫자 입력
    */
    combinedInventory[item[1]] ??= 0; // combinedInventory[item[1]] = combinedInventory[item[1]] || 0;
    combinedInventory[item[1]] += item[0];
  }
  // Alphabetize and put into original array format
  // 그리고 Object.keys로 text만 추출한 배열을 손쉽게 오름차순으로 정렬하고 map을 이용해 ["text", 재고 숫자]로 변형
  return Object.keys(combinedInventory)
    .sort()
    .map(item => [combinedInventory[item], item]);
}
```

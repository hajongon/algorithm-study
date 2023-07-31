# 10815\_숫자 카드

## 문제 설명

[문제 링크](https://www.acmicpc.net/problem/10815)

숫자 카드로 이루어진 2개의 배열을 주어질 때 2번 째 배열과 1번 째 배열 둘 다 가지고 있을 때 `1` 없을 때`0`으로 출력하시오,

- Input

```
5
6 3 2 10 -10
8
10 9 -5 2 3 4 5 -10
```

- Output

```
1 0 0 1 1 0 0 1
```

## 풀이와 설명

1. 이분 탐색을 이용하여 각각의 요소들이 있는지 확인한다.
2. 먼저 첫 배열을 오름차순으로 정렬을 한 후, 두 번째 배열을 반복하면서 첫 배열을 이분 탐색을 통해 찾는 요소의 유무를 확인한다.

```js
const fs = require("fs");
const input = fs.readFileSync("../input.txt").toString().trim().split("\n");

let findArr = input[1]
  .split(" ")
  .map(Number)
  .sort((a, b) => a - b);
let foundArr = input[3].split(" ").map(Number);

let ans = [];

function findCard(card, start, end) {
  if (start > end) return ans.push(0);

  let mid = parseInt((start + end) / 2);

  if (findArr[mid] === card) return ans.push(1);
  else if (findArr[mid] > card) findCard(card, start, mid - 1);
  else findCard(card, mid + 1, end);
}

for (let i = 0; i < foundArr.length; i++) {
  findCard(foundArr[i], 0, findArr.length - 1);
}

console.log(ans.join(" "));
```

# 18870\_좌표 압축

## 문제 설명

[문제 링크](https://www.acmicpc.net/problem/18870)

주어진 배열의 크기를 비교하여 각각 요소가 몇 번째로 큰지 비교하는 배열을 출력하시오.

- Input

```
- 5
  2 4 -10 4 -9

- 6
  1000 999 1000 999 1000 999
```

- Output

```
- 2 3 0 3 1
- 1 0 1 0 1 0
```

## 풀이와 설명

1. 배열을 따로 하나 더 만들어, 오름차순으로 정렬해주고, 또 중복 제거를 해준다. (이분 탐색 준비)
2. 기존 입력의 배열을 반복하면서 정렬된 배열을 통해 이분 탐색을 진행하고, 찾는 요소의 인덱스 값을 출력할 배열에 넣어준다. (오름차순으로 정렬했기 떄문에 인덱스가 곧, 몇 번째 크기를 말한다.)

```js
const fs = require("fs");
const input = fs.readFileSync("../input.txt").toString().trim().split("\n");

let arr = input[1].split(" ").map(Number);
let set = new Set([...arr]); // 중복 제거
set = [...set].sort((a, b) => a - b);
let ans = Array.from({ length: arr.length }, () => 0);

const findSort = (n, start, end, i) => {
  if (start > end) return;

  let mid = parseInt((start + end) / 2);

  if (set[mid] === n) {
    ans[i] = mid;
    return;
  } else if (set[mid] < n) findSort(n, mid + 1, end, i);
  else findSort(n, start, mid - 1, i);
};

for (let i = 0; i < arr.length; i++) {
  findSort(arr[i], 0, set.length - 1, i);
}
console.log(ans.join(" "));
```

# 1822\_차집합

## 문제 설명

[문제 링크](https://www.acmicpc.net/problem/1822)

두 개의 배열 A,B가 주어질 때 두 배열의 차집합을 구하시오. 단, 차집합이 없을 경우 0을 출력한다.
(차집합: A-B는 A에 포함되어 있고, B에는 없는 요소를 가지고 있는 집합을 말한다.)

- Input

```
- 4 3
  2 5 11 7
  9 7 4

- 3 5
  2 5 4
  1 2 3 4 5
```

- Output

```
- 3
  2 5 11

- 0
```

## 풀이와 설명

1. A-B의 차집합이기 때문에 먼저 B를 오름차순으로 정렬한다. (이분 탐색 준비)
2. A의 요소들을 돌면서 B를 이분 탐색으로 찾는 요소들이 있는지 판별한다.

```js
const fs = require("fs");
const input = fs.readFileSync("../input.txt").toString().trim().split("\n");

let findArr = input[1].split(" ").map(Number);
let foundArr = input[2]
  .split(" ")
  .map(Number)
  .sort((a, b) => a - b);
let ans = [];

function sortFunc(n, start, end) {
  if (start > end) return true;

  let mid = parseInt((start + end) / 2);

  if (foundArr[mid] === n) return false;
  else if (foundArr[mid] < n) return sortFunc(n, mid + 1, end);
  else return sortFunc(n, start, mid - 1);
}

for (let i of findArr) {
  sortFunc(i, 0, foundArr.length - 1) ? ans.push(i) : null;
}
ans.sort((a, b) => a - b);
console.log(ans.length === 0 ? 0 : `${ans.length}\n` + ans.join(" "));
```

# 14921\_용액 합성하기

## 문제 설명

[문제 링크](https://www.acmicpc.net/problem/14921)

오름차순으로 주어진 배열이 있을 때 각 원소의 합이 0에 가장 가까운 원소들을 찾으시오.

- Input

```
- 5
  -101 -3 -1 5 93

- 2
  -100000 -99999
- 7
  -698 -332 -123 54 531 535 699
```

- Output

```
- 2
- -199999
- 1
```

## 풀이와 설명

1. 투 포인터를 통해 start, end는 각각 첫 요소와 마지막 요소를 지정해준다.
2. 0과 가까운 값을 구하는 건 결국 두 합의 절대 값이 가장 작은 값을 구하는 것이기 때문에 각 요소의 합의 절대값이 가장 작은 값을 구한다.
3. 두 합이 음수이면 `start++`, 양수이면 `end--`를 하고, `start >= end` 될 경우 반복을 멈춘다.

```js
const fs = require("fs");
const input = fs.readFileSync("../input.txt").toString().trim().split("\n");
let arr = input[1].split(" ").map(Number);
let ans = arr[0] + arr[arr.length - 1];
let [start, end] = [0, arr.length - 1];

while (start < end) {
  let plus = arr[start] + arr[end];

  if (plus > 0) end--;
  else if (plus < 0) start++;
  else {
    ans = 0;
    break;
  }

  if (Math.abs(plus) < Math.abs(ans)) {
    ans = plus;
  }
}

console.log(ans);
```

# 12015\_가장 긴 증가하는 부분 수열 2

## 문제 설명

[문제 링크](https://www.acmicpc.net/problem/12015)

수열 A가 주어졌을 때, 가장 긴 증가하는 부분 수열을 구하는 프로그램을 작성하시오.
A = {10, 20, 10, 30, 20, 50} => {10,20,30,50}

- Input

```
6
10 20 10 30 20 50
```

- Output

```
4
```

## 풀이와 설명

### dp를 이용하여 푸는 방법 (시간 초과)

dp: count의 값을 가지고 있는 배열

- 배열의 지정된 요소보다 우측에 있는 다른 요소들이 클 경우 count를 해주고, 지정된 요소의 위치와 같은 dp의 인덱스에 값과, 반복을 했을 때 클 경우 count한 값과 비교하여 가장 큰 값을 dp의 인덱스에 넣어준다.
- 가장 큰 값을 리턴해주면 가장 큰 수열의 길이을 출력할 수 있다.

```js
const fs = require("fs");
const input = fs.readFileSync("./dev/stdin").toString().trim().split("\n");

let arr = input[1].split(" ").map(Number);
let dp = Array.from({ length: arr.length }, () => 0);
let numDp = 0;

for (let i = 0; i < arr.length; i++) {
  for (let j = i; j < arr.length; j++) {
    arr[i] < arr[j] ? (numDp++, (dp[j] = Math.max(dp[j], numDp))) : null;
  }
  numDp = 0;
}

console.log(Math.max(...dp));
```

### 이분탐색을 이용한 풀이

- 먼저, dp라는 배열에 첫 요소를 넣어준다. 반복문을 통해 input의 요소들과 dp의 마지막 요소를 비교하여 dp의 마지막 요소보다 input의 요소가 크면 dp 배열에 넣어준다.

- 만일 작거나 같을 경우 이분 탐색을 진행한다. 이분 탐색을 통해서 input의 비교 대상 요소를 dp의 어느 위치와 스위칭 할 지 찾는다.

```js
const fs = require("fs");
const input = fs.readFileSync("../input.txt").toString().trim().split("\n");

let arr = input[1].split(" ").map(Number);
let dp = [];

function search(start, end, n) {
  if (start >= end) return end;

  let mid = parseInt((start + end) / 2);

  if (dp[mid] < n) return search(mid + 1, end, n);
  else return search(start, mid, n);
}

dp.push(arr[0]);

for (let i = 1; i < arr.length; i++) {
  if (dp[dp.length - 1] < arr[i]) {
    dp.push(arr[i]);
  } else {
    let idx = search(0, dp.length - 1, arr[i]);
    dp[idx] = arr[i];
  }
}
console.log(dp.length);

// search 함수의 반복문 형태
// function search(start, end, n) {
//   let mid = 0;

//   while (start < end) {
//     mid = parseInt((start + end) / 2);
//     dp[mid] < n ? (start = mid + 1) : (end = mid);
//   }
//   return end;
// }
```

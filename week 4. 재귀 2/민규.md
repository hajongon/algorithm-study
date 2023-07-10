# 종이의 개수\_1780

## 문제 설명

[문제 링크](https://www.acmicpc.net/problem/1780)

- NxN으로 이루어진 행렬들 중 같은 수로 이루어져 있을 때 하나의 종이로 보고, 수가 다를 때는 같은 크기의 종이로 9개로 자르고, 같은 수가 되었을 때까지 반복하여 총 종이의 갯수를 출력하시오.
- Input
  첫째 줄 N(1 <= N <= 3^7, N은 3^k 꼴), 나머지 N개의 줄에는 N개의 정수로 행렬이 주어진다.

```
9
0 0 0 1 1 1 -1 -1 -1
0 0 0 1 1 1 -1 -1 -1
0 0 0 1 1 1 -1 -1 -1
1 1 1 0 0 0 0 0 0
1 1 1 0 0 0 0 0 0
1 1 1 0 0 0 0 0 0
0 1 -1 0 1 -1 0 1 -1
0 -1 1 0 1 -1 0 1 -1
0 1 -1 1 0 -1 0 1 -1
```

- Output
  첫 줄은 -1, 둘째 줄은 0, 셋째 줄은 1로 채워진 종이의 개수를 출력한다.

```
10
12
11
```

## 풀이와 설명

1. 행렬의 모든 수를 탐색하여 같은 수인지 먼저 판별, 같은 수 일때 행렬의 첫 번째 원소의 값에 해당되는 값을 +1 해준다.
2. 다른 수 일 경우, 행렬을 각각 1/3을 하여 다시 1번과 같은 작업을 진행한다.
3. 반복문을 통해 행을 증가하면서 한 행에 3등분 된 행렬을 탐색한다.
   ex) i \* (n / 3) + firstX , (n / 3) + firstY, n / 3
   n이 9일 경우 x,y축을 3으로 나누어 0,3,6을 기점으로 탐색하게 하고, n은 3으로 나누어 재귀 요청

```js
const fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().trim().split("\n");
const leng = Number(input.shift());
input = input.map((item) => item.split(" ").map(Number));
let ans = Array.from({ length: 3 }, () => 0);

function paper(arr, firstX, firstY, n) {
  let trigger = true;
  if (n < 1) return;

  for (let i = firstX; i < n + firstX; i++) {
    // 1
    for (let j = firstY; j < n + firstY; j++) {
      if (arr[firstX][firstY] !== arr[i][j]) {
        trigger = false;
        break;
      }
    }
    if (!trigger) break;
  }

  if (trigger) {
    switch (arr[firstX][firstY]) {
      case -1:
        ans[0]++;
        break;
      case 0:
        ans[1]++;
        break;
      case 1:
        ans[2]++;
        break;
    }
  } else {
    // 2
    for (let i = 0; i < 3; i++) {
      paper(arr, i * (n / 3) + firstX, 0 * (n / 3) + firstY, n / 3);
      paper(arr, i * (n / 3) + firstX, 1 * (n / 3) + firstY, n / 3);
      paper(arr, i * (n / 3) + firstX, 2 * (n / 3) + firstY, n / 3);
    }
  }
}

paper(input, 0, 0, leng);

console.log(ans.join("\n"));
```

# 색종이 만들기\_2630

## 문제 설명

[문제 링크](https://www.acmicpc.net/problem/2630)

### 문제

1. 행렬에 모든 수가 0 or 1로 같을 때 1개의 색종이로 본다.
2. 다를 경우 행렬을 4등분 하여 다시 1번과 같은 작업을 반복한다.
3. 0은 하얀색 종이, 1은 파란색 종이이다.

- Input:
  첫 줄: NxN (N=2^k, k는, 1<= k <=7)
  나머지: 색종이의 행렬

```
8
1 1 0 0 0 0 1 1
1 1 0 0 0 0 1 1
0 0 0 0 1 1 0 0
0 0 0 0 1 1 0 0
1 0 0 0 1 1 1 1
0 1 0 0 1 1 1 1
0 0 1 1 1 1 1 1
0 0 1 1 1 1 1 1
```

- Output:
  첫 줄은 하얀색 종이(0), 두번째는 파란색 종이(1) 순서로 출력

```
9
7
```

## 문제 풀이

1. 행렬을 돌면서 다 같은 수 인지 판별하고 같을 경우 행렬의 가장 첫 번째 요소의 수(0,1)의 갯수를 +1한다.
2. 다를 경우 행렬을 4로 나누고 각 사분면 마다 다시 재귀를 요청 해준다.

```js
const fs = require("fs");
let input = fs
  .readFileSync("/dev/stdin")
  .toString()
  .trim()
  .split("\n")
  .map((item) => item.split(" "));

const leng = Number(input.shift());
let ans = [0, 0];

function Quad(arr, n) {
  // 행렬을 4등분 하는 함수
  if (n === leng) {
    // 첫 행렬이 들어 올 경우 4등분을 할 필요가 없음
    let arr1 = [];
    arr1 = arr;
    return { arr1 };
  } else {
    let [arr1, arr2, arr3, arr4] = [[], [], [], []]; // 각 행렬을 슬라이스와 스프레드 문법을 통해 4등분 해준다.
    for (let i = 0; i < n; i++) {
      arr1 = [...arr1, arr[i].slice(0, n)];
      arr2 = [...arr2, arr[i].slice(n, n * 2)];
      arr3 = [...arr3, arr[i + n].slice(0, n)];
      arr4 = [...arr4, arr[i + n].slice(n, n * 2)];
    }
    return { arr1, arr2, arr3, arr4 };
  }
}

function quadTree(arr, n) {
  if (n < 1) return; // 재귀 멈춤 조건문

  let trigger = true; // 모든 요소가 같은지 판별하는 변수
  let quadObj = Quad(arr, n); // 행렬을 4등분하여 각 사분면마다 가지고 있는 객체
  let quad = 1; // 각 사분면의 증가 변수

  while (quad < 5 && quadObj[`arr${quad}`]) {
    // 1~4분면 반복하고 만일 객체에 사분면이 존재하지 않는 경우 멈춤( 첫 행렬 때는 1분면 밖에 없기 때문에 처음 행렬 제어)
    trigger = true;
    for (let i = 0; i < n; i++) {
      // 각 원소가 같은지 판별하는 반복문
      for (let j = 0; j < n; j++) {
        if (quadObj[`arr${quad}`][0][0] !== quadObj[`arr${quad}`][i][j]) {
          // trigger을 false로 바꾸고 만일 다를 경우 break
          trigger = false;
          break;
        }
      }
      if (!trigger) break; // 만일 다를 경우 break
    }
    if (trigger) ans[Number(quadObj[`arr${quad}`][0][0])]++; // 1
    else quadTree(quadObj[`arr${quad}`], n / 2); // 2
    quad++;
  }
  return;
}

quadTree(input, leng);

console.log(ans.join("\n"));
```

# 완전탐색\_최소직사각형

## 문제 설명

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/86491)

명함들의 가로,세로 사이즈의 배열들(sizes)을 받아 명함을 가로, 세로 방향으로 넣었을 때 전부 들어갈 수 있는 명함 지갑의 최소 사이즈를 구하시오.

- Input

| 명함 번호 | 가로 길이 | 세로 길이 |
| :-------: | :-------: | :-------: |
|     1     |    60     |    50     |
|     2     |    30     |    70     |
|     3     |    60     |    30     |
|     4     |    80     |    40     |

- Output

|                     sizes                     | result |
| :-------------------------------------------: | :----: |
|   [[60, 50], [30, 70], [60, 30], [80, 40]]    |  4000  |
| [[10, 7], [12, 3], [8, 15], [14, 7], [5, 15]] |  120   |
| [[14, 4], [19, 6], [6, 16], [18, 7], [7, 11]] |  133   |

## 풀이와 설명

1. 가로, 세로 모든 길이의 가장 큰 길이를 구한다.
   (가로 방향, 세로 방향 모든 방면에서 모든 명함이 들어갈 수 있는 크기)
2. 각 원소들의 가로,세로중 더 작은 것들을 비교한 후, 한 배열에 정리하여 그 배열에 가장 큰 값을 넣는다.
   (지갑의 최소 사이즈를 구하기 위해서, 각 원소에서 가로,세로 중 큰 값은 1번에서 구한 값으로 대처하고, 가로,세로 중 나머지 값을 커버할 수 있는 최대 값을 구해준다.)

```js
function solution(sizes) {
  let [x, y] = [sizes[0][0], sizes[0][1]];
  x = Math.max(...sizes.flat());
  y = Math.max(...sizes.map((item) => (item[0] > item[1] ? item[1] : item[0])));

  return x * y;
}
```

# 완전탐색\_모의고사

## 문제 설명

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42840)

### 문제

정답을 나열한 배열을 주었을 때 1,2,3번의 학생들이 규칙대로 찍었을 때 가장 많이 맞는 학생을 구하시오. 만일 중복될 경우 중복된 모든 학생을 출력하시오.

- 1번 학생: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
- 2번 학생: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
- 3번 학생: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

- Input / Output:

|   answers   | return  |
| :---------: | :-----: |
| [1,2,3,4,5] |   [1]   |
| [1,3,2,4,2] | [1,2,3] |

## 문제 풀이

1. 각 학생의 패턴을 미리 입력한 후, answer가 끝날 때까지 각 학생의 규칙 패턴을 반복해서 비교하고, 가장 맞이 맞춘 학생의 index를 출력해준다.
2. 만일 중복될 경우가 있으니, 가장 큰 수와 모든 정답 개수 배열을 비교해서 정답 배열에 넣어준다.

```js
function solution(answers) {
  var answer = [];
  let stack = [0, 0, 0];
  let a1 = [1, 2, 3, 4, 5];
  let a2 = [2, 1, 2, 3, 2, 4, 2, 5];
  let a3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];

  for (let i = 0; i < answers.length; i++) {
    if (answers[i] === a1[i % a1.length]) stack[0]++;
    if (answers[i] === a2[i % a2.length]) stack[1]++;
    if (answers[i] === a3[i % a3.length]) stack[2]++;
  }
  for (let i in stack) {
    if (Math.max(...stack) === stack[i]) answer.push(Number(i) + 1);
  }

  return answer;
}
```

# 완전 탐색\_소수 찾기

## 문제 설명

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42839)

### 문제

0 ~ 9까지 숫자로 이루어진 카드들에서 흩어진 종이 조각을 주어졌을 때 흩어진 종이 조각들로 소수를 몇 개까지 만들 수 있는지 출력하시오.

- Input / Output:

| numbers | return |
| :-----: | :----: |
|  "17"   |   3    |
|  "011   |   2    |

## 문제 풀이

- dfs를 활용하여 문제 풀이
- 소수를 가질 배열, 방문 배열을 만들고, 주어진 문자열에 모든 요소들을 dfs를 통해 방문하고, 요소들을 더해가면서, 소수인지 아닌지 판별하고, 방문 배열에 반문 표시를 반복해준다.
- 위의 방법을 모든 요소들의 `shift()`,`push()`를 통해 주어진 numbers의 자리를 바꾸면서 반복한다.
- 소수를 가질 배열은 반복 요소가 없어야 됨으로, `Set()`메소드를 사용한다.

```js
function prime(num) {
  // 소수 판별 함수
  if (num === 1 || num === 0) return false;
  let sqrt = Math.sqrt(num);

  for (let i = 2; i < parseInt(sqrt) + 1; i++) {
    if (num % i === 0) return false;
  }
  return true;
}

function solution(numbers) {
  let ans = new Set(); // 소수를 담을 Set메소드
  let arr = numbers.split("");
  let visit = Array.from({ length: arr.length }, () => false); // 방문 배열

  function findSort(start, arr, card) {
    // 만일 비교를 할 대상의 시작점이 arr의 길이와 같으면 재귀를 멈춘다.
    if (start === arr.length) return;
    let cardParts = ""; // 재귀를 통해 받는 card에 덫붙일 변수

    for (let i = 0; i < arr.length; i++) {
      if (!visit[i]) {
        // arr[i]의 요소가 방문되지 않았을 경우
        cardParts += arr[i];
        // (매개변수 card + arr[i]의 요소)가 소수일 경우 set에 add
        prime(Number(card + cardParts))
          ? ans.add(Number(card + cardParts))
          : null;
        visit[i] = true; // 방문 표시

        findSort(start + 1, arr, card + cardParts); // 시작점을 1씩 더해주고, card+cardParts를 통해 재귀

        visit[i] = false; //재귀가 끝나고 방문 표시 초기화
        cardParts = ""; // 재귀가 끝나고 card의 첨가할 값들 초기화
      }
    }
    return;
  }

  for (let i = 0; i < arr.length; i++) {
    // 요소들의 소수를 차례대로 바꿔준다.
    arr.push(arr.shift());
    findSort(0, arr, "");
  }

  return ans.size;
}
```

# 완전탐색\_카펫

## 문제 설명

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42842)

### 문제

brown색과 yellow의 색이 있을 때 yellow가 카펫의 가운데 패턴에 차지했을 때의 직사각형의 가로,세로를 return 하시오.

- 카펫의 가로 길이는 세로의 길이보다 같더나 길다.
- **카펫의 가운데 줄은 yellow의 양 옆 1줄로 brown이 차지한다.**

- Input / Output:

  | brown | yellow | return |
  | :---: | :----: | :----: |
  |  10   |   2    | [4,3]  |
  |   8   |   1    | [3,3]  |
  |  24   |   24   | [8,6]  |

## 문제 풀이

- yellow의 모든 약수들을 구해서 먼저 중앙에 오는 노란색 패턴의 사이즈를 측정한다.
- 카펫의 가운데 줄은 항상 brown이 1개씩 있기 때문에 brown - (yellow의 세로길이) \* 2 / 2 === yellow의 가로길이 +2 일 때 패턴이 성사 된다.
- [yellow의 가로길이+2, brown+yellow/x]값이 정답이 되게 된다.
- 주의 점: 가로 >= 세로

```js
function measure (n){
  // yellow의 모든 약수를 구해 직사각형의 가운데 모형들을 구한다.
    let measureArrX = [];
    let measureArrY = [];
    if(n === 0) return {measureArrX, measureArrY};
    for (let i = 1 ; i <= parseInt(Math.sqrt(n))+1  ; i++){
        n % i === 0 ? (measureArrY.push(i),measureArrX.push(n/i)): null
    }
    return {measureArrX, measureArrY}
}

function solution(brown, yellow) {
    let answer = [];
    let center = measure(yellow);
    let [x,y] = [0,0];

    for(let i = 0 ; i < center.measureArrY.length; i++ ){
        if((brown - (2*center.measureArrY[i])) / 2 === center.measureArrX[i]+2)
            x = center.measureArrX[i]+2;
            y = parseInt((brown+yellow) /x);
            x >= y ? answer = [x,y] : null
    }
    return answer;
}
);
```

# 완전 탐색\_피로도

## 문제 설명

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/87946)

### 문제

총 피로도 `k` , 던전의 [최소 필요 피로도, 실제 감소 피로도] `dungeons`, 가 주어졌을때 제일 많이 돌 수 있는 던전의 갯수가 몇개인지 return하시오.

- Input / Output:

  |  k  |         dungeons          | result |
  | :-: | :-----------------------: | :----: |
  | 80  | [[80,20],[50,40],[30,10]] |   3    |

## 문제 풀이

- dfs을 이용하여 모든 요소들을 돌면서 피로도를 비교한다.

```js
function solution(k, dungeons) {
  let set = []; // 돌 수 있는 던전을 담을 배열
  let ans = 0; // 던전를 돌 수 있는 최대 갯수를 담을 변수
  let visit = Array.from({ length: dungeons.length }, () => true); // 방문 배열

  function dfs(arr, k) {
    // dfs
    // k가 0보다 작으면 피로도가 초과 될 경우이니 배열의 총 길이에서 -1한 값을 비교한다.
    if (k < 0) {
      ans < set.length - 1 ? (ans = set.length - 1) : null;
      return;
    }
    // 모든 요소를 다 돌았을 경우 set의 사이즈를 그대로 반환한다.
    else if (
      k === 0 ||
      set.length === arr.length ||
      visit[visit.length - 1] === false
    ) {
      ans < set.length ? (ans = set.length) : null;
      return;
    }

    // 방문을 하지 않았거나, k가 arr[i]의 최소 피로도 보다 크거나 같으면 진행
    for (let i = 0; i < arr.length; i++) {
      if (visit[i] && k >= arr[i][0]) {
        set.push(arr[i][0]);
        visit[i] = false;
        dfs(arr, k - arr[i][1]);
        set.pop();
        visit[i] = true;
      }
    }
  }

  // 모든 요소의 순서를 바꿔가면서 dfs를 진행한다.
  for (let i = 0; i < dungeons.length; i++) {
    dungeons.push(dungeons.shift());
    dfs(dungeons, k);
  }

  return ans;
}
```

# 완전 탐색\_전력망을 둘로 나누기

## 문제 설명

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/86971)

### 문제

[트리](<https://namu.wiki/w/%ED%8A%B8%EB%A6%AC(%EA%B7%B8%EB%9E%98%ED%94%84)>) 형태로 주어진 배열에 하나의 간선을 제거하여 나온 두 개의 트리에서 각 트리의 차이가 가장 적었을 때의 값을 구하시오.

- 주의점 : 주어진 트리의 배열은 노드들이 연결된 순서로 출력이 되지는 않는다.

- Input / Output:

|  n  |                       wires                       |
| :-: | :-----------------------------------------------: |
|  9  | [[1,3],[2,3],[3,4],[4,5],[4,6],[4,7],[7,8],[7,9]] |
|  4  |                [[1,2],[2,3],[3,4]]                |
|  7  |       [[1,2],[2,7],[3,7],[3,4],[4,5],[6,7]]       |

## 문제 풀이

- 주어진 wires배열에 요소에 하나씩 요소가 제외된 배열을 dfs를 사용해서 연결되는 노드들의 갯수를 구하고 `연결된 노드의 개수 - (n - 연결된 노드의 개수)`의 절대값을 구하여 차이가 가장 작은 수를 출력해준다.
- queue를 통해 queue에 맨 처음 노드들의 연결된 간선을 확인해주고, queue의 첫 요소를 out시켜준다.
- out시킨 요소는 연결이 된 노드들이기 때문에 연결된 노드의 배열에 넣어준다.

```js
function solution(n, wires) {
  var answer = n;
  let wire = []; // wires에서 하나의 요소를 뺀 배열을 담을 변수
  // n의 갯수만큼 set메소드를 사용해서 중복되지 않는 노드의 수를 구한다.
  let ans = Array.from({ length: n }, () => new Set());
  // dfs를 통해 방문 표시를 할 배열
  let visit = Array.from({ length: n - 1 }, () => false);
  // queue를 통해 노드들이 연결의 유무를 확인한다.
  let queue = [];
  // 연결된 노드의 개수 - (n - 연결된 노드의 개수)의 절댓값
  let diff = 0;

  for (let i = 0; i < wires.length; i++) {
    // wires에서 하나의 요소를 빼준다.
    wire = [...wires.slice(0, i), ...wires.slice(i + 1, n)];
    // queue에 첫 요소를 넣어주고, 방문 처리한다.
    queue.push(wire[0]);
    visit[0] = true;
    let j = 1;

    // queue가 없는 경우는 모든 연결된 요소를 다 확인하는 경우
    while (queue.length) {
      // queue에 맨 처음 요소가 연결되어 있는 노드들을 확인하여 queue에 담는다.
      //단, 방문하지 않은 노드들만 넣어주어야한다.
      if (
        !visit[j] &&
        (queue[0].includes(wire[j][0]) || queue[0].includes(wire[j][1]))
      ) {
        queue.push(wire[j]);
        visit[j] = true;
      }

      j++;
      // 모든 wire의 배열을 다 돌았을때, queue의 처음 값을 빼서, ans에 모든 노드들을 담아준다.
      if (j === wire.length) {
        j = 1;
        queue.shift().map((item) => ans[i].add(item));
      }
    }
    // 방문 표시 초기화
    visit = visit.map((item) => (item = false));
  }

  // 가장 작은 차이값을 구하는 반복문
  for (let i = 0; i < ans.length; i++) {
    diff = Math.abs(ans[i].size - (n - ans[i].size));
    answer > diff ? (answer = diff) : null;
  }

  return answer;
}
```

# 완전 탐색\_모음사전

## 문제 설명

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/84512)

### 문제

사전 순으로 ['A', 'E', 'I', 'O', 'U'] 만 사용해서 5이하의 길이를 가진 문자열을 만든 `word`가 주어질 때 `word`의 사전 순으로 몇 번째 인지 출력하시오.

- 패턴: A", "AA", "AAA", "AAAA", "AAAAA", "AAAAE", "AAAAI", ...

- Input / Output:

|  word   | result |
| :-----: | :----: |
| "AAAAE" |   6    |
| "AAAE"  |   10   |
|   "I"   |  1563  |
|  "EIO"  |  1189  |

## 문제 풀이

- dfs를 사용하여 모든 요소들을 더해가면서 `word`와 같은지 비교한다.
- 여기서 앞선 dfs와 다른 점은 방문 배열이 필요 없다는 점이다. 같은 요소가 5이하의 문자열이면 가능하기 떄문이다.

```js
function solution(word) {
  var answer = 0;
  let arr = ["A", "E", "I", "O", "U"];
  let keyword = []; // 요소들을 확인하면서 더해질 배열

  function moum(arr) {
    // dfs를 통해서 만들어진 keyword와 word와 같거나 keyword가 5보다 크면 return
    if (word === keyword.join("") || keyword.length >= 5) return;

    for (let i = 0; i < arr.length; i++) {
      keyword.push(arr[i]); // word와 비교할 keyword에 각 요소들을 계속 push해준다.
      answer++; //answer을 통해 keyword가 몇 번째 패턴인지++ 해나아간다.
      moum(arr); // dfs 재귀
      if (word === keyword.join("")) break; // 조건을 충당했으니 더이상 반복문을 진행할 필요가 없다.
      keyword.pop(arr[i]);
    }
  }

  for (let i = 0; i < arr.length; i++) {
    moum(arr);
    arr.push(arr.shift());
    // 조건을 충당했으니 더이상 반복문을 진행할 필요가 없다.
    if (word === keyword.join("")) break;
  }

  return answer;
}

}

```

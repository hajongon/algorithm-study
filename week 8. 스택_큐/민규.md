# 알고리즘 정리

# 10828_스택

## 문제 설명
[문제 링크](https://www.acmicpc.net/problem/10828)

스택 아래와 같은 규칙으로 구현 하시오.

- push X: 정수 X를 스택에 넣는 연산이다.
- pop: 스택에서 가장 위에 있는 정수를 빼고, 그 수를 출력한다. 만약 스택에 들어있는 정수가 	없는 경우에는 -1을 출력한다.
- size: 스택에 들어있는 정수의 개수를 출력한다.
- empty: 스택이 비어있으면 1, 아니면 0을 출력한다.
- top: 스택의 가장 위에 있는 정수를 출력한다. 만약 스택에 들어있는 정수가 없는 경우에는 -1을 출력한다.

- Input
```
14
push 1
push 2
top
size
empty
pop
pop
pop
size
empty
pop
push 3
empty
top
```
- Output

```
2
2
0
2
1
-1
0
1
-1
0
3
```


## 코드

```js
const fs = require("fs");
const input = fs.readFileSync("../input.txt").toString().trim().split("\n").map((item) => item.replace("\r","").split(" "))
input.shift();

let stack = []
let ans = []
for(let i of input){
    if(i[0] === 'push') stack.push(i[1]);
    else if(i[0] === 'pop'){
        stack.length === 0 ? ans.push(-1) :ans.push(stack.pop());
    } 
    else if(i[0] === 'size') ans.push(stack.length);
    else if(i[0] === 'empty')
        stack.length === 0 ? ans.push(1) : ans.push(0)
    else if(i[0] === 'top')
    stack.length === 0 ? ans.push(-1) : ans.push(stack[stack.length-1]);
    
}

console.log(ans.join("\n"));
```

# 10845_큐

## 문제 설명
[문제 링크](https://www.acmicpc.net/problem/10845)

큐를 아래위 같은 방식으로 구현하시오.
- push X: 정수 X를 큐에 넣는 연산이다.
- pop: 큐에서 가장 앞에 있는 정수를 빼고, 그 수를 출력한다. 만약 큐에 들어있는 정수가 없는 	경우에는 -1을 출력한다.
- size: 큐에 들어있는 정수의 개수를 출력한다.
- empty: 큐가 비어있으면 1, 아니면 0을 출력한다.
- front: 큐의 가장 앞에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- back: 큐의 가장 뒤에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.

- Input
```
15
push 1
push 2
front
back
size
empty
pop
pop
pop
size
empty
pop
push 3
empty
front
```
- Output

```
1
2
2
0
1
2
-1
0
1
-1
0
3
```


## 코드


```js
const fs = require("fs");
const input = fs.readFileSync("../input.txt").toString().trim().split("\n").map((item) => item.replace("\r","").split(" "))
input.shift();

let queue = [];
let ans = [];

for(let i of input){
    
    switch (i[0]){
        case 'push':
            queue.push(i[1]);
            break;

        case 'pop' : 
            ans.push(queue.length !==0 ? queue.shift() : '-1')
            break;

        case 'size':
            ans.push(queue.length)
            break;
        
        case 'empty':
            ans.push(queue.length !==0 ? 0 : 1)
            break;

        case 'front':
            ans.push(queue.length !==0 ? queue[0] : '-1');
            break;

        case 'back':
            ans.push(queue.length !==0 ? queue[queue.length-1] : '-1');
            break; 
    }
    
}

console.log(ans.join("\n"));

```

# 10866_덱

## 문제 설명
[문제 링크](https://www.acmicpc.net/problem/10866)

- push_front X: 정수 X를 덱의 앞에 넣는다.
- push_back X: 정수 X를 덱의 뒤에 넣는다.
- pop_front: 덱의 가장 앞에 있는 수를 빼고, 그 수를 출력한다. 만약, 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- pop_back: 덱의 가장 뒤에 있는 수를 빼고, 그 수를 출력한다. 만약, 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- size: 덱에 들어있는 정수의 개수를 출력한다.
- empty: 덱이 비어있으면 1을, 아니면 0을 출력한다.
- front: 덱의 가장 앞에 있는 정수를 출력한다. 만약 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- back: 덱의 가장 뒤에 있는 정수를 출력한다. 만약 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.

deque를 아래와 같은 방식으로 구현하시오.
(차집합: A-B는 A에 포함되어 있고, B에는 없는 요소를 가지고 있는 집합을 말한다.)


- Input
```
15
push_back 1
push_front 2
front
back
size
empty
pop_front
pop_back
pop_front
size
empty
pop_back
push_front 3
empty
front
```
- Output

```
2
1
2
0
2
1
-1
0
1
-1
0
3
```


## 코드


```js
const fs = require("fs");
const input = fs
  .readFileSync("../input.txt")
  .toString()
  .trim()
  .split("\n")
  .map((item) => item.replace("\r", "").split(" "));
input.shift();
let deque = [];
let ans = [];

for (let i of input) {
  switch (i[0]) {
    case "push_front":
      deque.unshift(i[1]);
      break;

    case "push_back":
      deque.push(i[1]);
      break;

    case "pop_front":
      deque.length !== 0 ? ans.push(deque.shift()) : ans.push(-1);
      break;

    case "pop_back":
      deque.length !== 0 ? ans.push(deque.pop()) : ans.push(-1);
      break;

    case "size":
      ans.push(deque.length);
      break;

    case "empty":
      deque.length !== 0 ? ans.push(0) : ans.push(1);
      break;

    case "front":
      deque.length !== 0 ? ans.push(deque[0]) : ans.push(-1);
      break;

    case "back":
      deque.length !== 0 ? ans.push(deque[deque.length - 1]) : ans.push(-1);
  }
}

console.log(ans.join("\n"));


```

# 1874_스택 수열

## 문제 설명
[문제 링크](https://www.acmicpc.net/problem/1874)

[스택](https://interconnection.tistory.com/105#:~:text=%EC%8A%A4%ED%83%9D%EC%9D%B4%EB%9E%80%3F%20%3A%20%ED%95%9C%20%EC%AA%BD%20%EB%81%9D%EC%9D%B4,%EB%A7%89%ED%98%80%EC%9E%88%EC%96%B4%EC%84%9C%20%EC%9E%90%EB%A3%8C%EB%A5%BC%20%ED%95%9C%20%EB%B0%A9%ED%96%A5%EC%9C%BC%EB%A1%9C%EB%A7%8C%20%EC%8C%93%EB%8A%94%20%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%9D%B4%EB%8B%A4.)을 사용하여 1부터 n까지의 수를 스택을 사용하여 하나의 수열을 만들 때, 스택에 push와 pop이 되는 순간을 출력하시오.
단, 수열이 되지 않을 경우 `NO`를 출력하시오.

- Input
```
8
4
3
6
8
7
5
2
1

5
1
2
5
3
4
```
- Output

```
+
+
+
+
-
-
+
+
-
+
+
-
-
-
-
-

NO
```


## 코드

```js
const fs = require("fs");
const input = fs.readFileSync("../input.txt").toString().trim().split("\n").map((item) => Number(item.replace("\r","")))
const N = input.shift();
const stack = [];
let card = 1;
let ans = [];
for(let i = 0; i < input.length ; i++){
    while(card <= input[i]){
            stack.push(card);
            ans.push("+");
            card++
        }
    if(stack[stack.length - 1] !== input[i]){
        ans = [];
        break;
    }
    else{
        stack.pop();
        ans.push("-");
    }

    

}
console.log(ans.length === 0 ? "NO" : ans.join("\n"))




```

# 1021_회전하는 큐

## 문제 설명
[문제 링크](https://www.acmicpc.net/problem/1021)

N개의 원소를 포함하고 있는 양방향 순환 큐가 있을 때, 해당되는 입력 값을 원소의 이동을 최소화하여 아래의 규칙을 이용하여 뽑아낼 수 있다. 이 때, 입력 값의 원소를 뽑아 낼 때 2,3번 규칙을 사용한 횟수를 구하시오.

**규칙
1. 첫 번째 원소를 뽑아낸다. 이 연산을 수행하면, 원래 큐의 원소가 a1, ..., ak이었던 것이 a2, ..., ak와 같이 된다.
2. 왼쪽으로 한 칸 이동시킨다. 이 연산을 수행하면, a1, ..., ak가 a2, ..., ak, a1이 된다.
3. 오른쪽으로 한 칸 이동시킨다. 이 연산을 수행하면, a1, ..., ak가 ak, a1, ..., ak-1이 된다.
 

- Input
```
10 3
1 2 3

10 3
2 9 5

32 6
27 16 30 11 6 23
```
- Output

```
0

8

59
```


## 코드

```js
const fs = require("fs");
const input = fs
  .readFileSync("../input.txt")
  .toString()
  .trim()
  .split("\n")
  .map((item) => item.split(" "));
const [N, M] = input.shift().map(Number);
const arr = input[0].map((item) => Number(item));
let queue = Array.from({ length: N }, (_, i) => i + 1);
let [ans, mid] = [0, 0];

for (let i = 0; i < M; i++) {
  mid = parseInt(queue.length / 2);
  if (mid < queue.findIndex((item) => arr[i] === item)) {
    while (arr[i] !== queue[0]) {
      queue.unshift(queue.pop());
      ans++;
    }
  } else {
    while (arr[i] !== queue[0]) {
      queue.push(queue.shift());
      ans++;
    }
  }

  queue.shift();
}
console.log(ans);


```

# 9012_괄호

## 문제 설명
[문제 링크](https://www.acmicpc.net/problem/9021)

괄호 문자열 `(`,`)`로 이루어진 문자열들로 이루어진 배열을 주어질 때, 양 쪽 괄호가 순서에 맞게 연결되어 있으면 `YES`, 안 되면 `NO`를 출력하시오.

**예시
- 올바른 괄호: (())(), ((()))
- 올바르지 않는 괄호: (()(, (())()))
 

- Input
```
6
(())())
(((()())()
(()())((()))
((()()(()))(((())))()
()()()()(()()())()
(()((())()(
```
- Output

```
NO
NO
YES
NO
YES
NO
```


## 코드

```js
const fs = require("fs");
const input = fs
  .readFileSync("../input.txt")
  .toString()
  .trim()
  .split("\n")
  .map((item) => item.split(" "));
const [N, M] = input.shift().map(Number);
const arr = input[0].map((item) => Number(item));
let queue = Array.from({ length: N }, (_, i) => i + 1);
let [ans, mid] = [0, 0];

for (let i = 0; i < M; i++) {
  mid = parseInt(queue.length / 2);
  if (mid < queue.findIndex((item) => arr[i] === item)) {
    while (arr[i] !== queue[0]) {
      queue.unshift(queue.pop());
      ans++;
    }
  } else {
    while (arr[i] !== queue[0]) {
      queue.push(queue.shift());
      ans++;
    }
  }

  queue.shift();
}
console.log(ans);


```

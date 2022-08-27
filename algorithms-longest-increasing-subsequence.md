## Backtrack

```mermaid

stateDiagram-v2

    AA: subproblem(4) 
    AB: subproblem(3)
    AC1: subproblem(2)
    AC2: subproblem(2)
    AD1: subproblem(1)
    AD2: subproblem(1)
    AD3: subproblem(1)
    AD4: subproblem(1)

    BB: subproblem(3)
    BC1: subproblem(2)
    BC2: subproblem(2)
    BD1: subproblem(1)
    BD2: subproblem(1)
    BD3: subproblem(1)
    BD4: subproblem(1)

    BB --> BD2
    BB --> BC2
    
    BC1 --> BD3

    BC2 --> BD4

    AA --> AD1
    AA --> AC1
    AA --> AB

    AB --> AD2
    AB --> AC2
    
    AC1 --> AD3
    
    AC2 --> AD4
```

```js
function longestIncreasingSubsequence(arr) {
  function subproblem(index) {
    if (index == 0) {
      return 1
    }

    let max = 1
    for (let i = index - 1; i >= 0; i--) {
      if (arr[i] < arr[index]) {
        max = Math.max(max, 1 + subproblem(i))
      }
    }
    
    return max
  }

  let max = 1
  for (let i = 0; i < arr.length; i++) {
    max = Math.max(max, subproblem(i))
  }

  return max
}
```

## Backtrack

### 

```mermaid

stateDiagram-v2
    []
    [] --> [arr[0]]
    [] --> [arr[1]]
    [] --> [arr[2]]
    
    [arr[0]] --> [arr[0],arr[1]]
    [arr[0]] --> [arr[0],arr[2]]

    [arr[1]] --> [arr[1],arr[2]]

    [arr[0],arr[1]] --> [arr[0],arr[1],arr[2]]
```

### Include / Exclude

```mermaid

stateDiagram-v2
    A: []
    B: []
    C: [arr[0]]
    D: []
    E: [arr[0]]
    F: [arr[0], arr[1]]
    G: [arr[1]]
    H: []
    I: [arr[2]]
    J: [arr[1]]
    K: [arr[1], arr[2]]
    L: [arr[0]]
    M: [arr[0], arr[2]]
    O: [arr[0], arr[1]]
    P: [arr[0], arr[1], arr[2]]

    A --> B
    A --> C
    B --> D
    B --> G
    
    C --> E
    C --> F

    D --> H
    D --> I

    G --> J
    G --> K

    E --> L
    E --> M

    F --> O
    F --> P

```

```js
function longestIncreasingSubsequence(int[] arr, int i, int n, int prev) {
  if (i === n) {
    return 0;
  }

  const excl = longestIncreasingSubsequence(arr, i + 1, n, prev);

  let incl = 0;
  if (arr[i] > prev) {
    incl = 1 + longestIncreasingSubsequence(arr, i + 1, n, arr[i]);
  }

  return Math.max(incl, excl);
}
```

## Dynamic Programming

### Memoization

```mermaid

stateDiagram-v2
    A : subproblem(4)
    B : subproblem(3)
    C : subproblem(2)
    D : subproblem(1)
    A --> D
    A --> C
    A --> B
    
    C --> D

    B --> D
    B --> C

    AB : subproblem(3)
    AC : subproblem(2)
    AD : subproblem(1)
    
    AC --> AD

    AB --> AD
    AB --> AC

    BC : subproblem(2)
    BD : subproblem(1)
    
    BC --> BD

    CD : subproblem(1)
```

```js
function longestIncreasingSubsequence(arr) {
  let subproblemMemo = []

  function subproblem(index) {
    if (index == 0) {
      return 1
    }

    if (subproblemMemo[index]) {
      return subproblemMemo[index]
    }

    let max = 1
    for (let i = index - 1; i >= 0; i--) {
      if (arr[i] < arr[index]) {
        max = Math.max(max, 1 + subproblem(i))
      }
    }

    if (max > subproblemMemo[index]) {
      subproblemMemo[index] = max
    }
    
    return max
  }

  let max = 1
  for (let i = 0; i < arr.length; i++) {
    max = Math.max(max, subproblem(i))
  }

  return max
}
```

### Tabulation

```mermaid
stateDiagram-v2
    A : subproblem(4)
    B : subproblem(3)
    C : subproblem(2)
    D : subproblem(1)
    D --> A
    C --> A
    B --> A

    D --> C
    
    D --> B
    C --> B

    AB : subproblem(3)
    AC : subproblem(2)
    AD : subproblem(1)
    
    AD --> AC
    
    AD --> AB
    AC --> AB

    BC : subproblem(2)
    BD : subproblem(1)
    
    BD --> BC

    CD : subproblem(1)
```

```js
function longestIncreasingSubsequence(arr) {
  let subproblemTable = new Array(arr.length).fill(0);
  let max = 0;

  for (let i = 1; i < n; i++) {
    for (let j = 0; j < i; j++) {
      if (arr[i] > arr[j] && subproblemTable[i] < subproblemTable[j] + 1) {
        subproblemTable[i] = subproblemTable[j] + 1;
      }
    }
  }

  return Math.max(...subproblemTable);
}
```
## Greedy

```js
function longestIncreasingSubsequence(arr) {
  let pileTop = new Array(arr.length).fill(Infinity)
  let pileLength = new Array(arr.length).fill(0)

  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length; j++) {
      if (pileTop[j] > arr[i]) {
        pileTop[j] = arr[i]
        pileLength[j] += 1
        break
      }
    }
  }

  return Math.max(...pileLength)
}
```

### Greedy x Binary Search

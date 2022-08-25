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

  for (i = 0; i < n; i++) {
    if (max < subproblemTable[i]) {
      max = subproblemTable[i]; 
    }
  }

  return max;
}
```

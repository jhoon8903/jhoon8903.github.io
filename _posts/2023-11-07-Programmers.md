---
title: Linq Skip, Take
author: "이정훈"
category: Algorithm
tags: [Algorithm, Programming, csharp]
img : https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb260cae4-a3d0-448b-be5d-7486d5925148%2F34.png?table=block&id=9e7562fc-62db-4d05-bb21-4e95a2e04542&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2
comments_disable: true
---

![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb260cae4-a3d0-448b-be5d-7486d5925148%2F34.png?table=block&id=9e7562fc-62db-4d05-bb21-4e95a2e04542&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

# LINQ - Skip, Take

- 데이터 컬렉션을 관리하는 기능을 제공하는 Skip, Take
- 컬렉션에서 특정 수의 요소를 건너뛰거나 선택하는데 사용 됨

## Skip

>시퀀스의 시작에서 지정된 수의 요소를 건너뛰고 나머지 요소를 반환<br>`array.Skip(2)`의 배열은  `처음 두 요소를 건너뛰고 나머지 부분`을 반환

```csharp
int [] array = new int[] {0,1,2,3,4,5}
result = array.Skip(2); 
result // [2,3,4,5];
```

## Take

> 시퀀스의 시작에서 지정된 수의 연속된 요소를 반환<br>`array.Take(5)`는 배열의 처음 다섯 요소를 반환

```csharp
int [] array = new int [] {0,1,2,3,4,5,6,7}
result = array.Take(5);
result // [0,1,2,3,4]
```

## Skip + Take

> 두 메소드를 결합하면, 시퀀스의 중간 부분을 선택할 수 있음<br>만약 `세 번째` 부터 `일곱 번째` 요소까지 추출하려면 array.Skip(3).Take(5);<br>array = [0,1,2,3,4,5,6,7,8,9] <br> result // [2,3,4,5,6] 

# K 번째 수 - Programmers

### 문제 설명

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.
예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

>1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
>2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
>3. 2에서 나온 배열의 3번째 숫자는 5입니다.

배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- array의 길이는 1 이상 100 이하입니다.
- array의 각 원소는 1 이상 100 이하입니다.
- commands의 길이는 1 이상 50 이하입니다.
- commands의 각 원소는 길이가 3입니다.

##### 입출력 예

|array|commands|return|
|---|---|---|
|`[1, 5, 2, 6, 3, 7, 4]`|`[[2, 5, 3], [4, 4, 1], [1, 7, 3]]`|`[5, 6, 3]`|

```csharp
using System;
using System.Linq;
using System.Collections.Generic;

public class Solution 
{
    public int[] solution(int[] array, int[,] commands) 
    {
        List<int> commandSelect = new List<int>();
        for (int targetIndex = 0; 
        targetIndex < commands.GetLength(0); 
        targetIndex++)
        {
            int i = commands[targetIndex, 0];
            int j = commands[targetIndex, 1];
            int k = commands[targetIndex, 2];
            int[] sliceArray = array.Skip(i - 1).Take(j - i + 1).ToArray();
            Array.Sort(sliceArray);
            commandSelect.Add(sliceArray[k-1]);
        }
        return commandSelect.ToArray();
    }
}
```

1. `commandSelect`라는 이름의 정수 리스트를 생성합니다. 이 리스트는 최종 결과를 담게 될 것입니다.
    
2. `for` 루프를 사용하여 `commands` 배열의 각 요소를 순회합니다. `GetLength(0)` 메소드는 `commands` 배열의 첫 번째 차원(행의 개수)을 반환합니다. 이 경우 각 명령어 행을 순회하기 위해 사용됩니다.
    
3. `for` 루프 내에서 각 명령어는 `i`, `j`, `k` 세 개의 값을 갖습니다. 
   각각은 `commands` 배열에서 해당 위치의 값을 가져옵니다.
    - `i`는 시작 인덱스,
    - `j`는 종료 인덱스,
    - `k`는 반환할 원소의 인덱스를 의미합니다.
4. `array.Skip(i - 1)`는 주어진 배열에서 `i-1` 개의 요소를 건너뛰고 (`i`는 1부터 시작하는 인덱스임을 가정), `Take(j - i + 1)`는 건너뛴 후 `j - i + 1` 개의 요소를 선택하여 새로운 배열로 만듭니다.
    
5. 이렇게 만들어진 배열 `sliceArray`는 `Array.Sort`를 사용하여 정렬됩니다.
    
6. 정렬된 배열에서 `k-1` 위치의 요소를 `commandSelect` 리스트에 추가합니다. (`k`도 1부터 시작하는 인덱스를 가정하기 때문에 0부터 시작하는 배열 인덱스로 변환하기 위해 1을 뺍니다).
    
7. 모든 명령어가 처리되고 난 후, `commandSelect` 리스트는 `ToArray()` 메소드를 사용하여 배열로 변환되고, 이 배열이 최종적으로 반환됩니다.

예시를 통해 살펴보겠습니다:

- `array`: `[1, 5, 2, 6, 3, 7, 4]`
- `commands`: `[[2, 5, 3], [4, 4, 1], [1, 7, 3]]`

첫 번째 `command`는 `[2, 5, 3]`입니다:

- `i` = 2, `j` = 5, `k` = 3
- `array.Skip(1)` = `[5, 2, 6, 3, 7, 4]`
- `.Take(4)` = `[5, 2, 6, 3]`
- 정렬 후 = `[2, 3, 5, 6]`
- `k-1` 인덱스는 `3-1` = `2`, 값은 `5`

두 번째 `command`는 `[4, 4, 1]`입니다:

- `i` = 4, `j` = 4, `k` = 1
- `array.Skip(3)` = `[6, 3, 7, 4]`
- `.Take(1)` = `[6]`
- 정렬 후 = `[6]` (변함 없음)
- `k-1` 인덱스는 `1-1` = `0`, 값은 `6`

세 번째 `command`는 `[1, 7, 3]`입니다:

- `i` = 1, `j` = 7, `k` = 3
- `array.Skip(0)` = `[1, 5, 2, 6, 3, 7, 4]` (원본 배열 그대로)
- `.Take(7)` = `[1, 5, 2, 6, 3, 7, 4]` (원본 배열 그대로)
- 정렬 후 = `[1, 2, 3, 4, 5, 6, 7]`
- `k-1` 인덱스는 `3-1` = `2`, 값은 `3`

따라서 이 `solution` 메소드는 `[5, 6, 3]`을 최종 결과로 반환합니다.
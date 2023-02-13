---
layout: single
title: "징검다리 건너기"
categories: Programmers
tag: [java, Programmers]
---

## 문제
---
카카오 초등학교의 "니니즈 친구들"이 "라이언" 선생님과 함께 가을 소풍을 가는 중에 징검다리가 있는 개울을 만나서 건너편으로 건너려고 합니다. "라이언" 선생님은 "니니즈 친구들"이 무사히 징검다리를 건널 수 있도록 다음과 같이 규칙을 만들었습니다.

- 징검다리는 일렬로 놓여 있고 각 징검다리의 디딤돌에는 모두 숫자가 적혀 있으며 디딤돌의 숫자는 한 번 밟을 때마다 1씩 줄어듭니다.
- 디딤돌의 숫자가 0이 되면 더 이상 밟을 수 없으며 이때는 그 다음 디딤돌로 한번에 여러 칸을 건너 뛸 수 있습니다.
- 단, 다음으로 밟을 수 있는 디딤돌이 여러 개인 경우 무조건 가장 가까운 디딤돌로만 건너뛸 수 있습니다.

"니니즈 친구들"은 개울의 왼쪽에 있으며, 개울의 오른쪽 건너편에 도착해야 징검다리를 건넌 것으로 인정합니다.

"니니즈 친구들"은 한 번에 한 명씩 징검다리를 건너야 하며, 한 친구가 징검다리를 모두 건넌 후에 그 다음 친구가 건너기 시작합니다.

디딤돌에 적힌 숫자가 순서대로 담긴 배열 stones와 한 번에 건너뛸 수 있는 디딤돌의 최대 칸수 k가 매개변수로 주어질 때, 최대 몇 명까지 징검다리를 건널 수 있는지 return 하도록 solution 함수를 완성해주세요.

## 제한사항
---
- 징검다리를 건너야 하는 니니즈 친구들의 수는 무제한 이라고 간주합니다.
- stones 배열의 크기는 1 이상 200,000 이하입니다.
- stones 배열 각 원소들의 값은 1 이상 200,000,000 이하인 자연수입니다.
- k는 1 이상 stones의 길이 이하인 자연수입니다.

## 입출력 예
---  

| stones | k          | result |
|: ------------- |:-------------:|:---:|
|[2, 4, 5, 3, 2, 1, 4, 2, 5, 1] | 3 | 3 |

## 입출력 예에 대한 설명
---
입출력 예 #1  

첫 번째 친구는 다음과 같이 징검다리를 건널 수 있습니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/4560e242-cf83-4e77-a14c-174f3831499d/step_stones_104.png">

첫 번째 친구가 징검다리를 건너 후 디딤돌에 적힌 숫자는 아래 그림과 같습니다.
두 번째 친구도 아래 글미과 같이 징검다리를 건널 수 있습니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d64f29ac-3e35-4fd3-91fa-4d70e3b6c80a/step_stones_101.png">

두 번째 친구가 징검다리를 건넌 후 디딤돌에 적힌 숫자는 아래 그림과 같습니다.
세 번째 친구도 아래 그림과 같이 징검다리를 건널 수 있습니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/369bc8a1-7017-4135-a499-505247ab9cfc/step_stones_102.png">

세 번째 친구가 징검다리를 건넌 후 디딤돌에 적힌 숫자는 아래 그림과 같습니다.
네 번째 친구가 징검다리를 건너려면, 세 번째 디딤돌에서 일곱 번째 디딤돌로 네 칸을 건너 뛰어야 합니다. 하지만 k = 3 이므로 건너뛸 수 없습니다.

<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/e44e0a83-e637-48ad-858c-4c135c3b078f/step_stones_103.png">

따라서 최대 3명이 디딤돌을 모두 건널 수 있습니다.

## 풀이
---
두 가지 풀이 방법이 있다.
1. Slide Window
1. Binary Search

### Slide Window
    k의 크기를 가진 Window를 이용해 탐색을 한다. Window에서의 최대값을 리스트에 담고 그 리스트에서의 최솟값이 정답이 된다.

    Window사이즈-1 만큼의 공백은 건널 수 있고, 따라서 Window안의 어떤 징검다리든 하나만 밟는다면 다음 Window로 넘어갈 수 있기 때문에 최대값을 구하는 것이다.

    결국 그 최대값들 중 최솟값이 최대 건널 수 있는 친구의 수이다.

    윈도우 사이즈 만큼 덱의 크기를 유지하며 현재 덱에서 가장 큰 값을 정답과 비교하며 최대값 중 최솟값을 구하면 된다.

    덱의 첫번째 값이 윈도우 밖의 값, 즉 윈도우 범위 외의 값이면 제거.

    덱의 마지막 값이 현재값보다 작다면, 덱의 최대값을 구하는 과정이기에 필요없는 값이다. 따라서 마지막 값을 제거한다.

```java
import java.util.*;
// 데이터 클래스
class Data{
    int value, index;
    public Data(int value, int index) {
        this.value = value;
        this.index = index;
   }
}
class Solution {
    public static int solution(int[] stones, int k) {
        int answer = Integer.MAX_VALUE;
        ArrayDeque<Data> q = new ArrayDeque<>();  //슬라이딩 윈도우를 위한 덱
        for(int i = 0; i < stones.length; i++) {
            // 덱의 첫번째 값이 윈도우 밖의 값일때
            if(!q.isEmpty() && q.peek().index <= i - k) {
                q.pollFirst();
            }
            // 덱의 마지막 값이 현재값보다 작을때
            while(!q.isEmpty() && q.peekLast().value <= stones[i]) {
                q.pollLast();
            }
            // 덱에 현재값 삽입
            q.add(new Data(stones[i], i));
            // 덱에 있는 값 중 최대값이 정답보다 작은 경우
            if(answer >= q.peekFirst().value && i >= k - 1) {
                answer = q.peekFirst().value;
            }
        }
        return answer;
    }
}
```

### Binary Search
    N의 최대값인 200,000을 hi로 설정해 이분탐색을 수행한다. count를 세어 mid보다 값이 작은 징검다리가 k개이상이라면 반복문을 중단시켜 탐색 범위를 줄여나간다.

```java
class Solution {
    public int solution(int[] stones, int k) {
        int answer = 0; // 징검다리를 건너는 친구의 최소 수
        int min = 1;    // 징검다리를 건너는 친구의 최대 수
        int max = 200000000;    // 이분탐색 할 기준 : 징검다리는 건널 수 있는 친구의 수
        while (min <= max) {
            int mid = (min + max) / 2;  // 징검다리를 건널 인원
            // 징검다리 건널 수 있는지 확인
            if (canAcrossRiver(stones, k, mid)) {
              	// 다리를 건널 수 있으면 더 많은 친구들이 가능한지 친구의 수를 넓힘
                min = mid + 1;
                answer = Math.max(answer, mid);
            } else {
                // 다리를 건널 수 없으면 친구의 수를 좁힘
                max = mid - 1;
            }
        }
        return answer;
    }
    // 징검다리를 건널 수 있는지 판별
    boolean canAcrossRiver(int[] stones, int k, int friends) {
      	int skip = 0;   // 못 건너는 징검다리의 개수를 저장
        for (int stone : stones) {
            // 디딤돌의 숫자 - 건너는 친구의 수
            if (stone - friends < 0)
              	// 0보다 작으면 건널 수 없음을 의미
                skip++;
            else
              	// 0 이상이면 건널 수 있음의 의미 = 이 지점에 위치하면 다음으로 갈 수 있음
                // 다시 0으로 갱신
                skip = 0;
            // 못 건너는 징검다리의 수가 최대칸 k를 넘으면 해당 값은 건널 수 없음을 의히
            if (skip >= k)
                return false;
        }
        return true;
    }
}
```

### 첫 번째 시도

처음에 윈도우 방식을 생각해서 시도했으나, 디테일이 좀 부족했던 부분이 윈도우의 최대값을 찾고 다음 윈도우로 갈 때, 윈도우 사이즈만큼 이동해서 탐색이 아닌 단지 1칸 옆으로만 이동했다. 따라서 윈도우가 겹치는 부분이 많았고, 결국 시간 초과가 되었다.

문제에서 주어진 조건을 생각해보면 윈도우가 안 겹치게 놓아야 시간 내에 윈도우 방식으로 문제를 해결할 수 있다.

덱을 사용하면 안 겹치는 방법으로 구현이 가능하고 첫번째 방법이 그 풀이이다.

---

## 고찰

데이터 사이즈를 보고 잘 판별해야 한다. log(2억) = 27~28이다. 데이터 사이즈는 20만.

범위를 잘 조합해봐도 복잡도가 1억 이상이 나오면 이분탐색 유형일 확률이 높다. 이런 감각을 문제를 많이 풀어 키울 필요가 있다.

따라서, 범위가 크다면 dp나 이분탐색이고 문제 유형을 보고 빠르게 판단해 접근하자.
---
title: "Java 문제풀이 - 인사고과"
excerpt: "Programmers Java 문제 풀이"
author: Kwoo9
date: 2025-01-03
categories: [Programmers, Java]
tags: [Programmers, Java]
toc: true
toc_sticky: true
---

오늘은 Java 알고리즘 문제를 하나 가져왔습니다.  
요새 알고리즘 문제를 놓고있었더니 감이 떨어져서 재활훈련하는 그런 느낌으로  
하나씩 해보고 있습니다.

LV3의 **인사고과** 라는 문제입니다.
<details><summary>문제 설명
</summary>

완호네 회사는 연말마다 1년 간의 인사고과에 따라 인센티브를 지급합니다. 각 사원마다 근무 태도 점수와 동료 평가 점수가 기록되어 있는데 만약 어떤 사원이 다른 임의의 사원보다 두 점수가 모두 낮은 경우가 한 번이라도 있다면 그 사원은 인센티브를 받지 못합니다. 그렇지 않은 사원들에 대해서는 두 점수의 합이 높은 순으로 석차를 내어 석차에 따라 인센티브가 차등 지급됩니다. 이때, 두 점수의 합이 동일한 사원들은 동석차이며, 동석차의 수만큼 다음 석차는 건너 뜁니다. 예를 들어 점수의 합이 가장 큰 사원이 2명이라면 1등이 2명이고 2등 없이 다음 석차는 3등부터입니다.

각 사원의 근무 태도 점수와 동료 평가 점수 목록 scores이 주어졌을 때, 완호의 석차를 return 하도록 solution 함수를 완성해주세요.

**제한 사항**  
1 ≤ scores의 길이 ≤ 100,000  
scores의 각 행은 한 사원의 근무 태도 점수와 동료 평가 점수를 나타내며 [a, b] 형태입니다.  
scores[0]은 완호의 점수입니다.  
0 ≤ a, b ≤ 100,000  
완호가 인센티브를 받지 못하는 경우 -1을 return 합니다.
</details>


결국 두 점수가 다른 한 사람의 두 점수에 비해 각각 더 낮으면  
**인센티브가 없다**는 그런 무서운 내용입니다.

여기서 문제를 풀때 고려한 것은  
1. 원호가 인센티브 대상자가 아닌경우  
2. 랭킹은 동점자는 같은 등수이기에 결국 자기보다 큰 사람이 몇 명인지 알면 된다.
3. 두 가지 점수로 비교하기에, 한 점수를 기준으로 정렬하면 하나의 점수로만 비교 가능하다.  
4. 이에 근무태도를 내림차순, 동료평가를 오름차순 정렬  
4-1. 이 이유로는 근무 태도가 동점자가 있을 가능성 존재  
4-2. 이에, 동료 평가 점수를 오름차순으로 설정  


```java
import java.util.*;
class Solution {
    public int solution(int[][] scores) {
        int answer = 1;
        int[] origianl = scores[0];
        int sum = scores[0][0] + scores[0][1];
        int max = 0;
        
        Arrays.sort(scores, (o1, o2) -> {
            if(o1[0] == o2[0]){
                return o1[1] - o2[1];
            }            
            return o2[0] - o1[0];
        });
        
        for(int[] score : scores){
            if(max <= score[1]){
                max = score[1];
                if(score[0] + score[1] > sum){
                    answer++;
                }
            }
            else{
                if(score.equals(origianl)){
                    return -1;
                }
            }
        }
        
        return answer;
    }
}
```
이렇게 문제를 풀었는데요

여기서 오늘 헷갈렸던 부분은 저 original 부분입니다.  
문제를 읽다보면 scores의 처음이 원호의 점수로 주어집니다.  
이제 이걸 정렬 전 원호의 점수를 저장해놓고 비교를 했어야 하는데  
무작정 정렬 후 scores[0]과 비교를 해버렸더니 당연히 안풀렸습니다....

참고사이트  
https://mysterlee.tistory.com/109
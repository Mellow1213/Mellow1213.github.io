---
title: "[Algorithm] 정렬 알고리즘 이론"
excerpt: "정렬 알고리즘에 대한 이론 정리"
categories: 
  - algorithm
tags:
  - algorithm
  - sort
toc: true
date: 2024-10-07
last_modified_at: 2024-10-07
---

# 정렬 알고리즘

정렬 알고리즘(sorting algorithm)이란 원소들을 번호순이나 사전 순서와 같이 일정한 순서대로 열거하는 알고리즘이다. 

즉 어떤 데이터(혹은 원소)들이 주어졌을 때 이를 정해진 순서대로 나열하는 문제.
<br><br>

효율적인 정렬은 탐색이나 병합 알고리즘처럼 (정렬된 리스트에서 바르게 동작하는) 다른 알고리즘을 최적화하는 데 중요하며 데이터의 정규화나 의미있는 결과물을 생성하는 데 유용히 쓰인다. 

## 정렬 알고리즘을 사용하는 이유
### 정렬 데이터의 특징

실제 컴퓨터 분야에서 사용하는 데이터의 경우 숫자의 순서나 어휘의 순서대로 정렬한 다음 사용해야 되는 경우가 거의 항상 발생하는데 이걸 얼마나 효과적으로 해결할 수 있느냐가 정렬 문제의 핵심이고 정렬 알고리즘의 효율을 따지는 데 제일 중요한 요소가 된다.

이미 정렬된 데이터의 특징은 어떤 값을 임의로 집었을 때 해당 위치 오른쪽에는 무조건 그것보다 크거나 같은 값, 왼쪽에는 무조건 그것보다 작거나 같은 값이 있다는 것이다. 

따라서 탐색을 할 때 컴퓨터가 찾고자 하는 값보다 임의로 집은 값이 **작다면** 다음 탐색에는 오른쪽만 보면 된다.

컴퓨터가 어떤 값을 집어올리는 위치가 후보군의 가운데인 탐색 알고리즘이 이진 탐색 알고리즘이다. 

이진 탐색 알고리즘은 최악의 경우라도 $log⁡n$의 성능을 보이는데 예를 들어 43억 개의 **정렬된** 자료가 들어있는 데이터에서 어떤 값을 찾아야 할 때 최악의 비교 횟수(찾는 값이 없는 경우)는 겨우 32회에 불과하다. 33회 비교시에는 약 86억 개의 자료를 탐색할 수 있다. 

컴퓨터에서 정렬을 수행하는 이유 중 하나는 바로 이 이진 탐색이 가능한 데이터를 만들기 위해서이다. 이는 알고리즘 문제를 풀다 보면 확실히 느끼는 될 부분이다.



### 정렬 알고리즘 성능

다른 알고리즘과 마찬가지로 Big-O로 표현되는 시간 복잡도가 정렬 알고리즘의 성능을 나타내는 데 제일 중요하다.

정렬 알고리즘은 입력받는 데이터의 상태에 따라 평균, 최선, 최악의 경우의 시간 복잡도가 다르다.

![](/Attatchments/241007/sort-algorithm-Big-O.png)

정렬 알고리즘의 경우 최선의 시간복잡도가 $O(nlogn)$이다.
아직 $O(n)$의 시간 복잡도 정렬 알고리즘을 찾지 못한 것이 아니라 수학적으로 증명했을 때 이 정렬 알고리즘 문제의 구조 상 제일 빠른 알고리즘이 $O(nlogn)$인 것이다.

이 외에는 메모리의 탐색 횟수, 데이터의 교환 횟수 등등과
안정/불안정성이 있다.

모든 경우에 최선을 내는 알고리즘은 없으므로 각 알고리즘의 성능 및 특징을 잘 아는 것이 중요하다.



### 안정 정렬? 불안정 정렬?

안정 정렬(stable sort)은 중복된 값을 입력 순서와 동일하게 정렬한다. 
불안정 정렬(unstable sort)은 정렬을 한 뒤에도 기존의 순서 유지가 보장되지 않을 수도 있는 정렬이다. 섞일 수도 있고 아닐 수도 있다.

좋은 예시가 있어서 꺼무위키에서 임시로 이미지를 빌려왔다.
// 추후에 더 좋은 이미지를 준비할게요!

안정 정렬의 경우에는 기존의 시간 순으로 정렬했던 순서는 지역명으로 재정렬하더라도 기존 순서가 그대로 유지된 상태에서 정렬이 이뤄진다. 그러나 불안정 정렬의 경우에는 시간순으로 정렬한 값을 지역명으로 재정렬하면 기존의 정렬 순서는 무시된 채 모두 뒤죽박죽 뒤섞이고 만다.  
  
![5-17-4](https://i.namu.wiki/i/eQSUWF3e97XxPv1T7GtWpFPBKjc5Nuwx1xypw_ZioU_NsX2RI07cWHY79Vo7J9GJUHu6_SHrnt7sy2ToqaeMpGMFx864iTpAZaA-vnG1k5ZYhLgYziDmexexPkJHycy9JNgCsPG36mutvt7d8s7mwA.webp)  


이처럼 입력값이 유지되는 안정 정렬 알고리즘이 불안정 정렬 알고리즘보다 많은 상황에서 사용할 수 있을 것이다.
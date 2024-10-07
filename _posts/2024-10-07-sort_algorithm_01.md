---
title: "Sorting Algorithm 1. 정렬 알고리즘이란 무엇인가"
excerpt: "정렬 알고리즘이 뭔데? 왜 알아야 하는데?"
categories: 
	- CS
tags:
	- [CS, Algorithm, Sorting]
toc: true
date: 2024-10-07
last_modified_at: 2024-10-07
---


코딩테스트를 풀다보면, 혹은 프로그램을 개발하다보면 주로 마주치는 문제들은 데이터를 **정렬** 시켜야 하는 경우를 많이 볼 수 있다.

알고리즘을 공부하다보면 이 정렬에 대한 알고리즘을 중요하게 다루는데, 이와 같이 정렬 알고리즘은 개발자의 기초 중 하나라고 할 수 있다.

모든 분야 및 서비스에서 정렬 알고리즘은 아주 많이 쓰인다.

- 배달 앱의 경우 인기순, 판매량순, 최소 주문 가격 등등을 기준으로 정렬되어있다.
- 구글에 검색을 하면 내 검색어와의 관련도, 작성 날짜 등등으로 정렬할 수 있다.
- 스마트폰 연락처 목록도 이름 순으로 정렬되어 있다
- 온라인 쇼핑몰 상품도 가격, 인기도, 리뷰 점수 등으로 정렬
- 학교 성적표 - 과목명 또는 점수순으로 정렬
- 파일 탐색기에서 폴더 내 파일들 - 이름, 크기, 수정 날짜순으로 정렬

그렇다면, 이 중요한 정렬을 구현하기 위한 알고리즘은 대체 무엇이며, 어떻게 구현하는 것일까?

이에 대해서, 가장 기초가 되는 정렬 알고리즘을 정리하기 위해 해당 포스트를 썼다.

# 정렬 알고리즘

> [!cite]
> 정렬 알고리즘(sorting algorithm)이란 원소들을 번호순이나 사전 순서와 같이 일정한 순서대로 열거하는 알고리즘이다. 효율적인 정렬은 탐색이나 병합 알고리즘처럼 (정렬된 리스트에서 바르게 동작하는) 다른 알고리즘을 최적화하는 데 중요하다. 또 정렬 알고리즘은 데이터의 정규화나 의미있는 결과물을 생성하는 데 유용히 쓰인다.
> 
> \- [위키피디아 중 정렬 알고리즘 정의](https://ko.wikipedia.org/wiki/%EC%A0%95%EB%A0%AC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)


어떤 데이터(혹은 원소)들이 주어졌을 때 이를 정해진 순서대로 나열하는 문제.

실제 컴퓨터 분야에서 사용하는 데이터의 경우 숫자의 순서나 어휘의 순서대로 정렬한 다음 사용해야 되는 경우가 거의 항상 발생하는데 이걸 얼마나 효과적으로 해결할 수 있느냐가 정렬 문제의 핵심이고 정렬 알고리즘의 효율을 따지는 데 제일 중요한 요소가 된다.

### 왜 정렬된 데이터가 필요한가?
정렬된 데이터 자체가 필요한 경우 외에도 탐색을 하기 위해 정렬 알고리즘을 사용한다.

이미 정렬된 데이터의 특징은 어떤 값을 임의로 집었을 때 해당 위치 오른쪽에는 무조건 그것보다 크거나 같은 값, 왼쪽에는 무조건 그것보다 작거나 같은 값이 있다는 것이다. 

따라서 탐색을 할 때 컴퓨터가 찾고자 하는 값보다 임의로 집은 값이 **작다면** 다음 탐색에는 오른쪽만 보면 된다.

컴퓨터가 어떤 값을 집어올리는 위치가 후보군의 가운데인 탐색 알고리즘이 이진 탐색 알고리즘이다. 

이진 탐색 알고리즘은 최악의 경우라도 $log⁡n$의 성능을 보이는데 예를 들어 43억 개의 **정렬된** 자료가 들어있는 데이터에서 어떤 값을 찾아야 할 때 최악의 비교 횟수(찾는 값이 없는 경우)는 겨우 32회에 불과하다. 33회 비교시에는 약 86억 개의 자료를 탐색할 수 있다. 

컴퓨터에서 정렬을 수행하는 이유 중 하나는 바로 이 이진 탐색이 가능한 데이터를 만들기 위해서이다. 이는 알고리즘 문제를 풀다 보면 확실히 느끼는 될 부분이다.

# 정렬 알고리즘 성능
다른 알고리즘과 마찬가지로 Big-O로 표현되는 시간 복잡도가 정렬 알고리즘의 성능을 나타내는 데 제일 중요하다.

이러한 시간 복잡도는 입력받는 데이터의 상태에 따라 평균, 최선, 최악의 경우의 시간 복잡도가 다르게 된다.
<--- 예시 내용 추가 --->

이 외에는 메모리의 탐색 횟수, 데이터의 교환 횟수 등등과
안정/불안정성이 있다.

모든 경우에 최선을 내는 알고리즘은 없으므로 각 알고리즘의 성능 및 특징을 잘 아는 것이 중요하다.

# 안정 정렬? 불안정 정렬?
안정 정렬(stable sort)은 중복된 값을 입력 순서와 동일하게 정렬한다. 
불안정 정렬(unstable sort)은 정렬을 한 뒤에도 기존의 순서 유지가 보장되지 않을 수도 있는 정렬이다. 섞일 수도 있고 아닐 수도 있다.

좋은 예시가 있어서 꺼무위키에서 임시로 이미지를 빌려왔다.
// 추후에 더 좋은 이미지를 준비할게요!

안정 정렬의 경우에는 기존의 시간 순으로 정렬했던 순서는 지역명으로 재정렬하더라도 기존 순서가 그대로 유지된 상태에서 정렬이 이뤄진다. 그러나 불안정 정렬의 경우에는 시간순으로 정렬한 값을 지역명으로 재정렬하면 기존의 정렬 순서는 무시된 채 모두 뒤죽박죽 뒤섞이고 만다.  
  
![5-17-4](https://i.namu.wiki/i/eQSUWF3e97XxPv1T7GtWpFPBKjc5Nuwx1xypw_ZioU_NsX2RI07cWHY79Vo7J9GJUHu6_SHrnt7sy2ToqaeMpGMFx864iTpAZaA-vnG1k5ZYhLgYziDmexexPkJHycy9JNgCsPG36mutvt7d8s7mwA.webp)  


이처럼 입력값이 유지되는 안정 정렬 알고리즘이 불안정 정렬 알고리즘보다 많은 상황에서 사용할 수 있을 것이다.


다음 포스트에서는 왜 정렬 알고리즘을 열심히 공부해야 하는지 조금 더 자세하게 다룰 예정이다.
앞으로의 포스팅 계획에는 실무에서 쓰이지 않는, 시간 복잡도 $O(n^2)$을 가지는 알고리즘들도 다룰 예정이고, 구현한 코드를 정리할 계획이다. 이 과정에서 왜 이렇게까지 정렬 알고리즘을 배워야 하는지에 대해서 짚고 넘어가려고 한다.
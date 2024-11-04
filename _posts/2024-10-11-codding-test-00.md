---
title: "[BOJ][Silver II] 15663번: N과 M(9)"
excerpt: "전형적인 back-tracking 문제 중 하나"
categories:
  - boj
toc: true
last_modified: 2024-10-11T11:17:00
---

[문제 바로가기](https://www.acmicpc.net/problem/15663)

## 풀이 방법

전형적인 백 트래킹 문제 중 하나

문제에서 N의 자연수 중에서 M개를 고르는, $nPm$의 문제이다.
입력 수 중에 중복된 수가 있을 수 있으며 같은 수를 여러번 골라도 된다.
사전 순 출력, 중복  순열 X의 조건을 생각하자.

N개의 공이 있는 주머니에서 M개의 공을 뽑는 **순서**에 대한 경우를 모두 출력하면 되는 것이다.

## 입력 처리

처음엔 입력 값을 처리해보자.
입력 받을 것
자연수 N, M -> `int N, M`
N개의 수들 -> `vector<int> dataset;`

그리고 입력받은 값을 사전 순으로 나열해야 하므로 처리하기 편하게 정렬 알고리즘을 사용하자.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main(){
	int N, M;
	vector<int> dataset;
	
	cin >> N >> M;
	dataset.resize(N);
	
	for(int i=0; i<N; i++){
		cin >> dataset[i];
	}

	sort(dataset.begin(), dataset.end());
}
```

## 처리(탐색)
그 다음에 모든 수의 경우를 찾는 함수를 써보자.
처음에는 n개의 수를 고르고, 그 다음에 n개에서 수를 고르고, ... , 마지막에 M개 번 반복할 때까지 하는 것이다.

그러면 n개의 수를 고르는 동작이 같으므로, loop의 end point가 M개의 수를 고르는 것인 재귀함수를 짜자. 여기서 재귀함수를 쓰는 이유는 모든 경우를 반복문으로 찾는 것이 아닌, 트리 구조를 생각하며 하나하나 탐색하기 위함이다.

dataset 벡터의 경우 함수에서도 사용해야 하는데, 이를 매개변수로 받기엔 쉽지 않다.
그러므로 전역 변수로 빼자.
N과 M도 마찬가지로 계속 체크해야 하므로 전역 변수로 빼자.

Backtrack 함수
Backtrack에서는 주어진 N개의 수열에서 하나의 수를 조건에 맞게 선택할 것이다.
호출의 depth가 M만큼 되었다면, 수열에서 M개 만큼의 수를 고른 것이다.
따라서 계속 호출을 하되 M개에서 멈추는 로직을 짜도록 하자
depth를 계산해야 하므로 파라미터로 `int depth`를 넘겨주도록 하자

### 값 저장
계속해서 탐색을 하면서 track한 수를 저장해야 한 번의 탐색을 마쳤을 때 출력해야 할 수 가 무엇인지 알 것이다.
그렇다고 그 수들을 한꺼번에 저장하고 있는 것도 메모리 낭비이다. 문제에서 단순 출력을 요구하기 때문이다.

따라서 하나의 탐색을 마쳤을 때, 바로 출력하기 위해서 각각의 단계에서 선택한 수를 저장하는 벡터 `vector<int> selection`를 만들자.

## 출력
M개의 수를 고른 순간 수를 출력하자.
출력은 어떻게 해야할까?
재귀함수의 endpoint때 select 벡터의 값이 하나의 탐색 값이므로 해당 시점에서 select를 출력하자

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int N, M;
vector<int> dataset;
vector<int> selection;

void Backtrack(int depth){
	if(depth >= M){
		for(const auto& p : selection){
			cout << p << ' ';
		}
		cout << '\n';
		return;
	}

	for(int i=0; i<N; i++){
		selection[depth] = dataset[i];
		Backtrack(depth+1);
	}
}

int main(){	
	cin >> N >> M;
	dataset.resize(N);
	selection.resize(M);
	
	for(int i=0; i<N; i++){
		cin >> dataset[i];
	}

	sort(dataset.begin(), dataset.end());

	Backtrack(0);
}
```

여기서 Backtrack의 경우 탐색의 깊이인 0만 넘겨주면 된다.
데이터 자체는 전역변수 벡터에서 관리하고 있으므로 알아서 탐색을 진행 할 것이다.
처음 depth 0에서 0번 인덱스를 for문으로 하나하나 돌 것이므로 모든 경우의 수를 탐색한다.

### 예외 처리

이때 $nPm$에서 한번 고른 수를 다시 한번 고르는 경우는 없다.
노란 공이 하나밖에 없는 주머니에서 노란 공을 뽑았는데 다음 공이 또 노란색일 수는 없기 때문이다.
따라서 해당 순서에 있는 수를 골랐다고 표기하기 위해 방문을 체크하는 vector를 두자.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int N, M;
vector<int> dataset;
vector<int> selection;
vector<bool> visit;

void Backtrack(int depth){
	if(depth >= M){
		for(const auto& p : selection){
			cout << p << ' ';
		}
		cout << '\n';
		return;
	}

	for(int i=0; i<N; i++){
		if(!visit[i]){ // 아직 해당 수를 선택하지(뽑지) 않았다면
			selection[depth] = dataset[i];
			visit[i] = true; // 해당 수를 이미 선택함
			Backtrack(depth+1);
			visit[i] = false; // 다시 주머니에 수를 넣기
		}
	}
}

int main(){	
	cin >> N >> M;
	dataset.resize(N);
	selection.resize(M);
	visit.assign(N, false); // N개의 공에 false 표기. 아직 아무 공도 선택되지 않음

	for(int i=0; i<N; i++){
		cin >> dataset[i];
	}

	sort(dataset.begin(), dataset.end());
}
```

### 중복 처리
그리고 문제 조건 중에 같은 수열이 두 번 이상 출력되면 안된다.
그러나 문제의 입력 데이터 중 중복된 수가 있다.
우리는 지금 데이터 자체가 아니라 인덱스 기반으로 탐색을 하고 있으므로
모든 인덱스를 다 출력하면 중복된 데이터의 경우 중복된 수열을 출력할 수 밖에 없지?

여기서, 중복된 수를 피하기 위한 꼼수(?)를 하나 써보자
데이터는 오름차순으로 정렬되어있다. 그러면 중복된 수는 분명 붙어서 출력될 것이다.
갑자기 3번 출력과 7번 출력이 같을 수는 없단 말이다.

그러면 탐색을 할 때, 이전 값과 다음 탐색할 데이터가 같은지만 비교해주면 될 것이다.
값을 저장할 `int temp = 0`을 두자. 입력 데이터는 자연수이므로 초기값은 0으로 설정하자.

그리고 각 비교마다 temp와 현재 비교하는 dataset\[i]가 같은지 판별해서 같다면(즉, 중복된다면) 탐색을 하지 않는 것으로 하면 된다.

> [!note] 
> 이렇게 조건을 기반으로 필요하지 않은 탐색을 진행하지 않는 것을 가지치기(pruning)라고 한다.
> 이는 BackTracking의 중요 기법 중 하나이다.
> 
> 탐색을 하면서 조건에 맞지 않는 탐색의 경우 depth 자체를 파고드는 탐색을 끊어서
> 필요하지 않은 값을 탐색하는 경우를 사전에 차단하는 것이다.
> 이를 통해 빠른 탐색 및 불필요한 데이터 및 연산을 막을 수 있다.
> 
> 이전의 방문을 했는지, 안했는지에 대해서 판단하는 `!visit[i]` 로직 또한 프루닝이다.


```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int N, M;
vector<int> dataset;
vector<int> selection;
vector<bool> visit;

void Backtrack(int depth){
	if(depth >= M){
		for(const auto& p : selection){
			cout << p << ' ';
		}
		cout << '\n';
		return;
	}

	int temp = 0;
	for(int i=0; i<N; i++){
		if(!visit[i] && temp != dataset[i]){ // 이전 데이터와 중복되지 않는다면
			temp = dataset[i]; // 다음 비교를 위해 temp 최신화
			selection[depth] = dataset[i];
			visit[i] = true;
			Backtrack(depth+1);
			visit[i] = false;
		}
	}
}

int main(){	
    ios_base::sync_with_stdio(false); // 빠른 입출력
    cout.tie(0); // 출력의 경우만 많으므로 cout만 tie를 처리해주었다.
    
	cin >> N >> M;
	dataset.resize(N);
	selection.resize(M);
	visit.assign(N, false);

	for(int i=0; i<N; i++){
		cin >> dataset[i];
	}

	sort(dataset.begin(), dataset.end());
	Backtrack(0);
}
```

(빠른 출력을 위해 뒤늦게 동기화 처리를 해주었다.)

여기서 왜? int temp를 함수 내에 지역 변수로 선언했는가?
계속 데이터를 체크하기 위해 밖에 빼야 할 것 같은데... 하는 사람도 있을 것이다.

그러나 컴퓨터구조를 공부하다보면 왜 temp 변수를 지역 변수로 선언했는지 알 것이다.
먼저 재귀함수가 **Stack 메모리**에 어떻게 들어가는지 보자.

![[IMG-20241011095149599.png]]

지역 변수들은 각 **함수**별로 스택에 저장된다. 같은 식별자 temp를 사용하지만, 메모리 상으로는 별개의 변수인 것이다.
depth 0의 temp, depth 1의 temp는 각각 다른 함수이므로 식별자 또한 다른 것이다. 그리고 depth를 돌다가 다시 반환되서 돌아오면 이전 depth의 temp는 이전 값을 메모리에 저장한 채로 있는 것이다.

`temp` 변수가 `dataset[i]` 값을 저장한 이유는 같은 depth에서 중복 수열을 만들지 않기 위해서이다. 
아까 언급한 대로 `dataset` 배열을 오름차순으로 정렬했기 때문에 같은 값을 갖는 수가 있다면 바로 옆에 위치할 것이다. `temp`값이 이전 배열의 마지막 값을 저장하고 있으므로 같은 depth에서 체크하면 중복 수열을 만들지 않게 되는 것이다.

그리고 해당 로직을 통해 중복을 다 체크하고 이전 depth로 돌아왔을 때, 해당 depth에서는 별개의 함수 스택에 있었으므로 해당 depth의 temp값에 저장된 값을 토대로 진행하는 것이다.



![[IMG-20241010115251271.png]]
백트래킹 및 재귀함수를 활용한 프루닝에 대해 제대로 이해할 수 있는 문제였다.


# 기타 주의 점
이번 문제를 풀면서 처음에 선택하는 변수의 이름을 지을 때 `cache`, `select` 등등 이름을 지었다.

cache는 문제의 정의에 맞지 않고,
select는 이미 정의된 예악어라 컴파일 에러를 내보냈다.
(소켓통신 관련 네트워크 함수라고 한다)

지금 생각에도 뭔가 더 좋은 이름이 있을 것 같은데,,, 하면서 글을 쓰고 있다.    
(좋은 이름이 있다면 추천 바랍니다 ^-^;;)

변수 이름을 정할 때도 전반적인 전산학 및 언어에 대한 지식이 있어야 함을 다시 상기시켰던 에러들이었다.
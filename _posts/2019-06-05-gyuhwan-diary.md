---
layout: post
title:  "0605 Diary"
date:   2019-06-05 00:00:00 0000
author : "Gyuhwan Kim"
categories: Diary Network-flow GoF Codeforce
use_math: true
---

### 공부

**기상 시간** : 오전 7시 55분

**공부 시간** :  11시간 58분 33초



## Baekjoon Online Judge

[1102 발전소](<https://www.acmicpc.net/problem/1102>)

[1162 도로포장](<https://www.acmicpc.net/problem/1162>)

[2316 도시 왕복하기](<https://www.acmicpc.net/problem/2316>)

[10937 두부 모판 자르기](<https://www.acmicpc.net/problem/10937>)

[11495 격자 0 만들기](<https://www.acmicpc.net/problem/11495>)



## GoF의 디자인 패턴 

25 - 32p [ Ch 1.1 ~ 1.2 ]

객체지향 소프트웨어 설계란 **재사용할 수 있는 객체지향 소프트웨어를 만드는 것**

1) 올바른 크기의 클래스와 클래스의 인터페이스를 정의

2) 클래스간의 상속 정의, 클래스간의 관계 설정

3) 당장 갖고 있는 문제뿐만 아니라 나중에 생길 수 있는 문제나 추가된 요구사항도 수용할 수 있도록 설계

4) 재설계 없이 재사용 가능해야하며, 최소한의 수정으로 다시 사용할 수 있어야함



*** 디자인 패턴** : 재사용이 가능한 설계를 선택하고, 재사용을 방해하는 설계를 배제하도록 도와줌

#### 패턴의 요소 

1) **이름** : 한두 단어로 설계 문제와 해법을 서술

2) **문제** : 언제 패턴을 사용하는가와 해결할 문제와 그 배경을 설명

3) **해법** : 설계를 구성하는 요소들과 요소들간의 관계, 책임, 협력 관계 서술

4) **결과** : 패턴을 적용해서 얻는 결과와 장단점을 서술

> 디자인 패턴은  특정한 전후 관계에서 일반적 설계 문제를 해결하기 위해 상호교류하는 수정 가능한 객체와 클래스들에 대한 설명



#### MVC를 사용한 디자인 패턴

사용자 인터페이스를 만들 때 MVC(Model, View, Controller)의 세 가지 클래스를 사용함.

1) 모델 (Model) : 응용프로그램 객체

2) 뷰 (View) : 스크린에 모델을 디스플레이 하는 방법

3) 컨트롤러 (Controller) : 사용자 인터페이스가 사용자 입력에 반응하는 방법	



#### MVC의 특징



1) MVC는 뷰와 모델 간에 등록/통지(subscribe/notify) 프로토콜을 만들어 종속성을 없앰.

뷰의 외형은 반드시 모델의 상태를 반영하도록 보장해야 한다. 따라서 모델의 데이터가 변경될 때마다 모델은 자신과 관련된 뷰에 알려주고, 이 통보에 따라서 뷰는 스스로 외형을 변형해야 한다.

이러한 설계방법은 더 일반적인 문제에도 적용이 가능하다. 즉, 한 객체에서 일어난 변경을 다른 객체들에 반영하도록 별도의 객체를 두어 변경이 일어난 객체들은 변경 반영이 필요한 다른 객체들을 알 필요가 없게끔 분리하는 것이다. 이런 설계를 일반화한 것이 **감시자 패턴**이다.



2) MVC는 뷰를 중첩시킬 수 있음.

버튼의 제어판은 여러 개의 버튼을 포함하는 복잡한 뷰로 구현할 수 있음. MVC는 CompositeView 클래스를 이용하여 중첩된 뷰를 지원하고, 이는 View클래스의 서브클래스이므로 CompositeView를 View객체처럼 다룰 수 있다. 이는 복합 뷰를 마치 하나의 단일 뷰처럼 사용하려는 설계이다. 이런 설계를 일반화한 것이 **복합체 패턴**이다.



3) MVC는 시각적 표현 방법의 변경 없이 사용자 입력에 대한 뷰의 반응 방법을 변경할 수 있음.

명령 키 대신에 팝업 메뉴를 사용할 수도 있고, 키보드를 이용하도록 변경할 수 있다. MVC에서는 Controller를 이용하여 반응 방법을 캡슐화한다. 컨트롤러의 클래스 계층을 통해 기존 방식과 다른 입력 방식을 가진 새로운 컨트롤러를 정의하는 것이다. 이를 위해서 View 클래스가 Controller의 서브 클래스의 인스턴스를 사용하면, 다른 입력방식을 갖기 위해서 해당 컨트롤러 인스턴스로 대체하면 된다.

이러한 뷰와 컨트롤러 사이의 관계는 **전략 패턴**의 한 예이다. 전략 패턴은 알고리즘을 표현하는 객체로 정적 또는 동적으로 알고리즘을 대체하고자 할 때 유용한 방법이다.

※ MVC 패턴은 감시자, 복합체, 전략 패턴 외에 다른 패턴도 사용한다. 예를 들어 팩토리 메서드 패턴을 이용해서 뷰에 대한 기본 컨트롤러 클래스를 지정한다던지, 장식자 패턴을 이용해서 뷰에 스크롤을 추가한다던지 등등이 있다. 하지만 MVC에서 뷰와 컨트롤러 관계를 맺어주는데 주로 사용하는 패턴은 감시자, 복합체, 전략 패턴이다.



### HopCroft-Karp Algorithm

[kks227님의 블로그](https://m.blog.naver.com/kks227/220816033373)를 토대로 작성하였습니다.

간선 용량이 모두 1인 이분 그래프에서만 동작하는 최대 유량 알고리즘

이 알고리즘의 시간복잡도는 
$$
O(E \sqrt V)
$$
 이다. 이 때,  완전 이분 그래프에서 
$$
E = O(V^2)
$$
이므로 최악의 시간복잡도는 
$$
O(V^{2.5})
$$
알고리즘에 대한 자세한 설명은 위의 블로그에서 확인할 수 있다.

이 포스팅에서는 HopCroft-Karf에 대한 기본 코드와 관련된 문제를 다룬다.

#### HopCroft-Karp Basic Code (Class)

```cpp
class HopCroft {
private:
	int N;
	vector<bool> used;
	vector<int> A, B, level;
	vector< vector<int> > edges;
public:
	void init(int N) {
		this->N = N;
		used.assign(N, false);
		A.assign(N , -1);
		B.assign(N , -1);
		level.assign(N, -1);
		edges.assign(N, vector<int>());
	}

	void addEdge(int src, int dst) {
		edges[src].push_back(dst);
	}

	void makeLevelGraph() {
		queue<int> q;
		for (int i = 0; i < N; i++) {
			if (!used[i]) {
				level[i] = 0;
				q.push(i);
			}
			else {
				level[i] = -1;
			}
		}
		while (!q.empty()) {
			int here = q.front();
			q.pop();
			for (int there : edges[here]) {
				int matchedA = B[there];
				if (matchedA != -1 && level[matchedA] == -1) {
					level[matchedA] = level[here] + 1;
					q.push(matchedA);
				}
			}
		}
	}
	bool dfs(int here) {
		for (int there : edges[here]) {
			int matchedA = B[there];
			if (matchedA == -1 || level[matchedA] == level[here] + 1 && dfs(matchedA)) {
				used[here] = true;
				A[here] = there;
				B[there] = here;
				return true;
			}
		}
		return false;
	}

	int execute() {
		int match = 0;
		while (1) {
			makeLevelGraph();
			int flow = 0;
			for (int i = 0; i < N; i++) {
				if (!used[i] && dfs(i)) flow++;
			}
			if (flow == 0) break;

			match += flow;
		}
		return match;
	}
};

```

#### Main Code

```cpp
int main() {
/*	input();  문제에 따라 달라지는 부분 => 이분 그래프를 만들어주는 부분이다. */
	int ans = hf.execute();
	printf("%d\n", ans);
	return 0;
}
```



#### * 관련 문제

[BOJ 3736 System Engineer](icpc.me/3736) 

[BOJ 13166 범죄 파티](icpc.me/13166)



## Codeforce

#### Educational Codeforces Round 66 (Rated for Div. 2)  

시작 시간 : 2019/06/05 23:35 (2h)

1690 =>  나락

#### [A. From Hero to Zero](https://codeforces.com/contest/1175/problem/A)

각 테스트케이스마다 n, k가 주어지고, 2가지 연산이 있다.

1)  n이 k로 나눠질경우 n을 k로 나눈다.

2) 나눠지지 않을 경우 1만큼 감소시킨다.

**이 때,  0까지 가는데 몇 번의 연산이 걸리는지에 대한 문제이다. 읽고 바로 코딩한 나머지, 시간 초과가 걸리는 경우를 생각하지 못해서 패널티를 받았다.**

#### [B. Catch Overflow!](https://codeforces.com/contest/1175/problem/B)

l개의 쿼리가 주어진다. 각 쿼리는 아래 3개의 연산 중 하나를 수행한다. (l은 10,000이하의 자연수)

- for n => for loop  $ (1 \leq n \leq 100) $

- end => 바로 위의 for문을 n번 실행하고 종료한다.

- add => 증가

쿼리로 이루어진 프로그램의 수행 결과 값을 출력한다. 단 이 때, 수행 중 
$$
2^{32}-1
$$
값을 넘어가게 되면 OVERFLOW!!!를 출력한다.

**"for n"이 나올 경우 vector에 n값을 넣고, "add"할 때 vector값들을 모두 곱한 값을 더하게 하였다. 곱하는 도중 범위를 넘어가게 되면 OverFlow Flag를 True, 그리고 add 과정 중에 해당 값을 넘어가게 되면 Flag를 True로 바꾸었다. Flag가 True일 경우 입력만 받도록 하였다.**

**(위 문제에서 n=1인 for문이 여러가지 나오는 경우 TLE가 나오는 함정이 있었다. 이를 고려하지 못해 나락갔다...)**

#### [C. Electrification](https://codeforces.com/contest/1175/problem/C)

테스트케이스 T가 주어진다.

각 테스트케이스의 첫 번째 줄에 n, k가 주어진다.

각 테스트케이스의 두 번째 줄에 n개의 자연수 $ a_i $가 주어진다.
$$
f_k(x)
$$
라는 함수는 다음과 같이 수행된다.

1) 
$$
d_i =|a_i -x|
$$
로 리스트를 구성한다.

2) 해당 리스트를 오름차순으로 정렬한다.

3) 
$$
d_{k+1}
$$
값을 반환한다.

이 때,  $ f_k(x) $ 값이 최소 값이 되는 x값을 구하는 것이 문제에서 요구하는 바이다.

**처음 접근한 방법은 삼분 탐색이었고, 잘못된 접근 방법이었으나 대회 중에는 무엇이 잘못되었는지 파악을 못했다. 그래프를 그려보았다면 삼분 탐색으로 접근할 수 없다는 것을 판단했을텐데 그러지 못했다 ㅠㅠ 대회가 끝나고 곰곰히 생각해보니 k+1번째 수가 최소값이 되려면 배열에 있는 n개의 수를 k+1개로 묶은 subArray에서 중간값이 후보군이 된다는 아이디어가 떠올랐고, 이를 구현한 결과 Accepted 받았다.**

#### [D. Array Splitting](<https://codeforces.com/contest/1175/problem/D>)

n개의 원소를 가진 배열이 주어지고, 이를 k개의 블럭으로 나눠야한다. 이 때, 블럭에는 최소 1개 이상의 원소가 들어가야 한다.  이 때 구하는 값은 **각 원소의 값 * 각 원소의 블록 번호** (번호는 1부터 시작)의 합이며 이 값이 최대가 되도록 각 블록에 원소를 배치하는 것이었다. 블록은 왼쪽에서 오른쪽으로 1부터 k로 증가하며, 연속되는 원소로 이루어져야 한다. 예를 들면
$$
a=[1,-2,-3,4,-5,6,-7]
$$
라면 
$$
[1,-2,-3], [4,-5],[6,-7]
$$
로 나누면 합은 
$$
1*1 -2*1-3*1+4*2-5*2+6*3-7*3 = -9
$$
가 된다.

**대회가 끝난 후 친구에게 풀이법을 듣고 풀었다. 풀이법은 간단하다. 위의 예제처럼 나눠질 경우**
$$
[1,-2,-3,4,-5,6,-7] + [4,-5,6,-7]+[6,-7]
$$
**로 나타낼 수 있다. 가중치가 각각 1, 2, 3이기 때문에 3번째 블록은 3번 더해지고, 2번째 블록은 2번 더해지고 1번째 블록은 1번 더해진다는 뜻**

**그래서,** 
$$
psum[i]= psum[i+1]+arr[i]
$$
**와 같은 식으로 Partial Sum을 구성하고,  먼저 ans에 psum[1]을 더해준다. 그리고 psum[2]~ psum[n]을 vector에 넣고 내림차순으로 정렬한 후 가장 큰 값부터 차례대로 k-1개를 더해주면 정답이 나오게 된다.**


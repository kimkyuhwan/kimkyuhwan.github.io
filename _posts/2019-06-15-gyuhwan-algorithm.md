---
layout: post
title:  "문자열 알고리즘"
date:   2019-06-15 00:00:00 0000
author : "Gyuhwan Kim"
categories: Algorithm KMP Trie Aho-Corasick
use_math: true
---

# 알고리즘

## KMP 알고리즘

KMP 알고리즘은 문자열 A에서 문자열 B를 발견하는 알고리즘이다.

문자열 A에서 문자열 B를 발견하는 알고리즘은 가장 간단한 방법으로는 문자열 A의 문자를 앞에서부터 탐색하고, A의 i번째 문자와 문자열 B의 첫 번째 문자가 같을때 나머지 문자들도 일치하는지 확인한다. 확인하다가 일치하지 않으면 다시 i+1번째부터 이 과정을 반복하기 때문에,  이는 A의 문자열 길이를 N이라고 하고, B의 문자열 길이를 M이라고 했을때 O(NM)이라는 시간복잡도를 가진다.

KMP는 i번째 문자열에서 비교하다가 j번째에서 일치하지 않으면 j+1번째 부분부터 다시 확인하기 때문에 O(N + M)의 시간복잡도를 갖는다.

이 때, 문자열이 일치하지 않는다고 해서 j+1번째 부분에서 다시 B의 첫 번째 문자열부터 비교한다면 A에 B가 존재하는데도 불구하고 못 찾는 경우가 발생할 수 있다. 따라서, 일치하지 않을때 그동안 일치했던 부분을 이용하여 이를 해결한다.

#### KMP.cpp

```cpp
string N, M;
vector<int> fail;

void makeFailure() {
	fail.assign(M.size(),0);
	for (int i = 1, j = 0; i < fail.size(); i++) {
		int prev = fail[i - 1];
		while (j > 0 && M[i] != M[j]) j = fail[j - 1];
		if (M[i] == M[j]) fail[i] = ++j;
	}
}

vector<int> executeKMP(string &N, string &M) {
	vector<int> kmpIdx;
	makeFailure();
	for (int i = 0, j=0; i < N.size(); i++) {
		while (j > 0 && N[i] != M[j]) j = fail[j - 1];
		if (N[i] == M[j]) {
			j++;
			if (j == M.size()) {
				kmpIdx.push_back(i-M.size()+2);
				j = fail[j-1];
			}
		}
		else {
			j = fail[j];
		}
	}
	return kmpIdx;
}
```



## 트라이 (Trie)

트라이는 여러 문자열을 저장하는 알고리즘으로 검색어 자동완성 등에 사용될 수 있는 알고리즘이다.  저장된 문자열을 찾는 경우나 삽입하는데 걸리는 시간복잡도는 O(N)으로 문자열의 길이 시간 만큼 소모되지만,  공간복잡도가 문제가 된다. 따라서 보통 문제에 입력 데이터 개수가 최대 X를 넘지 않는다고 설명되어 있다. 

 해당 포스팅에서는 트라이를 정적으로 저장하는 방법과 동적으로 저장하는 방법에 대해서 설명한다. 시간복잡도 상으로는 차이가 없지만, 실제 동작 시간은 확인해본 결과 정적으로 구현하는 방법이 3~4배 정도 빠르다. 기본 예제 문제로는 [BOJ 5052](icpc.me/5052)가 있다. 코드는 다음과 같다.

#### Static Trie

```cpp
#define NEXT_MAX 10
#define CHAR_MAX 100000

class Trie {
private:
	int cnt;
	int next[CHAR_MAX+1][NEXT_MAX];
	bool isNode[CHAR_MAX+1];
	bool hasChild[CHAR_MAX+1];

public:
	Trie() {
		cnt = 1;
		memset(next, 0, sizeof(next));
		memset(isNode, 0, sizeof(isNode));
		memset(hasChild, 0, sizeof(hasChild));
	}
	bool insert(char *key, int node=0) {
		if (*key == '\0') {
			isNode[node] = true;
			return !hasChild[node];
		}
		else {
			int nextIndex = *key - '0';
			if (!next[node][nextIndex]) {
				next[node][nextIndex] = cnt++;
			}
			hasChild[node] = true;
			return !isNode[node] && insert(key + 1,next[node][nextIndex]);
		}
	}
};
```

#### Dynamic Trie

```cpp
#define NEXT_MAX 10

class Trie {
private:
	Trie *next[NEXT_MAX];
	bool isNode;
	bool hasChild;

public:
	Trie() {
		fill(next, next + NEXT_MAX, nullptr);
		isNode = hasChild = false;
	}
	~Trie() {
		for (int i = 0; i < NEXT_MAX; i++) {
			if (next[i]) delete next[i];
		}
	}
	bool insert(char *key) {
		if (*key == '\0') {
			isNode = true;
			return !hasChild;
		}
		else {
			int nextIndex = *key - '0';
			if (!next[nextIndex]) {
				next[nextIndex] = new Trie();
			}
			hasChild = true;
			return !isNode && next[nextIndex]->insert(key + 1);
		}
	}
};
```

#### 5052.cpp

```cpp
Trie *trie;

int main() {
	scanf("%d", &TC);
	for (int i = 0; i < TC; i++) {
		scanf("%d", &N);
		bool result = true;
		trie = new Trie();
		for (int j = 0; j < N; j++) {
			scanf(" %s", str);
			if (result) {
				result = trie->insert(str);
			}
		}
		if (result) puts("YES");
		else puts("NO");
		delete trie;
	}
	return 0;
}
```



## 아호코라식(Aho-Corasick)

아호코라식 알고리즘은 기본적으로 문자열 A에서 문자열 배열 B의 원소들이 존재하는지 찾는데 쓰인다.  KMP의 경우 문자열 A와 다른 1개의 문자열 B를 비교하는 알고리즘이라면, 이는 여러 개와 한꺼번에 비교하는 알고리즘이다.  해당 알고리즘에는 Trie 자료구조가 사용되며, KMP의 Failure 개념이 사용된다. 

알고리즘의 동작과정은 다음과 같다.

1) 먼저 문자열 배열 B에 있는 문자열들을 Trie에 삽입한다.

2) BFS를 이용하여 Failure 배열을 채운다. (fail은 일치하지 않을 경우 이동하는 노드의 위치를 담게된다.)

3) 문제에 맞게 이용한다.

#### AhoClass.cpp

```cpp
const int root = 0;
const int NEXT_MAX = 26;
const int NODE_MAX = 100000;

class Aho {
private:
	int cnt;
	int next[NODE_MAX + 1][NEXT_MAX];
	int fail[NODE_MAX + 1];
	bool output[NODE_MAX + 1];
	
public:
	Aho() {
		cnt = 1;
		memset(next, 0, sizeof(next));
		memset(output, 0, sizeof(output));
		memset(fail, 0, sizeof(fail));
	}

	bool canGo(int node, int nextIndex) {
		return next[node][nextIndex] != 0;
	}

	void insert(char *str, int node = root) {
		if (*str == '\0') {
			output[node] = true;
		}
		else {
			int nextIndex = *str - 'a';
			if (!canGo(node, nextIndex)) {
				next[node][nextIndex] = cnt++;
			}
			insert(str + 1, next[node][nextIndex]);
		}
	}

	void makeFailure() {
		queue<int> q;
		q.push(root);
		fail[root] = root;
		while (!q.empty()) {
			int node = q.front();
			q.pop();
			for (int i = 0; i < NEXT_MAX; i++) {
				int nextNode = next[node][i];
				if (!nextNode) continue;
				if (node == root) {
					fail[nextNode] = root;
				}
				else {
					int destNode = fail[node];
					while (destNode != root && !next[destNode][i]) {
						destNode = fail[destNode];
					}
					if (next[destNode][i]) {
						destNode = next[destNode][i];
						fail[nextNode] = destNode;
					}
				}
				if (output[fail[nextNode]]) {
					output[nextNode] = true;
				}
				q.push(nextNode);
			}
		}
	}

	bool isExist(char *str) {
		int current = root;
		for (int j = 0; str[j]; j++) {
			int nextIndex = str[j] - 'a';
			while (current != root && !canGo(current, nextIndex)) {
				current = fail[current];
			}
			if (canGo(current, nextIndex)) {
				current = next[current][nextIndex];
			}
			if (output[current]) {
				return true;
			}
		}
		return false;
	}
};
```

#### 9250_main.cpp

```cpp
Aho aho;
int N,M;
char str[10010];
int main() {
	scanf("%d", &N);
	for (int i = 0; i < N; i++) {
		scanf(" %s", str);
		aho.insert(str);
	}
	aho.makeFailure();
	scanf("%d", &M);
	for (int i = 0; i < M; i++) {
		scanf(" %s", str);
		bool ans = aho.isExist(str);
		if (ans) puts("YES");
		else puts("NO");
	}
}
```



### TEST

[문제] 길이 $n$의 문자열 $S$가 주어진다. 이 때, $0\le i<n$인 $i$에 대해 $p_i$를 $S[0 .. k] = S[i .. i+k]$인 가장 큰 $k$의 값으로 정의한다. $p_i$를 모든 $i$에 대해 구하라.



이 문제 또한 문자열이 주어질 때, 각 $i$에 대해 $i$번째부터 시작하는 부분 문자열의 접두사 $S[i..i+k]$와 전체 문자열의 접두사 $S[0..k]$가 같은 최대 길이를 구하는 문제가 된다. $k=0$일 때도 $S[i] \neq S[0]$이라면, $p_i = -1$로 하자.



이 역시 Manacher 알고리즘과 거의 같은 방식으로 작동한다. 여기에도 이때까지 구한 $i+p[i]$, 즉 구한 문자열의 접두사의 오른쪽 끝의 위치 값들 중 가장 큰 값을 놓고 초기값을 정하는 과정을 사용한다. 간단히 요약하면 다음과 같다.



0. $i$를 $0..n-1$까지 진행한다. 아래는 $p[0..i-1]$이 모두 구해졌을 때 $p[i]$를 구하는 과정이다.

1. $r = \max_{j=0..i-1}(j+p[j])$라 하고, 이 때의 $j$를 $l$이라 하자. $S[l..r]$은 같은 길이의 접두사 $S[0..r-l]$과 같다.

2. $p[i]$의 초기값을 결정한다.

 2-1. $i \le r$이라면, $p[i] \leftarrow min(r-i, p[i-l])$

 2-2. $i > r$이라면, $p[i] \leftarrow -1$

3. $S[i+p[i]+1] = S[p[i]+1]$이 성립하지 않을 때까지 $p[i]$를 계속 1씩 증가시킨다.



1
2
3
4
5
6
7
8
9
10
void prefix(string s, int *p) {
    int n = s.size(), l = -1, r = -1;

    for (int i=1; i<n; i++) {
        if (i<=r) p[i] = min(r-i, p[i-l]);
        while (i+p[i]+1<n and s[i+p[i]+1] == s[p[i]+1]) p[i]++;
        if (r < i+p[i]) r = i+p[i], l = i;
    }
}

Colored by Color Scripter
cs


3번에서 $p[i]$를 증가할 때마다 $r$이 증가하기 때문에 도합 $O(n)$의 시간 복잡도로 돌아가리란 것을 알 수 있다.



출처 및 참고: #





​                 



모든 좋고 재미있는 알고리즘이 그렇듯이, 이 알고리즘도 이해하기 쉽고 간단한데 반해 온갖 신기한 응용을 할 수 있다. 추후 두 알고리즘을 이용하는 신기한 문제들을 업로드할 것이다.
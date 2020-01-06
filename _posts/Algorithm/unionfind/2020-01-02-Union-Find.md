---
title: Union Find 알고리즘이란
# 오버레이 되는 이미지 및 글
excerpt: 특정 집합에 어떠한 원소가 있는지 Find하는 시간 복잡도를 줄일수 있는 알고리즘


tags: [ProblemSolving, unionfind, disjoint-set]
categories: [Algorithm,]
---

# Disjoint-Set은 뭐야?
서로 중복되지 않는 부분 집합들로 나눠진 원소에 대해 정보를 저장하고 조작하는 자료구조이다. 즉 서로소 집합 자료구조이다. 이러한 자료구조를 표현하기 위해 Union-Find라는 알고리즘이 사용된다.

# Union-Find란?
가장 작은 값을 루트로 두고 나머지 집합에 속한 원소들의 부모를 루트로 둘 경우 해당 원소가 어느 집합에 속해 있는지 파악하기가 쉽다.

# 설명
워낙 간단해서 생략한다 소스로 이해하는게 낫다.

# 소스코드
```
#include <iostream>
using namespace std;

int parent[10];

int find(int num) {
	if (parent[num] == num)return num;
	return parent[num] = find(parent[num]);
}

void merge(int u, int v) {
	u = find(u);
	v = find(v);
	parent[u] = v;
	/*
	// 작은 값을 부모로 두는 경우
	if (u < v)
		parent[v] = u;
	else
		parent[u] = v;
	*/
}

int main() {
	int relation[5][2] = {
		{0, 1}, {1, 3}, {3, 4}, {2, 5}, {8, 9}
	};
	
	// 처음엔 자기 자신을 가르킴
	for (int i = 0; i < 10; ++i)
		parent[i] = i;
	for (int i = 0; i < 5; ++i)
		merge(relation[i][0], relation[i][1]);
	
	if (find(1) == find(2)) {
		cout << "1과 2는 같은 그룹\n";
	}
	else {
		cout << "1과 2는 다른 그룹\n";
	}

	if (find(0) == find(4)) {
		cout << "0과 4는 같은 그룹\n";
	}
	else {
		cout << "0과 4는 다른 그룹\n";
	}
}
```
# 참고자료
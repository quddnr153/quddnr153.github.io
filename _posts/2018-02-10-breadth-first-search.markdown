---
layout: post
title:  "Breadth First Search (BST)"
date:   2018-02-10 21:00:00 +0900
tags: algorithm bfs breadthfirstsearch graph
---
# 1. Introduction
* * *
Breadth First Search (너비 우선 탐색) 은 그래프 탐색 알고리즘 중 하나로 (깊이 우선 탐색도 있음), Shortest path algorithm, minimum spanning tree 에서 사용하는 알고리즘 이다.

이름에서 알 수 있듯이, 알고리즘의 과정은 시작점에서 가장 가까운 정점부터 순서대로 방문하는 탐색 알고리즘이다.

너비 우선 탐색은 각 정점을 방문할 때마다 모든 인접 정점들을 검사하고, 이중 처음 보는 정점을 발견하면 이 정점을 방문 예정이라고 기록해 둔뒤, 별도의 위치에 저장한다 (일반적으로 queue 라는 자료구조에 저장).

이접한 정점을 모두 검사하고 나면, 지금까지 저장한 목록에서 다음 정점을 꺼내서 방문한다. 따라서 너비 우선 탐색의 방문 순서는 정점의 목록에서 어떤 정점을 먼저 꺼내는지에 의해 결정된다.

BST 의 상태는 아래와 같다:
1. 아직 발견되지 않은 상태
2. 발견되었지만 아직 방문되지는 않은 상태 (이 상태에 있는 정점들의 목록은 큐에 저장되어 있다.)
3. 방문된 상태

* * *
# 2. Complexity
* * *

모든 정점을 한 번씩 방문, 정점을 방문할 때마다 인접한 모든 간선을 검사.

따라서, 인접 리스트로 구현된 경우에는
```
 O(|V|+|E|)
```
그리고 인접 행렬로 구현했을 경우
```
O(|V|^2)
```
의 시간 복잡도를 가진다.

* * *
# 3. Usage
* * *

BST 는 대개 하나의 용도인 최단 경로 문제를 푸는 것에 사용 된다.

* * *
# 4. Example
* * *

[토마토](https://www.acmicpc.net/problem/7569)

[Source code](://github.com/quddnr153/acmicpc/blob/master/bfs/Bfs7569.java)


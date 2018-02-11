---
layout: post
title:  "Traveling salesman problem"
date:   2018-02-10 20:00:00 +0900
tags: tsp algorithm dp dynamicprogramming bitmask
---
# 1. Introduction
* * *
Traveling salesman problem (TSP) 는 Combinatorial Optimization 에 속하는 문제이다.

여러 도시가 있고, 한 도시에서 다른 도시로 가는 가중치를 알고 있을 때, 각 도시를 한번만 방문하고 출발한 도시로 되돌아 오는데 가장 비용이 적게드는 경로를 구하는 문제이다.

일반적으로 x 도시 에서 y 까지 의 여행 비용은 y 에서 x 까지 비용과 같다는 의미로 여행 비용은 대칭적이다.


가장 기본적인 접근으로 완전 탐색을 생각해보자.

N 개의 도시가 있다고 생각하면,

출발 도시를 선택하고 다음 도시를 선택하는 가지수는 (N - 1) 가지가 있다.

그 다음으론 (N - 2) ....

총 경로의 수는 (N - 1)! 가 된다.

Factorial 이라는 수학 용어를 배웠을 것이다.

이는 지수승 (x^n) 보다 빠르게 증가하는 수로 N 이 12 만 되어도 60 억을 넘어가는 숫자가 된다.

컴퓨터 (대충 1억 번 연산을 1초가 걸린다 해도 60초가 걸리는 일이다...)의 연산으로도 숫자가 커지면 계산할 수 없는 정도의 크기가 된다.

하지만, 우리에겐 여러 접근 방법이 있다.

1. dynamic programming
2. bit mask
3. Minumum spanning tree heuristic

과 같은 방법을 사용할 수 있다.

위와 같은 방법은 포스팅을 각각 할 예정이고, acmicpc 사이트에서 예제문제를 풀어보도록 하겠다.

* * *
# 2. Example
* * *

[외판원 순회](https://www.acmicpc.net/problem/2098)

[Source code](https://github.com/quddnr153/acmicpc/blob/master/tsp/2098.cpp)


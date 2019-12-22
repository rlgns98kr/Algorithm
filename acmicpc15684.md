# 사다리 조작

https://www.acmicpc.net/problem/15684

## 개요

H*N 의 Map에 사다리를 그리는 문제.  i열에서 출발하였다면 i열에 도착하여야 한다고 하였을 때 사다리 그리는 횟수의 최소를 구하는 문제. 3을 넘어가면 -1출력.(URL 참조)

## 분석

### 시간 복잡도

3을 넘어가면 -1출력이라는 조건이 없었다면 2^H*N의 경우의 수(사다리를 그리거나 안그리거나)를 따져줘야 한다.

하지만 3개의 사다리만 선택하면 되므로 H*N Combination 3 만큼의 경우의 수 * H*N(검사) 의 경우의 수만 따져주면 된다. 이 경우에 시간복잡도는 약 10억정도로 시간 내에 출력가능하다. 

현재 2회의 바꾸는 횟수가 증명되었다면 3회째는 검사할 필요가 없으므로 이를 가지치기해주면 시간을 더욱 줄일 수 있다.

### 공간 복잡도

H\*N*4 + a bytes

## 알고리즘

1. dfs(int step, int r, int c)
   1. 현재 맵이 문제의 조건을 충족하는 지 검사(i에서 출발하여 i로 도착하는 지)
      1. 충족한다면 min값 갱신 후 return;
   2. (i, j) 를 (i!=r || j>c)만 (r, j)~(h,n) 탐색한다.
      1. 양 옆과 자기자신에 사다리가 그려지지 않았고 step+1<min 일 때만 재귀호출
         1. map\[i][j]=1
         2. dfs(step+1, i, j)
         3. map\[i][j]=0
2. min이 초기값일 경우에는 -1, 아니면 min을 출력.

## 기타

바꾼 열의 좌우의 출발지만 검사한다면 시간을 더욱 줄일 수 있다고 생각하였지만 다른 출발지에서 바꾼 열로 가는 경우 때문에 모두 검사해야한다는 것을 알게 되었다.

## 소스

```c
#include <stdio.h>

int map[40][20], min = 4, n, h;

int chkline(int r, int c)
{
	if (map[r][c - 1] == 1 || map[r][c + 1] == 1 || map[r][c]==1) return 0;
	return 1;
}

int chk()
{
	int i, j, e;
	for (i = 1; i <= n; i++)
	{
		e = i;
		for (j = 1; j <= h; j++)
		{
			if (map[j][e] == 1) e += 1;
			else if (map[j][e - 1] == 1) e -= 1;
		}
		if (e != i) return 0;
	}
	return 1;
}

void dfs(int st, int r, int c)
{
	int i, j;
	if (chk() == 1)
	{
		if (min > st) min = st;
		return;
	}

	for (i = r; i <= h; i++)
	{
		for (j = 1; j <= n; j++)
		{
			if ((i!=r||j>c) && chkline(i, j) == 1 && min>st+1)
			{
				map[i][j] = 1;
				dfs(st + 1, i, j);
				map[i][j] = 0;
			}
		}
	}
}

int main()
{
	int m, i, x, y;
	scanf("%d%d%d", &n, &m, &h);
	for (i = 1; i <= m; i++)
	{
		scanf("%d %d", &x, &y);
		map[x][y] = 1;
	}

	dfs(0, 1, 0);

	if (min == 4) min = -1;
	printf("%d", min);
}
```


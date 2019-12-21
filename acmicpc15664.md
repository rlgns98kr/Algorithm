# N과 M(10)

https://www.acmicpc.net/problem/15664

## 개요

N(1<=N<=8)개의 자연수 중 M(1<=M<=8)개를 고른 비내림차순(오름차순이거나 동차순) 수열들을 출력하는 문제, 중복되는 수열을  출력해서는 안된다.

## 분석

### 시간 복잡도

최악의 경우 N개 중 M개를 뽑는 경우의 수 * N(출력) 정도의 시간 복잡도를 갖는다.

### 공간 복잡도

N*4 byte

## 알고리즘

1. 오름차순 정렬을 한다.
2. 재귀함수를 이용하여 인덱스를 (전 차수에서 뽑은 인덱스+1 ~ N) 만큼 반복한다.
3. 그 중 (전 차수에서 뽑은 인덱스+1)이거나 (현재 차수에서 뽑은 숫자와 다른 숫자) 이면 선택한다.

## 기타

## 소스

```c
#include <stdio.h>

int a[10], n, m, b[10], chk[10];

void dfs(int st, int se)
{
	int i;
	b[st] = a[se];
	if (st == m)
	{
		for (i = 1; i <= m; i++) printf("%d ", b[i]);
		printf("\n");
		return;
	}
	for (i = se+1; i <= n; i++)
	{
		if (i==se+1 || a[i]!=a[i-1])
		{
			dfs(st + 1, i);
		}
	}
}

int main()
{
	int i,j, im;
	scanf("%d%d", &n, &m);
	for (i = 1; i <= n; i++) scanf("%d", &a[i]);

	for (i = 1; i <= n; i++)
	{
		for (j = i + 1; j <= n; j++)
		{
			if (a[i] > a[j])
			{
				im = a[i];
				a[i] = a[j];
				a[j] = im;
			}
		}
	}
	dfs(0,0);
}
```


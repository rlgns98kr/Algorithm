# 캐슬 디펜스

https://www.acmicpc.net/problem/17135

## 분석

조합을 추출해내어 시뮬레이션을 진행하는 문제이다. 적들의 좌표를 저장한 뒤에 좌표를 이동시키면 쉽게 이동시킬 수 있다. 이동은 죽을 적들을 제외하고 기존 배열에 갱신하는 방식으로 한다. (삽입인덱스가 탐색인덱스보다 작거나 같으므로 바로 대입이 가능하다.)

## 알고리즘

1. dfs로 step 3만큼 도달하며 배열에 열을 저장한다.
   1. step 3에 도달했다면 시뮬레이션을 시작한다.
      1. 적들이 있다면 반복
         1. 3개의 열에 대해 죽일 적을 찾아 표시한다.
         2. 죽일 적만큼 카운트를 센다.
         3. 죽일 적과 n행을 지난 적을 제외하고 기존 배열에 복사한다.
      2. 카운트가 max보다 크다면 갱신
2. max 출력

## 소스

```c
#include <stdio.h>

int e[250][2], e2[250][3], n, m, d, en=0, g[4], max=0;

int abs(int a)
{
	if (a < 0) return a * -1;
	return a;
}

int dt(int y, int r, int c) {return abs(y - c) + abs(r - (n + 1));}

void ch()
{
	int i, j, ni=en, nni=en, gmin[4][3], cnt=0;

	for (i = 1; i <= en; i++)
	{
		e2[i][0] = e[i][0];
		e2[i][1] = e[i][1];
		e2[i][2] = 0;
	}

	while (ni > 0)
	{
		nni = ni;
		ni = 0;
		gmin[1][0] = 50, gmin[2][0] = 50, gmin[3][0] = 50;
		gmin[1][2] = 0, gmin[2][2] = 0, gmin[3][2] = 0;
		for (i = 1; i <= nni; i++)
		{
			for (j = 1; j <= 3; j++)
			{
				if (dt(g[j], e2[i][0], e2[i][1]) <= d)
				{
					if (dt(g[j], e2[i][0], e2[i][1]) < gmin[j][0])
					{
						gmin[j][0] = dt(g[j], e2[i][0], e2[i][1]);
						gmin[j][1] = e2[i][1];
						gmin[j][2] = i;
					}
					else if (dt(g[j], e2[i][0], e2[i][1]) == gmin[j][0])
					{
						if (gmin[j][1] > e2[i][1])
						{
							gmin[j][1] = e2[i][1];
							gmin[j][2] = i;
						}
					}
				}
			}
		}
		for (i = 1; i <= 3; i++) e2[gmin[i][2]][2] = 1;
		for (i = 1; i <= nni; i++)
		{
			if (e2[i][2] == 1) ++cnt;
			e2[i][0]++;
			if (e2[i][0] <= n && e2[i][2] == 0)
			{
				ni++;
				e2[ni][0] = e2[i][0];
				e2[ni][1] = e2[i][1];
				e2[ni][2] = 0;
			}
		}
	}
	if (max < cnt) max = cnt;
}

void dfs(int st)
{
	int i;
	if (st == 3)
	{
		ch();
		return;
	}
	for(i=g[st]+1; i<=m; i++)
	{
		g[st + 1] = i;
		dfs(st + 1);
	}
}

int main()
{
	int i, j, im;
	scanf("%d%d%d", &n, &m, &d);
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= m; j++)
		{
			scanf("%d", &im);
			if (im == 1)
			{
				en++;
				e[en][0] = i;
				e[en][1] = j;
			}
		}
	}
	dfs(0);
	printf("%d", max);
}
```


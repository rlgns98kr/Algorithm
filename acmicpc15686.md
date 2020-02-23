# 치킨배달

## 분석

최대 13개의 치킨집 중 M개를 골라 각 집들과의 거리를 계산하여 거리의 합의 최소를 찾을 수 있다. 각 집과 치킨집의 위치는 고정되어 있으므로 집과 치킨집과의 거리를 **미리 계산해놓고 저장**하여 이용할 수 있다. 또한 맵배열은 선언하지 않고 집배열과 치킨집배열에 좌표값을 저장하면 공간복잡도도 줄일 수 있다.

## 알고리즘

1. 모든 집들과 치킨집사이의 거리를 배열에 저장해놓는다.
2. dfs로 치킨집을 선택하여 배열에 저장한다.
   1. step이 M이 되면 치킨집과 집들과의 최소거리의 합을 구한다.
   2. 전체 최소거리와 비교하여 갱신한다.

## 소스

```c
#include <stdio.h>

char house[101][3], chk[14], dmin[101][14], chick[14][2];
int hn=0, cn=0, min=130000, m;

int abs(int a) { return (a < 0) ? a * -1 : a; }
int cal(int r, int c, int x, int y) { return abs(r - x) + abs(c - y); }

void cmin()
{
	int i, j, sum=0, mmin;
	for (i = 1; i <= hn; i++)
	{
		mmin = 100;
		for (j = 1; j <= m; j++) mmin = (mmin > dmin[i][chk[j]]) ? dmin[i][chk[j]] : mmin;
		sum += mmin;
	}
	min = (min > sum) ? sum : min;
}

void dfs(int st)
{
	int i;
	if (st == m)
	{
		cmin();
		return;
	}
	for (i = chk[st]+1; i <= cn; i++)
	{
		chk[st+1] = i;
		dfs(st + 1);
	}
}

int main()
{
	int n, i, j, im;
	scanf("%d%d", &n, &m);
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= n; j++)
		{
			scanf("%d", &im);
			if (im == 1)
			{
				hn++;
				house[hn][0] = i;
				house[hn][1] = j;
			}
			else if (im == 2)
			{
				cn++;
				chick[cn][0] = i;
				chick[cn][1] = j;
			}
		}
	}

	for (i = 1; i <= hn; i++)
	{
		for (j = 1; j <= cn; j++) dmin[i][j] = cal(house[i][0], house[i][1], chick[j][0], chick[j][1]);
	}

	dfs(0);
	printf("%d", min);
}
```


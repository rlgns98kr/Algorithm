# 미세먼지 안녕!

https://www.acmicpc.net/problem/17144

## 분석

미세먼지의 확산부분에서 확산시키며 map\[r\]\[c\]의 값이 변하게 되면 확산시키는 양이 변할 수 있다. 따라서 확산시키기 전에 저장을 해주거나 따로 배열을 두어 고정값으로 이용하는 방법이 있다.

공기청정기 작동 부분에서는 -1이 함께 이동되지 않게 주의하고 이동되었더라도 다시 초기화 해주도록 한다.

## 알고리즘

1. mapstatic 배열에 map 배열을 복사한다.
2. map 배열을 mapstatic배열을 이용해 갱신한다.(확산)
3. map 배열에 대해 공기청정기를 작동시킨다.(move)

## 소스

```c
#include <stdio.h>

int map[55][55], mapstatic[55][55], n, m, dd[4][2] = { {0,1},{0,-1},{1,0},{-1,0} }, up=0;

int bc(int r, int c)
{
	if (r > 0 && r <= n && c > 0 && c <= m) return 1;
	return 0;
}

void move()
{
	int r=up-1, c=1, ax=-1, ay=0;
	while (r!=up || c!=1)
	{
		map[r][c] = map[r + ax][c + ay];
		r += ax;
		c += ay;
		if (r == 1 && c == 1)
		{
			ax = 0;
			ay = 1;
		}
		else if (r == 1 && c == m)
		{
			ax = 1;
			ay = 0;
		}
		else if (r == up && c == m)
		{
			ax = 0;
			ay = -1;
		}
	}
	r = up+1+1, c = 1, ax = 1, ay = 0;
	while (r  != up+1 || c  != 1)
	{
		map[r][c] = map[r + ax][c + ay];
		r += ax;
		c += ay;
		if (r == n && c == 1)
		{
			ax = 0;
			ay = 1;
		}
		else if (r == n && c == m)
		{
			ax = -1;
			ay = 0;
		}
		else if (r == up+1 && c == m)
		{
			ax = 0;
			ay = -1;
		}
		
	}
	map[up][2] = 0;
	map[up+1][2] = 0;
}

int main()
{
	int t, i, j, k, l, nr, nc, answer=0;
	scanf("%d%d%d", &n, &m, &t);
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= m; j++)
		{
			scanf("%d", &map[i][j]);
			if (map[i][j] == -1 && up == 0) up = i;
		}
	}
	for (k = 1; k <= t; k++)
	{
		for (i = 1; i <= n; i++)
		{
			for (j = 1; j <= m; j++) mapstatic[i][j] = map[i][j];
		}
		for (i = 1; i <= n; i++)
		{
			for (j = 1; j <= m; j++)
			{
				if(mapstatic[i][j]>0)
				{
					for (l = 0; l < 4; l++)
					{
						nr = i + dd[l][0];
						nc = j + dd[l][1];
						if (bc(nr, nc) == 1 && map[nr][nc] != -1)
						{
							map[nr][nc] += mapstatic[i][j] / 5;
							map[i][j]-= mapstatic[i][j] / 5;
						}

					}
				}
				
			}
		}
		move();
	}

	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= m; j++) answer += map[i][j];
	}
	printf("%d", answer+2);
}
```


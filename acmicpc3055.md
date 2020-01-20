# 탈출

https://www.acmicpc.net/problem/3055

## 개요

돌과 물이 존재하는 map에서 S부터 D까지 도달하는 최단 시간을 출력하는 문제. 도달하지 못한다면 KAKTUS를 출력하면 된다. 물은 1시간에 4방향으로 불어나고 이동은 물이 불어난 후에 같은 속도로 한다.

## 분석

대표적인 BFS문제로 볼 수 있다. 각 step 별로 물을 먼저 4방향으로 1칸씩 퍼트리고 난 뒤에 이동을 하면 쉽게 풀이할 수 있다. 도착하였을 때 출력하고 종료한다면 반복문이 끝날 때는 도착도 하지 못하고 더이상 갈 곳이 없을 때 이므로 따로 조건을 따져보지 않고 KAKTUS를 출력하면 된다.

## 알고리즘

1. step에 해당하는 반복문을 돌린다.
   1. S의이동좌표큐의 마지막 인덱스와 현재인덱스의 값이 같다면 종료한다.
   2. 현재까지 저장되어있는 물의 좌표들의 마지막 인덱스를 저장해놓는다.
   3. 물의 불어남에 해당하는 반복문을 돌린다.
      1. 위에서 저장해놓은 마지막 인덱스가 현재 인덱스와 같다면 break한다.
      2. 현재 인덱스의 좌표값을 기준으로 4방향 탐색한다.
         1. map의 범위에 벗어나지 않는지 0인지와 방문했던 곳인지를 chk한다.
         2. 조건에 일치하면 quem에 넣고 map을 갱신한다.
      3. 현재 인덱스를 증가시킨다.
   4. 현재까지 저장되어있는 S의 좌표들의 마지막 인덱스를 저장해놓는다.
   5. S의 이동에 해당하는 반복문을 돌린다.
      1. 위에서 저장해놓은 마지막 인덱스가 현재 인덱스와 같다면 break한다.
      2. 현재 인덱스의 좌표값을 기준으로 4방향 탐색한다.
         1. 도달한 곳이 D이면 **step을 출력하고 종료**한다.
         2. map의 범위에 벗어나지 않는지 0인지와 방문했던 곳인지를 chk한다.
         3. 조건에 일치하면 que에 넣는다.
2. **KAKTUS 출력**

## 소스

```c
#include <stdio.h>

int n, m, map[55][55], chk[55][55][2], que[5000][3], quem[5000][2], qr = 0, qf = 0, qmr = 0, qmf = 0, dd[4][2] = { {0,1},{0,-1},{1,0},{-1,0} };

int bc(int a, int b)
{
	if (a >= 1 && a <= n && b >= 1 && b <= m) return 1;
	return 0;
}

int main()
{
	int	i, j, nx, ny, br;
	char im;
	scanf("%d%d\n", &n, &m);
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= m; j++)
		{
			scanf("%c", &im);
			if (im == '.') map[i][j] = 0;
			else if (im == 'D') map[i][j] = 3;
			else if (im == '*')
			{
				quem[qmf][0] = i;
				quem[qmf][1] = j;
				quem[qmf++][2] = 0;
				map[i][j] = 2;
			}
			else if (im == 'S')
			{
				que[qf][0] = i;
				que[qf][1] = j;
				que[qf++][2] = 0;
				map[i][j] = 1;
			}
			else map[i][j] = -1;
		}
		scanf("%c", &im);
	}
	while (1)
	{
		if (qr == qf) break;
		br = qmf;
		while (1)
		{
			if (qmr == br) break;
			for (i = 0; i < 4; i++)
			{
				nx = quem[qmr][0] + dd[i][0];
				ny = quem[qmr][1] + dd[i][1];
				if (bc(nx, ny) == 1 && map[nx][ny] == 0 && chk[nx][ny][0]==0)
				{
					quem[qmf][0] = nx;
					quem[qmf++][1] = ny;
					map[nx][ny] = 2;
					chk[nx][ny][0] = 1;
				}
			}
			qmr++;
		}

		br = qf;
		while (1)
		{
			if (qr == br) break;
			for (i = 0; i < 4; i++)
			{
				nx = que[qr][0] + dd[i][0];
				ny = que[qr][1] + dd[i][1];
				if (map[nx][ny] == 3)
				{
					printf("%d", que[qr][2] + 1);
					return 0;
				}
				if (bc(nx, ny) == 1 && map[nx][ny] == 0 && chk[nx][ny][1]==0)
				{
					que[qf][0] = nx;
					que[qf][1] = ny;
					que[qf++][2] = que[qr][2] + 1;
					chk[nx][ny][1] = 1;
				}
			}
			qr++;
		}
	}
	printf("KAKTUS");
}
```


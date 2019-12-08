# 연구소 3

https://www.acmicpc.net/problem/17142



## 개요

N*N의 Map에는 0, 1, 2의 값이 있다. 바이러스는 2에서 시작하여 전파될 수 있으며 1에는 전파될 수 없다. 1초당 4방향으로 한칸씩 전파된다. M개의 바이러스를 전파하여 모든 0을 채우는 최소 시간을 찾는 문제. (10>=2의 개수>=M>=1, N<=50)



## 분석

### 시간 복잡도

Map에 존재하는 2의 좌표 중 M개를 선택하는 경우의 수의 최악은 10개 중 5개를 선택하는 조합이므로 252가 된다. 이 252개의 경우의 수에 대해 각각의 전파시간들을 BFS로 구해낼 수 있다. BFS의 최악은 모든 좌표를 다 살피는 경우이므로 N*N이 된다. 252 * 2500은 0.25초만에 실행될 수 있다.



### 공간 복잡도

Q의 크기 이외에는 배열의 개수를 쉽게 정할 수 있다. Q의 크기는 좌표의 최악인 2500으로 설정한다.



## 알고리즘

1. Map을 입력받으며 2의 좌표를 미리 저장해놓고 0의 개수를 센다.
2. 재귀함수Combination(int before, int depth)를 이용하여 조합을 찾으며 저장한다. (before<i이면 재귀호출)
3. 재귀함수의 depth가 M과 같아지면 저장된 조합을 이용하여 BFS를 구현한다.
   1. Q에서 좌표를 하나 꺼낸다.
   2. 4방향의 방문할 좌표에 대해 검사하고 만족하면 Q에 넣는다.
   3. 0의 개수가 방문한 0의 개수와 같아지면 step을 반환한다.
   4. 그렇지 않고 q에 방문할 곳이 없으면 9999를 반환한다.
4. BFS의 반환값 중 최솟값을 출력한다.(9999일 경우에는 -1)



## 주의사항

만약 일반적인 BFS의 종료조건인 Q에 좌표값이 없을 때로 구현하였다고 가정해보자. 마지막 Step에 Q에 있는 모든 Map[좌표]가 2라면 Step은 하나 감소되어야한다.(문제 조건에 의하면 2는 통과는 되면서 채우지는 않아도 되는 값임)

따라서, 처음에 0의 개수를 세어주어 BFS에서 0을 방문할 때마다 개수를 세어 비교한다면 적절히 종료할 수 있다.



## 소스

```c
#include <stdio.h>

int map[55][55], n, m, chk[55][55], vi[15][2], select[15], min = 9999, dd[4][2] = { {0,-1},{0,1},{1,0},{-1,0} }, via=0, yc=0;
int que[5000][2], que2[5000][2];

int bc(int r, int c)
{
	if (r > 0 && r <= n && c > 0 && c <= n) return 1;
	return 0;
}

void dfs(int sn, int st)
{
	int i;
	if (st == m)
	{
		i = bfs();
		if (min > i) min = i;
		return;
	}
	for (i = 1; i <= via; i++)
	{
		if (sn < i)
		{
			select[st + 1] = i;
			dfs(i, st + 1);
		}
	}
}

int bfs()
{
	int t, i, j, count, st=0, in=0, r, c, nr, nc, oc;
	oc = yc;
	if (oc == 0) return 0;
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= n; j++)
		{
			chk[i][j] = 0;
		}
	}
	for (i = 1; i <= m; i++)
	{
		in++;
		que[in][0] = vi[select[i]][0];
		que[in][1] = vi[select[i]][1];
		chk[que[in][0]][que[in][1]] = 1;
	}
	while (1)
	{
		st++;
		count = 0;
		
		for (i = 1; i <= in; i++)
		{
			r = que[i][0];
			c = que[i][1];
			for (j = 0; j < 4; j++)
			{
				nr = r + dd[j][0];
				nc = c + dd[j][1];
				if (bc(nr, nc)==1 &&map[nr][nc] != 1 && chk[nr][nc] == 0)
				{
					count++;
					chk[nr][nc] = 1;
					que2[count][0] = nr;
					que2[count][1] = nc;
					if(map[nr][nc]==0) --oc;
				}
			}
		}
		if (oc == 0) return st;
		if (count == 0) return 9999;
		for (i = 1; i <= count; i++)
		{
			que[i][0] = que2[i][0];
			que[i][1] = que2[i][1];
			que2[i][0] = 0;
			que2[i][1] = 0;
		}
		for (i = count + 1; i <= in; i++)
		{
			que2[i][0] = 0;
			que2[i][1] = 0;
		}
		in = count;
	}
}

int main()
{
	int i, j;
	scanf("%d%d", &n, &m);
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= n; j++)
		{
			scanf("%d", &map[i][j]);
			if (map[i][j] == 2)
			{
				via++;
				vi[via][0] = i;
				vi[via][1] = j;
			}
			else if (map[i][j] == 0) ++yc;
		}
	}
	dfs(0, 0);
	if (min == 9999) min = -1;
	printf("%d", min);
}
```


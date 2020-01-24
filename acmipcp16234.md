# 인구 이동

https://www.acmicpc.net/problem/16234

## 개요

n\*n 의 인구 수의 map이 주어진다. 상, 하, 좌, 우 4방향의 인구수가 min이상이고 max이하이면 국경선을 연다. 국경선을 연 나라들을 연합이라고 부르겠다. step마다 연합별 로 인구가 이동한다. 연합의 개별 국가 별 인구수는 연합의 인구수의 평균값을 소수 첫째자리에서 내림한 값이 된다. step이 몇번 일어나는 지를 출력하면 된다.

## 분석

반복문으로 좌표마다 4방향의 국경선이 열리는 지 검사하고 갱신해준다면 어떤 좌표를 먼저 검사하느냐에 따라 연결되어 있는 연합을 검사하지 못할 수 있다. 예를 들어 0행 0열부터 검사한다면 \(0,3\), \(1,3),\(1,2\),\(1, 1\)의 연합이 \(0,3\), (1,3) 연합과 \(1,1\),\(1,2\)의 연합으로 여겨질 수 있다. 따라서 한 좌표에서 이어진 모든 좌표들을 찾아야하는데 이는 DFS나 BFS를 이용할 수 있다.

DFS를 이용하는 경우 좌표수가 2500개이므로 4^2500\(좌표당 4방향\) 의 방문을 해야하지만 방문한 곳을 chk하여 다시 방문하지 못하게 한다면 한 좌표당 최대 방문수는 2500이 된다.

이렇게 DFS를 이용하여 좌표마다 이어진 곳을 방문하며 sum과 방문수를 구한다. 후에 방문한 곳들을 chk를 이용하여 sum/방문수로 갱신해준다. 여기서 chk는 sum/방문수를 저장한 que의 index로 갱신해준다면 이후에 map갱신을 쉽게 할 수 있다.

## 알고리즘

1. flag==1 이면 반복한다.(while)
   1. flag=0
   2. i:1\~n, j:1\~n 반복
      1. (i, j)를 시작으로  방문좌표(r, c)에 대해 **chk\[r\]\[c\]를 f로 갱신**하고 인구(sum)와 수(t)를 세며 DFS를 돌린다.
      2. **que\[f++\]=sum/t**
      3. t가 1보다크면 flag를 1로 갱신한다. (2번이상 방문한 곳이므로 인구이동이 일어났다는 것을 의미한다.)
   3. i:1\~n, j:1\~n 반복
      1. **map\[i\]\[j\] = que\[chk\[i\]\[j\]\]**
      2. chk\[i\]\[j\] = 0
   4. flag==1 이면 answer++
2. answer 출력

## 소스

```c
#include <stdio.h>

int map[60][60], chk[60][60], dd[4][2] = { {0,1},{0,-1},{1,0},{-1,0} }, que[2600], f;
int n, min, max, t, sum;

int abs(int n)
{
	if (n < 0) return n * -1;
	return n;
}

int chkchk(int a, int b, int c, int d)
{
	if (abs(map[a][b] - map[c][d]) <= max && abs(map[a][b] - map[c][d]) >= min) return 1;
	return 0;
}

int bc(int r, int c)
{
	if (r >= 1 && r <= n && c <= n && c >= 1) return 1;
	return 0;
}

void dfs(int r, int c)
{
	int i, nr, nc;
    
	chk[r][c] = f;
	sum+=map[r][c];
	++t;
    
	for (i = 0; i < 4; i++)
	{
		nr = r + dd[i][0];
		nc = c + dd[i][1];
		if (bc(nr, nc) == 1 && chk[nr][nc]== 0 && chkchk(r, c, nr, nc) == 1) dfs(nr, nc);
	}
}

int main()
{
	int i, j, flag=1, answer=0;
	scanf("%d%d%d", &n, &min, &max);
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= n; j++)
		{
			scanf("%d", &map[i][j]);
		}
	}
	
	while (flag == 1)
	{
		flag = 0;
        f = 1;
        
		for (i = 1; i <= n; i++)
		{
			for (j = 1; j <= n; j++)
			{
				if (chk[i][j] == 0)
				{
					t = 0;
					sum = 0;
					dfs(i, j);
                    
					que[f++] = sum / t;

					if (t > 1) flag = 1;
				}
			}
		}
        
		for (i = 1; i <= n; i++)
		{
			for (j = 1; j <= n; j++)
			{
				map[i][j] = que[chk[i][j]];
				chk[i][j] = 0;
			}
		}
        
		for (i = 1; i <= f; i++) que[i] = 0;
		if(flag==1) ++answer;
	}
	printf("%d", answer);
}
```


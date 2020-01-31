# Puyo Puyo

## 분석

터질 수 있는 뿌요가 여러 그룹이 있다면 동시에 터져야 하고 여러 그룹이 터지더라도 한번의 연쇄가 추가된다.

위 문장으로 터질 수 있는 뿌요들을 전부 터트리고 나서 중력으로 끌어당겨야 함을 알 수 있다.

## 알고리즘

1. 전 단계에 뿌요가 터졌다면 반복 while(flag==1)
   1. flag=0
   2. 전 좌표에 대해 반복
      1. chk배열 초기화를 하면서 count\>=4인 터질 수 있는 뿌요들을 터트린다.
      2. 4방향의 같은 색 뿌요들을 DFS로 검색하며 count를 증가시키고 방문지를 chk한다.
      3. count\>=4이면 flag=1
   3. 중력의 영향으로 뿌요들 떨어짐
   4. answer++
2. answer-1 출력

## 기타

중력의 영향으로 뿌요들이 떨어지는 것에 대해 배열을 새로 생성하지 않고 1번의 탐색으로 복사하는 것에 대한 고민이 있었다. 갱신될 index와 탐색하는 index가 같지 않을 때 탐색하는 index의 map\[index\]\[j\]값을 0으로 바꿔주는 것으로 해결하였다. (down함수)

## 소스

```c
#include <stdio.h>

int map[15][10], chk[15][10], dd[4][2] = { {-1,0},{1,0},{0,-1},{0,1} }, count, flag = 1;

int bc(int r, int c)
{
	if (r > 0 && r <= 12 && c > 0 && c <= 6) return 1;
	return 0;
}

void down()
{
	int i, j, si;
	for (j = 1; j <= 6; j++)
	{
		si = 12;
		for (i = 12; i > 0; i--)
		{
			if (map[i][j] != 0)
			{
				map[si][j] = map[i][j];
				if (si != i) map[i][j] = 0;
				si--;
			}
		}
	}
}
void cho()
{
	int i, j;
	for (i = 1; i <= 12; i++)
	{
		for (j = 1; j <= 6; j++)
		{
			if (chk[i][j] != 0)
			{
				if (count >= 4)
				{
					flag = 1;
					map[i][j] = 0;
				}
				chk[i][j] = 0;
			}
		}
	}
}
void dfs(int r, int c)
{
	int nr, nc, i;
	chk[r][c] = 1;
	++count;
	for (i = 0; i < 4; i++)
	{
		nr = r + dd[i][0];
		nc = c + dd[i][1];
		if (bc(nr, nc) == 1 && chk[nr][nc] == 0 && map[nr][nc] == map[r][c]) dfs(nr, nc);
	}
}

int main()
{
	int i, j, answer = 0;
	char imsi;
	for (i = 1; i <= 12; i++)
	{
		for (j = 1; j <= 6; j++)
		{
			scanf("%c", &imsi);
			if (imsi == '.') map[i][j] = 0;
			else if (imsi == 'R') map[i][j] = 1;
			else if (imsi == 'G') map[i][j] = 2;
			else if (imsi == 'B') map[i][j] = 3;
			else if (imsi == 'P') map[i][j] = 4;
			else if (imsi == 'Y') map[i][j] = 5;
		}
		scanf("%c", &imsi);
	}
	while(flag==1)
	{
		flag = 0;
		for (i = 1; i <= 12; i++)
		{
			for (j = 1; j <= 6; j++)
			{
				cho();
				count = 0;
				if (map[i][j] != 0) dfs(i, j);
			}
		}
		answer++;
		down();
	}
	printf("%d", answer-1);
}
```
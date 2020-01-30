# 아기 상어

https://www.acmicpc.net/problem/16236

## 분석

 아기 상어는 자신의 크기보다 작은 물고기만 먹을 수 있다. 따라서, 크기가 같은 물고기는 먹을 수 없지만, 그 물고기가 있는 칸은 지나갈 수 있다.

먹을 수 있는 물고기가 1마리보다 많다면, 거리가 가장 가까운 물고기를 먹으러 간다.

거리는 아기 상어가 있는 칸에서 물고기가 있는 칸으로 이동할 때, 지나야하는 칸의 개수의 최솟값이다.

거리가 가까운 물고기가 많다면, 가장 위에 있는 물고기, 그러한 물고기가 여러마리라면, 가장 왼쪽에 있는 물고기를 먹는다.



따라서 BFS로 지날 수 있는 지점을 방문하면서 Step별로 먹을 수 있는 물고기들 중 가장 위에 있으면서 가장 왼쪽에 있는 물고기를 잡아먹으면 된다.

## 알고리즘

1. 시작 위치를 q에 넣는다.
2. 전 단계에서 물고기를 잡아먹었다면 반복한다. while(flag==1)
   1. flag=0
   2. BFS while(front<rear)
      1. step별 반복을 위해 현재 step의 rear를 저장 orear=rear;
      2. step별 반복 while(front<orear)
         1. 방문가능하면
            1. que에 넣음(BFS)
            2. 먹을수 있으면 행과 열의 최솟값갱신, flag=1
      3. 해당 step에 먹을 수 있는 물고기가 있다면 먹고 answer+step, break
   3. 배열들 초기화 및 시작 위치 갱신
3. answer 출력

## 소스

```c
#include <stdio.h>

int n, map[21][21], chk[21][21], que[400][2], front=0, rear=0, orear;
int dd[4][2] = { {-1,0},{0,-1},{0,1},{1,0} };

int bc(int r, int c)
{
	if (r >= 1 && r <= n && c >= 1 && c <= n) return 1;
	return 0;
}

int main()
{
	int i, j, cr, cc, nr, nc, mr, mc, md, step, ss = 2, se = 0, flag = 1, answer = 0;

	scanf("%d", &n);
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= n; j++)
		{
			scanf("%d", &map[i][j]);
			if (map[i][j] == 9)
			{
				que[rear][0] = i;
				que[rear][1] = j;
                map[i][j]=0;
				rear++;
			}
		}
	}

	while (flag == 1)
	{
		flag = 0;
		mr = 21;
		mc = 21;
        
        step=0;
		while (front < rear)
		{
			orear = rear;
            step++;
			while (front < orear )
			{
				cr = que[front][0];
				cc = que[front][1];
				front++;

				chk[cr][cc] = 1;
				
				for (i = 0; i < 4; i++)
				{
					nr = cr + dd[i][0];
					nc = cc + dd[i][1];

					if (bc(nr, nc) == 1 && map[nr][nc] <= ss && chk[nr][nc] == 0)
					{
						if (map[nr][nc] != 0 && map[nr][nc] < ss)
						{
							if (nr < mr || (nr == mr && nc < mc))
							{
								mr = nr;
								mc = nc;
								flag = 1;
							}
						}

						chk[nr][nc] = 1;
						que[rear][0] = nr;
						que[rear][1] = nc;
						rear++;
					}
				}
			}
			if (flag == 1)
			{
				map[mr][mc] = 0;
				se++;
				if (ss == se)
				{
					se = 0;
					ss++;
				}
				answer += step;
				break;
			}
		}

		for (i = 1; i <= n; i++)
		{
			for (j = 1; j <= n; j++) chk[i][j] = 0;
		}

		rear = 0;
		front = 0;
		que[rear][0] = mr;
		que[rear][1] = mc;
		rear++;
	}
	printf("%d", answer);
}
```


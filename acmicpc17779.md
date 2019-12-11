# 게리맨더링 2

 N * N(<=20) map에 인구가중치(<=100)가 주어진다. N * N을 5개 구역으로 나누어 인구가 가장 많은 구역과 인구가 가장 적은 구역의 차이의 최소값을 구하는 문제.

 

### 문제유형 : 브루트포스(완전 탐색)



### 알고리즘

1.  5구역의 가장 윗 꼭지점인 (r, c)를 정한다. (for문 0~n-1)

2. d1, d2를 정한다. (for문 1~n-2)

3. map을 탐색하며 인덱스(i,j)와 경계조건을 비교한다.(xy좌표평면에서 부등식에 해당하는 영역을 떠올리면 쉽다. ex) 1구역의 경우 열(j)은 5구역의 c보다 작거나 같아야 하고 행(i)은 r+d1보다 작아야 하고 행과 열의 합(i+j)은 r+c보다 작아야 한다.)

4. 구역 별로 표시한 뒤 구역별 인구 수 합을 구한다.

5. 가장 인구가 많은 지역과 적은 지역의 차이의 최솟값을 갱신한다.

 

### 풀이

 같은 대각선 내의 점들의 행과 열의 합이나 차가 일정하다는 것을 알면 조건을 따지기 쉽다.



### 코드

```c
#include <stdio.h>

int map[50][50], bang[2][2] = { {1,-1},{1, 1} }, n, chk[50][50], mina=99999;

void chkk()
{
	int i, j, min=99999, max=0, s[6];
	for (i = 1; i <= 5; i++) s[i] = 0;
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= n; j++)
		{
			if (chk[i][j] == 1) s[1] += map[i][j];
			if (chk[i][j] == 2) s[2] += map[i][j];
			if (chk[i][j] == 3) s[3] += map[i][j];
			if (chk[i][j] == 4) s[4] += map[i][j];
			if (chk[i][j] == 5) s[5] += map[i][j];
		}
	}
	
	for (i = 1; i <= 5; i++)
	{
		if (min > s[i]) min = s[i];
		if (max < s[i]) max = s[i];
	}
	if (mina > (max - min)) mina = max - min;
}

int bc(int r, int c)
{
	if (r <= n && r >= 1 && c <= n && c >= 1) return 1;
	return 0;
}

void cal(int r, int c)
{
	int i, j, k, re[3],ce[3], sw, l;
	for (i = 1; i <= n; i++)//d1
	{
		for (j = 1; j <= n; j++)//d2
		{
			sw = 0;
			re[0] = r + j;
			ce[0] = c + j;
			re[1] = re[0] + i;
			ce[1] = ce[0] - i;
			re[2] = r + i;
			ce[2] = c - i;
			for (k = 0; k < 3; k++)
			{
				if (bc(re[k], ce[k]) == 0) sw = 1;
			}
			if (sw == 1) continue;

			for (k = 1; k <= n; k++)
			{
				for (l = 1; l <= n; l++)
				{
					if (re[2] > k&& c >= l && k + l < r + c) chk[k][l] = 1;
					else if (c < l && k <= re[0] && c - r < l - k) chk[k][l] = 2;
					else if (re[2] <= k && ce[1] > l&& ce[2] - re[2] > l - k) chk[k][l] = 3;
					else if (re[0]<k && ce[1] <= l && k + l >re[1] + ce[1]) chk[k][l] = 4;
					else chk[k][l] = 5;
				}
			}
			chkk();

		}
	}
}

int main()
{
	int i, j;
	scanf("%d", &n);
	for (i = 1; i <= n; i++)
	{
		for (j = 1; j <= n; j++)
		{
			scanf("%d", &map[i][j]);
		}
	}
	for (i = 1; i <= n; i++)
	{
		for (j = 2; j < n; j++)
		{
			cal(i, j);
		}
	}
	printf("%d", mina);
}
```


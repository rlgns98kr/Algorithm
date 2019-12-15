# 게리맨더링

https://www.acmicpc.net/problem/17471

## 개요

그래프의 노드(2<=N<=10)를 각각 연결된 두 부분으로 나누어서 각 노드의 가중치들의 합의 차이를 최소화하는 문제

## 분석

### 시간 복잡도

O(2^N)의 경우의 수로 두 부분을 나누고 연결되어있는지 확인O(N) 하면 O(2^N*N)의 시간 복잡도를 가진다.

### 공간 복잡도

인접행렬 N^2*4 + a byte의 공간복잡도를 가진다.

## 알고리즘

1. 인접행렬을 만든다.
2. i=0~2^N-1
   1. i(10진수)에 해당하는 이진수를 배열(이진수의 길이는 N)에 저장한다.
   2. 이진수의 0과 1의 개수를 센다.
   3. 이진수 배열의 값이 0인 Index(노드)들만 인접행렬을 이용하여 연결되어 있는모든 지점을 방문하여 개수를 센다.
      1. 인접행렬이 연결되어있고 방문하지 않았으며 저장된 배열의 값이 0이면 방문한다.
   4. 이진수 배열의 값이 1인  Index(노드)들만 인접행렬을 이용하여 연결되어 있는모든 지점을 방문하여 개수를 센다.
      1. 인접행렬이 연결되어있고 방문하지 않았으며 저장된 배열의 값이 1이면 방문한다.
   5. 3에서 센 개수와 4, 5에서 센 개수가 같은 지 검사한다.
      1. 같다면 Min을 갱신한다.
3. Min 출력

## 기타

이진수를 이용하여 모든 경우의 수를 따져줄 수 있다. 물론 재귀형식의 브루트포스로도 모든 경우의 수를 따져줄 수 있지만 10!의 시간복잡도를 줄이려면 중복된 수열을 제거해주는 복잡한 코드가 필요할 것 같다는 생각이 든다.

또한 이진수를 이용하여 구역을 나눈 후에 미리 노드들의 가중치의 차를 계산하면 현재 Min보다 그 가중치의 차이가 더 적을때만 검사해주는 것이 가능하므로 시간을 단축시킬 수 있다. 

## 소스

```c
#include <stdio.h>
#include <math.h>

int n, hn[11], select[11], min=99999, chk0, chk1, map[11][11], chk[11], cnt00, cnt11;

int cal()
{
	int cha=0, i;
	for (i = 1; i <= n; i++)
	{
		if (select[i] == 0) cha += hn[i];
		else cha -= hn[i];
	}
	return abs(cha);
}

void chk0dfs(int s)
{
	int i;
	for (i = 1; i <= n; i++)
	{
		if (map[s][i] == 1 && select[i] == 0 && chk[i] == 0)
		{
			cnt00++;
			chk[i] = 1;
			chk0dfs(i);
		}
	}
}

void chk1dfs(int s)
{
	int i;
	for (i = 1; i <= n; i++)
	{
		if (map[s][i] == 1 && select[i] == 1 && chk[i] == 0)
		{
			cnt11++;
			chk[i] = 1;
			chk1dfs(i);
		}
	}
}

int main()
{
	int i, j, time, im, imm, s0, s1, cnt0, cnt1;
	scanf("%d", &n);
	for (i = 1; i <= n; i++) scanf("%d", &hn[i]);
	for (i = 1; i <= n; i++)
	{
		scanf("%d", &im);
		for (j = 1; j <= im; j++)
		{
			scanf("%d", &imm);
			map[i][imm] = 1;
			map[imm][i] = 1;
		}
	}
	time = pow(2, n);
	for (i = 0; i < time; i++)
	{
		im = i;
		s0 = 0;
		s1 = 0;
		cnt0 = 0;
		cnt1 = 0;
		for (j = 1; j <= n; j++)
		{
			select[j] = im % 2;
			if (s0==0 && select[j] == 0) s0 = j;
			if (s1 == 0 && select[j] == 1) s1 = j;

			if (select[j] == 0) ++cnt0;
			else ++cnt1;
			im /= 2;
		}

		chk0 = 0;
		chk1 = 0;
		im = cal();

		if (im < min && s0!=0 && s1!=0)
		{
			cnt00 = 1;
			cnt11 = 1;

			for (j = 1; j <= n; j++) chk[j] = 0;
			chk[s0] = 1;
			chk0dfs(s0);

			for (j = 1; j <= n; j++) chk[j] = 0;
			chk[s1] = 1;
			chk1dfs(s1);

			if(cnt00 == cnt0) chk0 = 1;
			if (cnt11 == cnt1) chk1 = 1;
		}

		if (chk0 == 1 && chk1 == 1 && min > im) min = im;
	}
	if (min != 99999) printf("%d", min);
	else printf("-1");
}
```


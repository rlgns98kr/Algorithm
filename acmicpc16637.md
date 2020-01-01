# 괄호 추가하기

https://www.acmicpc.net/problem/16637



## 개요

N(0<=N<=19)개의 길이의 수식에 숫자와 연산자가 번갈아 가며 주어진다. 숫자는 0~9이고, 연산자는 *, +, - 3종류이다. 연산자의 우선순위는 같기 때문에 괄호를 먼저 계산하고 왼쪽부터 계산한다. 괄호안에는 연산자가 한개만 들어 있어야 하고 중첩된 괄호는 사용할 수 없다. 괄호의 개수는 제한이 없을 때 수식의 최댓값을 구하는 문제



## 분석

### 브루트포스

N이 19이기 때문에 연산자의 개수는 9개가 된다. 이중 괄호를 칠 수 있는 조합의 개수는 2^9-(연속된 연산자들에 괄호를 친 경우=중첩된 괄호) 이기때문에 2^9*N 전수조사로도 풀릴 수 있는 문제이다.

전 단계에서 괄호를 친 연산자보다 왼쪽에 있는 연산자들만 선택할 수 있도록 재귀함수를 구현한다면 괄호의 조합을 모두 조사할 수 있다.

### DP

왼쪽부터 계산한다는 문제의 조건으로부터 문제를 작은문제들로 나눌 수 있다. 다음 연산자를 바로 계산할 지, 아니면 그 다음 나오는 연산자에 괄호를 쳐서 계산할 지의 문제가 되는 것이다.

1\*3\-2-> (왼쪽부터 그대로 계산) 혹은 1\*(3\-2)

결국 **현재까지 계산한 값 \+,\*,\- 값 \+,\*,\- 값...  => Present \+,\*,\- Value1 \+,\*,\- Value2...** 에서 Value1과 2사이에 괄호를 치거나 치지않거나의 문제가 되는 것이다.

여기서 연산자가 \+와 \*로만 주어지는 경우에 Present가 최댓값이 되면 식 전체가 최댓값이 됨을 알 수 있다. 따라서 Present를 항상 최대로 만드는 값만을 저장하고 있다면 최종적으로 답을 구할 수 있을 것이다.

연산자 \-가 추가적으로 주어지면 어떨까? Present가 음수일(최대값이 아닐) 때 **NextPresent = Present \* (Value1\-Value2)** 에서 최댓값이 나올 수 있다. 하지만 이 최댓값은 **Present가 최솟값**이어야만 나올 수 있다.(가장 작은 음수와 음수를 곱해야 가장 큰 양수가 되기 때문이다)

그렇다면 Present가 최솟값일 때와 최댓값일 때 나올 수 있는 값들을 비교하여 최댓값을 찾는다면 그것이 NextPresent의 최댓값일 것이다.

하지만 최솟값또한 Present가 양수일(최솟값이 아닐) 때 **NextPresent = Present \* (Value1\-Value2)** 에서 최솟값이 나올 수 있다. 따라서 이까지 고려해준다면 NextPresent는 항상 최댓값이 될 것이다.

## 알고리즘

1. map에 입력을 받는다.(홀수인덱스에는 숫자, 짝수인덱스에는 연산자가 들어있다.)
2. maxmax와 minmin배열의 \[1\],\[3\]을 map배열의 \[1\],\[3\]으로 초기화 한다.
3. i를 5부터 n까지 2씩증가시키며 반복한다.(홀수인덱스에는 숫자)
   1. \(maxmax\[i-2\]와 map[i]를 map[i-1]로 연산한 값\)과 \(maxmax[i-4]과 \(map[i-2]와 map[i]를 연산한 값\)을 연산한 값\) 중 큰것으로 maxmax[i]를 갱신한다. \(순서대로 연산할 지 map[i-1]의 연산에 괄호를 칠 지\)
   2. minmin도 같은 방법으로 작은것으로 갱신한다.
   3. (mamax[i])와 (minmin[i-4]와 (map[i-2]와 map[i]를 연산한 값)을 연산한 값) 중 큰것으로 maxmax[i]를 갱신한다.(minmin[i-4]가 음수이고 map[i-3]가 *이고 map[i-1]이 - 일 때 maxmax가 갱신될 수 있다)
   4. minmin도 같은 방법으로 갱신한다.
4. maxmax[n] 출력

## 소스

```c
#include <stdio.h>

int map[25], maxmax[25], minmin[25];

int max(int a, int b)
{
	if (a >= b) return a;
	return b;
}
int min(int a, int b)
{
	if (a <= b) return a;
	return b;
}
int red(char a)
{
	if (a == '+') return -1;
	if (a == '-') return -2;
	if (a == '*') return -3;
	else return (int)(a-'0');
}
int cal(int i, int a, int b)
{
	if (map[i] == -1) return a + b;
	if (map[i] == -2) return a - b;
	if (map[i] == -3) return a * b;
}

int main()
{
	int n, i, im;

	scanf("%d\n", &n);
	for (i = 1; i <= n; i++)
	{
		scanf("%c", &im);
		map[i] = red(im);
	}

	minmin[1]= maxmax[1] = map[1];
	minmin[3]= maxmax[3] = cal(2, map[1], map[3]);

	for (i = 5; i <= n; i+=2)
	{
		maxmax[i] = max(cal(i - 1, maxmax[i - 2], map[i]), cal(i - 3, maxmax[i - 4], cal(i - 1, map[i - 2], map[i])));
		minmin[i] = min(cal(i - 1, minmin[i - 2], map[i]), cal(i - 3, minmin[i - 4], cal(i - 1, map[i - 2], map[i])));
		maxmax[i] = max(cal(i - 3, minmin[i - 4], cal(i - 1, map[i - 2], map[i])), maxmax[i]);
		minmin[i] = min(cal(i - 3, maxmax[i - 4], cal(i - 1, map[i - 2], map[i])), minmin[i]);
	}

	printf("%d", maxmax[n]);
}
```


# 프로그래머스 Level 1 모음

## 예산

### 알고리즘

1. 내림차순 정렬 후 순서대로 예산을 초과하는 지 확인한다.
2. 모든 부서를 지원하고도 예산이 남을 경우를 잘 처리해주어야 한다.



### 코드

```javascript
function sort_base(a, b){
    return a-b;
}

function solution(d, budget) {
    var answer = -1, sum = 0;
    
    d.sort(sort_base);
    console.log(d);
    for(let i=0; i<d.length; i++){
        sum+=d[i];
        if(sum>budget){
            answer=i;
            break;
        }else if(sum===budget){
            answer=i+1;
            break;
        }
    }
    if(answer===-1) answer=d.length;
    
    return answer;
}
```



## 체육복

### 알고리즘

1. 2중 for를 돌려서 각각의 값들을 비교할 수도 있지만 hash해서 비교하면 시간복잡도가 줄어든다.
2. lost와 reserve가 중복되는 학생들은 hash할 때 제거한다.
3. lost를 오름차순 정렬해서 reserve에서 자신보다 번호가 1 작은 학생에게 먼저 빌리거나 lost를 내림차순 정렬해서 reserve에서 자신보다 번호가 1 큰 학생에게 먼저 빌리는 방법을 사용할 수 있다.(test case는 기본적으로 lost가 오름차순 정렬 되어있는 듯 하다.)



### 코드

```javascript
const base=(a, b) => a-b

function solution(n, lost, reserve) {
    var answer = 0, hash_reserve={}, hash_lost={}, chk_reserve={};
    
    for(let i=0; i<lost.length; i++) hash_lost[lost[i]]=1;
    for(let i=0; i<reserve.length; i++){
        chk_reserve[reserve[i]]=1;
        if(!hash_lost[reserve[i]]) hash_reserve[reserve[i]]=1;
    }
    for(let i=0; i<lost.length; i++){
        if(chk_reserve[lost[i]]) delete hash_lost[lost[i]];
    }
    
    lost.sort(base);
    for(let i=0; i<lost.length; i++){
        if(hash_lost[lost[i]] && hash_reserve[lost[i]-1]){
            delete hash_lost[lost[i]];
            delete hash_reserve[lost[i]-1];
        }
        if(hash_lost[lost[i]] && hash_reserve[lost[i]+1]){
            delete hash_lost[lost[i]];
            delete hash_reserve[lost[i]+1];
        }
    }
    
    answer=n-Object.keys(hash_lost).length;
    return answer;
}
```



## 문자열 내림차순으로 배치하기

### 알고리즘

1. buble 정렬도 가능



### 코드

```c++
#include <string>
#include <vector>

using namespace std;

string solution(string s) {
    string answer = "";
    char imsi;
    for(int i=0; i<s.length(); i++)
    {
        for(int j=i+1; j<s.length(); j++){
            if(s[i]<s[j]){
                imsi=s[i];
                s[i]=s[j];
                s[j]=imsi;
            }
        }
    }
    return s;
}
```



## x만큼 간격이 있는 n개의 숫자

### 코드

```c++
#include <string>
#include <vector>

using namespace std;

vector<long long> solution(int x, int n) {
    vector<long long> answer;
    answer.push_back(x);
    for(int i=1; i<n; i++){
        answer.push_back(answer.back()+x);
    }
    return answer;
}
```



## 행렬의 덧셈

### 코드

```c++
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> solution(vector<vector<int>> arr1, vector<vector<int>> arr2) {
    for(int i=0; i<arr1.size(); i++){
        for(int j=0; j<arr1[0].size(); j++){
            arr1[i][j]+=arr2[i][j];
        }
    }
    return arr1;
}
```



## 직사각형 별 찍기

### 코드

```c++
#include <iostream>

using namespace std;

int main(void) {
    int a;
    int b;
    cin >> a >> b;
    for(int i=1; i<=b; i++){
        for(int j=1; j<=a; j++){
            printf("*");
        }
        printf("\n");
    }
    return 0;
}
```



## 시저 암호

### 알고리즘

1. s의 각 인덱스에 접근하여 공백, 대문자, 소문자를 구분한다.
2. s가 소문자일 경우 stack overflow가 날 수 있으므로 비교한 후에 'z'를 넘어가면 26을 빼준 뒤 다시 n을 더해주면 stack overflow를 방지할 수 있다.



```c++
#include <string>
#include <vector>

using namespace std;

string solution(string s, int n) {
    string answer = "";
    for(int i=0; i<s.length(); i++){
        if(s[i]!=' ')
        {
            if(s[i]>='a' && s[i]<='z'){
                if(s[i]+n>'z') s[i]-=26;
                s[i]+=n;
            }else if(s[i]>='A'&& s[i]<='Z'){
                if(s[i]+n>'Z') s[i]-=26;
                s[i]+=n;
            }
        }
        answer=answer+s[i];
    }
    return answer;
}
```


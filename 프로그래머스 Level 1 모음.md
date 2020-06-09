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



## 2016년

### 알고리즘

1. 배열을 선언해서 반복문을 돌린다.



### 코드

```python
def solution(a, b):
    answer = ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT']
    month = [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    s = 0
    
    if a == 1 :
        s+=b-1
    else :
        s+=30
        for i in range(2, a) :
            s+=month[i-1];
        s+=b;

    return answer[(5+s)%7]
```



## 가운데 글자 가져오기

### 코드

```python
def solution(s):
    answer = ''
    a=len(s);
    if a%2==1 : answer= s[int(a/2)]
    else : answer = s[int(a/2)-1]+s[int(a/2)]
    return answer
```



## 문자열 내 마음대로 정렬하기

### 함수

1. sort함수의 비교함수는 -1이면 a가 앞으로 1이면 b가 앞으로

### 코드

```javascript
var ni;

function basic(a, b){
    if(a[ni]<b[ni]) return -1;
    else if(a[ni]>b[ni]) return 1;
    else{
        if(a<b) return -1;
        else if(a>b) return 1;
        return 0;
    }
}

function solution(strings, n) {
    var answer = [];
    ni=n;
    strings.sort(basic);
    return strings;
}
```



## 서울에서 김서방 찾기

### 코드

```python
def solution(seoul):
    return "김서방은 "+str(seoul.index('Kim'))+"에 있다"
```



## 핸드폰 번호 가리기

### 코드

```python
def solution(seoul):
    return "김서방은 "+str(seoul.index('Kim'))+"에 있다"
```



## 평균 구하기

### 코드

```javascript
function solution(arr) {
    return arr.reduce((acc, current)=>acc+current)/arr.length;
}
```



## 이상한 문자 만들기

### 코드

```python
def solution(s):
    answer='';
    bar=0;
    for i in range(0, len(s)) :
        if s[i]==' ' : 
            a=' '
            bar=(i+1);
        elif (i-bar)%2==0 : a=s[i].upper();
        else : a=s[i].lower();
        answer=answer+a;
    return answer
```



## 문자열을 정수로 바꾸기

### 코드

```javascript
function solution(s) {
    return Number(s);
}
```



## 문자열 다루기 기본

### 코드

```python
def solution(s):
    return s.isdecimal() and (len(s)==4 or len(s)==6)
```



## 문자열 내 p와 y의 개수

### 코드

```python
def solution(s) :
    return (s.count('p')+s.count('P')) == (s.count('y')+s.count('Y'))
```



## 소수 찾기

### 코드

```javascript
function solution(n) {
    var answer = 0;
    var test=new Array(n+1);
    for(let i=2; i<=Math.sqrt(n)+1; i++){
        for(let j=i; j<=n; j+=i){
            if(j!=i && j%i===0) test[j]=1;
        }
    }
    
    for(let i=2; i<=n; i++){
        if(!test[i]) ++answer;
    }
    return answer;
}
```



## 최대공약수와 최소공배수

## 알고리즘

1. \(A=Ga\>B=Gb\) 라고 한다면 a와 b는 서로소이므로 1이상의 공통의 약수가 없다. 따라서 a와 a-b도 공통인 약수가 없다.

2. 이러한 원리를 이용하여 A와 B의 최대공약수는 A와 A-B의 최대공약수와 같다.
3. 계속하여 큰 수에서 작은 수를 빼주다 보면\(A>B\)? \(A-B, B\):\(A, B-A\) 두 수가 같아진다. 서로소인 두 수를 가정하였을 때, 큰 수에서 작은 수 반복해서 빼주다 보면 두 수 모두 1이 된다.

4. 두 수가 같아질 때가 두 수(a,b) 모두 1이되었을 때이므로 그때의 수는 Ga=G 가 된다.

### 코드

```javascript
function solution(n, m) {
    var answer = [];
    var big=(n>=m)? n:m
    var small=(n<m)? n:m
    while(1){
        if(big>small) big-=small;
        else if(big<small) small-=big;
        else break;
    }
    answer[0]=big;
    answer[1]=answer[0]*(n/answer[0])*(m/answer[0]);
    return answer;
}
```



## 나누어 떨어지는 숫자 배열

### 코드

```javascript
function solution(arr, divisor) {
    let answer=arr.filter((data)=>data%divisor===0).sort((a,b)=>a-b);
    return (answer[0])? answer:[-1]
}
```




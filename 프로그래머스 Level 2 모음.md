# 프로그래머스 Level 2 모음

## n진수 게임

### 코드

```javascript
function solution(n, t, m, p) {
    var answer = '';
    var ABC='ABCDEF'
    var su='0';
    for(let i =0; i<=t*m; i++){
        let j=i;
        let a=''
        while(j){
            if(j%n>=10) a=ABC[(j%n)%10]+a;
            else a=j%n+a;
            j=parseInt(j/n);
        }
        su=su+a;
    }
    for(let i=0; i<t; i++) answer+=su[p+i*m-1]
    return answer;
}
```

## 파일명 정렬

### 알고리즘

1. 파일명에는 `,`가 들어가지 않기 때문에 쉼표를 추가하고 인덱스를 써넣어 두 파일명이 같을 때 기준으로 삼는다.

### 코드

```javascript
function base(a, b){
    let a1, a2, a3, b1, b2, b3;
    for(let i=0; i<a.length; i++){
        if(a[i]>='0' && a[i]<='9'){
            a1=i;
            break;
        }
    }
    for(let i=0; i<b.length; i++){
        if(b[i]>='0' && b[i]<='9'){
            b1=i;
            break;
        }
    }
    for(let i=a1; i<a.length; i++){
        if(a[i]<'0' || a[i]>'9'){
            a2=i;
            break;
        }
    }
    for(let i=b1; i<b.length; i++){
        if(b[i]<'0' || b[i]>'9'){
            b2=i;
            break;
        }
    }
    for(let i=a2; i<a.length; i++){
        if(a[i]==','){
            a3=i;
            break;
        }
    }
    for(let i=b2; i<b.length; i++){
        if(b[i]==','){
            b3=i;
            break;
        }
    }
    let ha=a.slice(0,a1).toUpperCase();
    let na=Number(a.slice(a1,a2));
    let sa=Number(a.slice(a3+1))
    let hb=b.slice(0,b1).toUpperCase();
    let nb=Number(b.slice(b1,b2));
    let sb=Number(b.slice(b3+1));
    if(ha<hb) return -1;
    else if(ha>hb) return 1;
    else{
        if(na<nb) return -1;
        else if(na>nb) return 1;
        else{
            if(sa<sb) return -1;
            else return 1;
        }
    }
}

function solution(files) {
    var answer = [];
    for(let i=0; i<files.length; i++) files[i]+=','+i;
    files.sort(base)
    for(let i=0; i<files.length; i++){
        for(let j=0; j<files[i].length; j++){
            if(files[i][j]==',') files[i]=files[i].slice(0, j);
        }
    }
    return files;
}
```



## 압축

### 코드

```javascript
function solution(msg) {
    var answer = [];
    var alphabet= ' ABCDEFGHIJKLMNOPQRSTUVWXYZ'
    var index=alphabet.split('');
    for(var i=0; i<msg.length; i++){
        var key=-1;
        var sw=1;
        while(sw!==-1 && i+key<msg.length){
            key++;
            sw=index.findIndex(e=>e===(msg.slice(i,i+key+1)))
        }
        answer.push(index.findIndex(e=>e===(msg.slice(i,i+key))))
        index.push(msg.slice(i,i+key+1));
        i=i+key-1;
    }
    return answer;
}
```



## 방금그곡

### 코드

```python
from datetime import datetime
def solution(m, musicinfos):
    change = [['C#','D#','F#','G#','A#'],['c','d','f','g','a']]
    answer="(None)"
    mmax=-1;
    for i in range(5) :
        m=m.replace(change[0][i], change[1][i])
    for i in musicinfos :
        a=i.split(',');
        for j in range(5) :
            a[3]=a[3].replace(change[0][j], change[1][j])
        
        b=datetime.strptime(a[1],'%H:%M')-datetime.strptime(a[0],'%H:%M')
        c=int(b.total_seconds()/60)
        
        jae=''
        
        for j in range(c) :
            jae+=a[3][j%len(a[3])]
            
        if not jae.find(m) == -1 and mmax<c :
            mmax=c
            answer=a[2]
            
    return answer
```



## 후보키

### 알고리즘

1. column을 BFS로 탐색 하며 key로 삼을 수 있는 지 검사한다.
2. key로 삼을 수 있는 것은 set 자료 구조를 이용하여 chkchk에 저장한다.
3. chkchk에 저장되어 있는 것을 부분집합으로 하지 않는 key만을 후보키로 삼는다.

### 코드

```python
import queue
b=[]
chkchk=[]

def chk(a) :
    global b
    dic = {}
    for i in range(len(a)) :
        mun = ''
        for j in range(len(b)) :
            mun = mun + ',' + a[i][b[j]]
        if mun in dic : return False
        else : dic[mun]=1
    return True

def solution(relation):
    answer = 0
    global b, chkchk
    q=queue.Queue()
    
    for i in range(len(relation[0])) : q.put([i])
        
    while not q.empty() :
        b=q.get()
        if chk(relation) :
            t=0
            for j in range(len(chkchk)) :
                if chkchk[j]<=set(b) : t=1
            if t==0 :
                answer+=1
                chkchk.append(set(b))
        else :
            for i in range(b[-1]+1, len(relation[0])) : q.put(b+[i])
    return answer
```


# K번쨰수

### 알고리즘

1. 배열의 깊은 복사를 한 후 정렬한다. k번째의 값을 참조하여 answer 배열에 넣어주면 답을 구할 수 있다.



### 코드

```javascript
function compareNumbers(a, b) {
  return a - b;
}
function solution(array, commands) {
    var answer = [];
    let imsi;
    for(let i=0; i<commands.length; i++){
        imsi=Object.assign([],array.slice(commands[i][0]-1,commands[i][1]));
        imsi.sort(compareNumbers);
        answer.push(imsi[commands[i][2]-1]);
    }
    return answer;
}
```


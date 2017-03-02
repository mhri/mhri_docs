Dialog에서 Motion을 사용할 경우 다음과 같이 사용할 수 있습니다.


### 간단 예제
```
Example:
  - <log=로그할 아이템>
```

### 다중아이템 로그
```
Example:
  - <log=로그아이템1/로그아이템2/...>
```

### rivescript 내부변수
```
Example:
  - <log=현재유저이름은 <get name>입니다.>
```

### social memory 값 호출
```
Example:
  - <log=메모리값불러오기 <call>mget speech_recognition/confidence</call>>
```

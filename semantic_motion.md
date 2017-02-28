Dialog에서 Motion을 사용할 경우 다음과 같이 사용할 수 있습니다.

### Tag를 이용하는 경우

로봇의 여러가지 제스쳐를 Semantic Motion으로 분류하였습니다. Tag는 다음과 같은 종류로 구성되어 있습니다.
사용자가 특정 Tag를 지정하지 않는 경우, Neutral를 기본값으로 사용합니다.
```
Example:
    - <sm=tag:[tag_name]> 안녕하세요.
```

Supported Tag Name
```
    neutral
    encourage
    attention
    consolation
    greeting
    waiting
    advice
    praise
    command
```


### 제스쳐 이름을 이용하는 경우

로봇에 저장된 특정 모션을 사용하는 경우, 다음과 같이 사용할 수 있습니다.

```
Example:
    - <sm=name:[motion_name]> 안녕하세요.
```

motion_name은 로봇에 저장된 모션 이름을 사용하면 됩니다.


### Coordinate Motion을 사용하는 경우

로봇이 특정 위치나 특정 사람을 바라보는 경우 (gaze), 팔을 이용해 가르키는 경우 (pointing)에는 다음과 같이 사용할 수 있습니다.

```
Example:
    - <gaze=[person_name]> 사용자님. 안녕하세요?
    - <pointing=[object_name or person_name] 저 물체를 바라봐 주세요.
```

person_name과 object_name은 대화 이전에 Social Memory에 정보(위치)가 기록되어 있어야 합니다. 

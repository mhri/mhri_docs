# 소셜 이벤트를 이용한 HRI 스크립트 개발 매뉴얼

**작성자**: 장민수 (minsu@etri.re.kr)

**라이센스**: 미정

## 소개
이 튜토리얼은 sHRI(social Human-Robot Interaction)의 소셜 이벤트의 구조와 기능을 설명하고 각 이벤트의 활용 사례를 보인다.
sHRI 를 이용한 로봇 응용 개발을 위한 가이드를 제공하기 위함이다.

## 이벤트 목록
sHRI 프레임워크가 생성하는 소셜 이벤트의 목록은 아래와 같다. 
이벤트 발생 상황은 로봇의 각종 센서를 통해 해당 상황이 검출되거나 인식된 상황을 말한다. 
예를 들어, 카메라의 시야각 안에 사람의 얼굴이 등장하고 프레임워크의 얼굴검출기가 얼굴의 위치를 검출하면 `face-detected` 이벤트가 발생한다.

| 상호작용 상황 | 이벤트 이름 | 이벤트 발생 상황 |
| --------- | --------- | ------------ |
| 얼굴 등장    | `face-detected` | 새로운 얼굴이 등장했을 때 |
| 얼굴 사라짐  | `face-disappeared` | 지속적으로 검출되던 얼굴이 사라졌을 때 |
| 사회적 거리 상태 | `person-entered-zone` | 카메라와 사람의 거리가 변화했을 때 |
| 사람의 근접 상태 | `person-approached` | 사람이 카메라 방향으로 가까워졌을 때 |
| 사람의 근접 상태 | `person-left` | 사람이 카메라로부터 멀어졌을 때 |
| 신원 인식 | `face-recognized` | 사람의 신원을 알아냈을 때, 또는 인식 결과가 변화했을 때 |
| 사람의 관심 | `person-engaged` | 정면 얼굴이 일정 시간 이상 연속 검출되었을 때 |
| 사람의 무관심 | `person-disengaged` | 관심 상태에 있던 사람이 다른 곳을 일정 시간 이상 바라봤을 때 |
| 사용자 발화 시작 (입술 움직임) | `lip-talking-started` | 입술 움직임으로부터 발화 시작 시점이 감지되었을 때 |
| 사용자 발화 종료 (입술 움직임) | `lip-talking-finished` | 입술 움직임으로부터 발화 종료 시점이 감지되었을 때 |

## 이벤트 활용 가이드

### 사람의 등장: `person-appeared` 이벤트

#### 소개
sHRI 는 RGB-D 카메라로 들어온 영상으로부터 사람을 검출하고 추적하여 사람의 등장과 퇴장을 인식한다.
sHRI 는 시야에 존재하지 않던 새로운 사람을 발견하면 `person-appeared` 이벤트를 생성한다.
예를 들어, 사람이 등장했을 때 인사말을 건네는 시나리오는 아래와 같이 구현할 수 있다.

```
+ e:person-appeared
- 안녕하세요, 반가워요!
```

`person-appeared` 이벤트는 카메라 시야 안에 존재하지 않는 새로운 사람이 등장할 때 마다 발생한다.
아무도 없는 상황에서 한 사람이 등장하면 이벤트가 한 번 발생하고, 한 사람이 있는 상황에서 다른 사람이 등장하면 다시 한번 이벤트가 발생한다.
새로운 두 사람이 등장하면 이벤트가 두 번 발생한다.
다만, 사람 검출 속도가 느린 경우 복수의 사람이 등장해도 이벤트가 한 번 발생할 수도 있다.

#### 이벤트 데이터
sHRI 프레임워크의 소셜 이벤트는 발생한 상황에 관련한 정보를 이벤트 데이터로 제공한다.
이벤트 데이터는 이벤트 카테고리 이름과 데이터 필드 이름의 조합으로 접근할 수 있다.
`person-appeared` 이벤트의 카테고리 이름은 `person_detection` 이고, 이벤트 데이터 필드는 `count` 와 `person_id` 이다. 
`count` 는 새로 등장한 사람의 숫자를 나타내는 정수 데이터이고, `person_id` 는 새로 등장한 사람의 아이디 목록이다.

`person-appeared` 이벤트의 이벤트 데이터를 읽기 위해서는 sHRI 스크립트의 `mget` 태그를 활용해야 한다.
한 사람이 새로 등장하여 검출한 경우 그 사람의 아이디를 읽어서 `pid` 변수에 저장하는 스크립트는 아래와 같다.

```
+ e:person-appeared
- 반갑습니다! <set pid=<mget person_detection/person_id[0]>>
```

`person-appeared` 이벤트가 발생한 시점에 몇 명이 등장했는지 알고 싶으면 다음과 같이 값을 읽어온다.

```
+ e:person-appeared
- 반갑습니다! 등장하신 사람의 숫자는 <mget person_detection/count>이네요.
```

#### 이벤트 발생 시점에 대한 주의 사항
앞서 설명한 바와 같이 `person-appeared` 이벤트는 새로운 사람이 등장한 순간에 발생한다.
만약 사람이 이미 카메라 시야각 안에 존재하는 경우 이벤트는 발생하지 않는다.
예를 들어, 사람이 카메라 앞에 서 있는 상태에서 sHRI 프레임워크를 구동한 경우 소셜 이벤트를 처리하는 스크립트가 로딩되기 전에 이벤트가 이미 발생해버렸을 수가 있다.
따라서, 해당 이벤트를 꼭 사용해야 하는 경우 사람이 존재하지 않는 상태에서 sHRI 프레임워크를 완전히 구동한 후 사람이 카메라 시야각 안으로 들어와야 한다.

다른 이벤트의 경우에도 상태 변화가 발생한 시점에만 이벤트가 발생하므로 이 점을 고려하여 스크립트를 작성할 필요가 있다.

### 사람의 퇴장: `person-disappeared` 이벤트
카메라 시야각 안에 존재하던 사람이 시야각 밖으로 벗어나 더 이상 검출되지 않으면 `person-disappeared` 이벤트가 발생한다.

`person-disappeared` 이벤트 데이터는 `person-appeared` 이벤트와 동일하다. 단, `person_id` 필드는 사라진 사람의 아이디 목록, `count` 는 사라진 사람의 숫자를 가리킨다.

### 사람 수 변화: `number-of-guests-changed` 이벤트

#### 소개
카메라 시야각 안에 존재하는 사람의 숫자가 변하면 `number-of-guests-changed` 이벤트가 발생한다.
`number-of-guests-changed` 의 이벤트 카테고리는 people_detection 이다.

이벤트 데이터는 `difference` 와 `no_of_people` 로서 각각 새로 등장했거나 빠진 사람의 숫자와 변화 발생 뒤 남은 사람의 숫자이다.
예를 들어, 두 사람이 있다가 한 사람이 새로 등장했다면, `difference` 는 1, `no_of_people` 은 3 이다.
세 사람이 있다가 두 사람이 사라졌다면 `difference` 는 -2, `no_of_people` 은 1 이다.

#### 예제
만약 카메라 시야각 안에 있던 사람이 모두 사라진 경우 반응하려면 아래와 같이 이벤트를 활용하면 된다.

```
+ e: number-of-guests-changed
* <mget people_detection/no_of_people> == 0 => 모두 가셨네요.
```

두 명이 사라졌을 때 반응하려면 다음과 같이 이벤트를 활용하면 된다.

```
+ e: number-of-guests-changed
* <mget people_detection/difference> == -2 => 두 분이 가셨네요.
```

### 얼굴 인식: `face-recognized` 이벤트

#### 소개
카메라 시야각 안에 새로운 사람이 등장하고 얼굴이 일정 시간 카메라 정면을 바라봐서 얼굴이 인식된 경우 `face-recognized` 이벤트가 발생한다.
`face-recognized` 이벤트의 카테고리는 `face_recognition` 이다.
얼굴이 인식되는 시간은 sHRI 프레임워크를 실행하는 컴퓨터의 계산 속도에 따라 좌우되나, 대체적으로 5 초 이내에 인식이 완료된다.

#### 이벤트 데이터
`face-recognized` 이벤트 데이터는 인식한 사람에 대한 정보의 목록들로 구성된다. 
구성 목록은 `face_id`, `person_id`, `name`, `confidence` 로서, `face_id[0]`은 인식한 첫번째 사람의 얼굴 아이디, 
`person_id[0]`은 인식한 첫번째 사람의 사람 아이디, `name[0]`은 인식한 첫번째 사람의 실제 이름, `confidence[0]`는 인식한 첫번째 사람의 얼굴 인식 신뢰도로서 0 ~ 1 사이의 값이다.

얼굴 아이디(face_id)는 사람이 카메라 시야각에 등장했을 때 부여된 임의의 아이디로서 그 사람이 사라질 때까지 변함없이 유지되는 아이디를 말한다. 
`face-recognized` 이벤트의 `face_id` 값은 `person-appeared` 이벤트의 `person_id` 와 동일한 의미를 갖는 값이다.

사람 아이디(`person_id`)는 인식된 고유 신원에 부여한 아이디를 가리킨다. 
sHRI 프레임워크는 사람의 얼굴에 그 사람을 구별하는 고유 아이디를 부여하는데 이것이 바로 `person_id` 데이터 값에 저장된다. 
(즉, `face-recognized` 이벤트의 `person_id` 필드와 `person-appeared` 이벤트의 `person_id` 필드는 서로 다른 의미를 갖는다.)

종합하여 이벤트 데이터의 의미를 설명하면 다음과 같다. 
카메라 시야각 안에 사람이 없는 상태에서 sHRI 프레임워크가 알고 있는 사람이 등장했을 때 발생하는 `person-appeared` 와 `face-recognized` 이벤트의 데이터 값의 예를 다음과 같이 들 수 있다.

| 이벤트 | 이벤트 데이터 |
| ---- | ---------- |
| `person-appeared` | `person_id = [‘p20170102_1353’, ‘p20170102_1354’]` |
| `face-recognized` | `face_id = [‘p20170102_1353’]`<br> `person_id = [‘user012312’]`<br> `name = [‘김남득’]`<br> `confidence = [0.879]`  |

이벤트 데이터의 값을 해석해 보면, 현재 카메라 시야각 안에는 두 사람이 존재하며 각 사람에게는 `p20170102_1353` 과 `p20170102_1354` 라는 아이디가 부여되어 있다.
그 중 아이디가 `p20170102_1353` 인 사람의 얼굴을 인식하여 신원을 파악하였으며 그 사람의 고유 아이디는 `user012312` 이고, 이름은 김남득, 얼굴 인식의 신뢰도는 87.9%임을 알 수 있다.

얼굴 인식 결과 초면인 경우 `person_id` 의 값은 `‘unknown’`을 갖는다.

아는 사람이면 이름을 호칭하면서 인사하고, 처음 본 사람에게는 초면 인사를 하는 스크립트 예제는 다음과 같다.

```
+ e:face-recognized
* <mget face_recognition/person_id[0]> == unknown => 처음 뵙네요! 
* <mget face_recognition/person_id[0]> != unknown => <mget face_recognition/name[0]>님, 다시 뵙네요. 반가워요!
```

### 표정인식: `face-expression-detected` 이벤트

#### 소개
얼굴 표정을 인식한 결과를 이벤트로 제공한다. 현재 표정 인식은 미소를 짓는지 여부를 판정하는 기능만을 수행한다.
따라서, 미소를 짓기 시작하는 시점과 종료하는 시점에만 이벤트가 발생한다. 이벤트 카테고리는 `face_expression_recognition` 이다.

#### 이벤트 데이터
이벤트 데이터는 `face_id`, `emotion_id`, `emotion_confidence` 로서 각각 카메라 시야각 안에서 검출한 사람의 아이디, 미소 여부를 가리키는 이진값 (0: 무표정, 1: 미소), 
그리고 표정 인식의 신뢰도를 나타내는 0 ~ 1 사이의 실수값이다.

예를 들어, 카메라 시야각에 한 사람이 존재하고 있고 그 사람의 얼굴에서 미소가 검출된 경우 반응하는 스크립트 예제는 다음과 같다.

```
+ e:face-expression-detected
* <mget face_expression_recognition/emotion_id[0]> == 1 => 미소가 예뻐요
```

### 집중여부판단: `person-engaged,person-disengaged` 이벤트

#### 소개
사용자가 로봇에게 집중하는지 여부를 판단하여 그 결과를 제공하는 이벤트이다.
사람이 로봇에게 집중하는지 여부는 사람의 얼굴 방향을 기준으로 판정한다. 
일정 시간 이상 카메라를 정면으로 바라보면 집중하고 있는 상태로 간주한다. 
카메라는 로봇의 바로 옆 또는 뒤에 배치되어 있어야 한다.

이 두 이벤트는 사용자의 집중 상태가 변화한 시점에만 발생한다. 
사용자가 집중하기 시작한 시점에 `person-engaged` 이벤트가 발생하고, 집중하지 않기 시작한 시점, 즉 집중 상태가 종료한 시점에 `person-disengaged` 이벤트가 발생한다.

`person-engaged` 이벤트의 카테고리는 `engagement_detection` 이고, `person-disengaged` 이벤트의 카테고리는 `disengagement_detection` 이다.

#### 이벤트 데이터
두 이벤트의 이벤트 데이터는 `face_id` 와 `count` 로 동일하다. 
`face_id` 는 집중하고 있거나 집중하고 있지 않은 사람의 아이디 목록이고, `count` 는 `face_id` 에 저장된 아이디의 개수를 나타낸다.

카메라 시야각 안에 한 사람이 존재하고 그 사람이 로봇에게 집중하기 시작했을 때 말을 건네려면 다음과 같이 스크립트를 적으면 된다.

```
+ e:person-engaged
* 제게 집중해 주시니 고맙습니다.
```

### 립리딩 기반 발화 여부 검출: `lip-talking-started`,`lip-talking-finished` 이벤트

#### 소개
입술의 움직임을 검출하여 사용자의 발화 여부를 알려주는 이벤트이다. 
사용자가 말하기 시작했다고 판단한 시점에 `lip-talking-started` 이벤트가 발생하고, 말하기를 중단했다고 판단한 시점에 `lip-talking-finished` 이벤트가 발생한다.

이 두 이벤트의 카테고리는 `vad_by_lip_reading` 이다.

#### 이벤트 데이터
이 두 이벤트의 이벤트 데이터는 `face_id` 만을 포함한다.
`face_id` 는 발화를 시작했거나 종료한 사람의 아이디이다.

카메라 시야각 안에 한 사람이 존재하고 그 사람이 말하지 않는 경우에만 로봇이 말하도록 하는 스크립트 예제는 다음과 같다.

```
+ 안녕!
- {topic=check}

> topic check
+ e:lip-talking-finished
- {topic=continue} {@init} < topic
> topic continue + init
- 반갑습니다!
< topic
```

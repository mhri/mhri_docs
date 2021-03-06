현재 Social HRI software Framework에서 지원하는 Social Event 및 Social Data를 정리한 문서입니다.
이벤트 종류와 상관없이 Default 값이 Bool Type (true, false)인 경우 Social Event로 사용 가능합니다.
그외의 경우는 Social Memory에 저장된 값을 읽거나 쓸수 있습니다.

### 터치 관련
```
touch_activity:
    head_touched: false
    l_hand_touched: false
    r_hand_touched: false
    touched_part: ''
```

### 발화 관련
 ```
voice_activity:
    start_speech: false
    end_speech: false
    silency_detected: false
    silency_time: 0
```

### 음성 인식
```
speech_recognition:
    speed_recognized: false
    recognized_word: ''
    confidence: 0
```

### 라우드 사운드 검출
```
sound_localization:
    loud_sound_detected: false
    direction_lr: ''
    direction_fb: ''
    transform: []
    frame_id: ''
```

### 사람 검출/추적
```
person_detection:
    person_appeared: false
    person_disappeared: false
    person_id: []
    count: 0
```

### 얼굴 검출
```
face_detection:
    face_detected: false
    face_disappeared: false
    face_id: []
    count: 0
```

### 얼굴 인식
```
face_recognition:
    face_recognized: false
    face_id: []
    person_id: []
    name: []
    confidence: []
    count: 0
```

### 표정 인식
```
facial_expression_recognition:
    faciel_expression_detected: false
    face_id: ''
    emotion_id: ''
    emotion_confidence: 0
```

### Person Identification Event
##### saveral fields for personal trait information
```
person_identification:
    person_identified: false
    session_face_id: []
    person_id: []
    name: []
    face_pos: []
    confidence: []
    gender: []
    cloth_color: []
    eyeglasses: []
    height: []
    hair_style: []
    emotion: []         # 'happy', 'unhappy'
```

### Engagement Detection
```
engagement_detection:
    person_engaged: false
    face_id: []
    count: 0
```

### Disengagement Detection
```
disengagement_detection:
    person_disengaged: false
    face_id: []
    count: 0
```

### 입술 움직임으로부터 판단한 발화 여부
```
vad_by_lip_reading:
    lip_talking_started: false
    lip_talking_finished: false
    face_id: ''
```

### 의도인식
```
intention_recognition:
    intention_affirmation: false
    intention_negation: false
    intention_concentration: false
    face_id: ''
```

### 사람 인식
```
people_detection:
    number_of_guests_changed: false
    difference: 0
    no_of_people: -1
```

```
pointing_gesture_recognition:
    pointing_gesture_recognized: false
    face_id: ''
    gesture_id: ''
```

### Approach & Leave Status  
```
presence_detection:
    person_approached: false
    person_left: false
    face_id: []
    count: 0
```

### Proxemic Status
##### a list of face IDs with a corresponding zone ID  
```
proxemics_detection:
    person_entered_zone: false
    face_id: []
    zone_id: []
    count: 0
```

### Saliency Status
##### notify the most salient person
```
saliency_detection:
    saliency_changed: false
    face_id: ''
    saliency_value: 0
    face_index: -1
```

### 턴테이킹 관련
이제 니가 말할 차례야
니가 계속 말하렴.
아직 내 말이 끝나지 않았어
이제 내가 말할 차례야.
```
turn_taking:
    turn_yielding: 0
    back_channel: 0
    turn_maintaining: 0
    turn_requesting: 0
```

```
environment_detection:
    environment_detection: false
    ambient_temperature: 0
    relative_humidity: 0
```

```
data_type:
  # 위치 (기본데이터)
  transform:
    translation:
      x: 0.0
      y: 0.0
      z: 0.0
    rotation:
      r: 0.0
      p: 0.0
      y: 0.0
```

---
layout: post
categories: project
title:  "피파 온라인 API를 활용한 랭커 유저 분석"
excerpt: "파이썬을 활용해 피파 온라인 API를 사용해 보자"
tags: [Python, project, JSON, API, game data analysis]
use_math : true
date: 2020-12-30
last_modified_at: 2020-12-30
---

​    

# 다시 피파 API를 건드리는 이유

피파 온라인 API을 이용한 데이터 분석을 진행해서 블로그에 작성했는데, 생각보다 많은 관심을 받았다(감사합니다). 이게 대략 네 달전 이야기인데, 연말을 맞아 코드를 유지보수 하려 몇 개 건드려 봤더니 지금은 작동하지 않는 코드가 되어버렸다. 

이 글에서 소개하는 코드는 지금 글을 작성하고 있는 2020년 12월 말 기준으로 잘 작동하고 있다. 혹시나 작동에 문제가 있다면 글 하단의 disqus 댓글이나 [Github repository](https://github.com/euneestella/fifa-online-ranker-analysis)의 Issue를 통해 남겨 주시면 된다. 분석 내용에 관련한 코멘트나 개선 사항 혹은 오류 제보는 언제나 환영이니 많은 관심 부탁드린다.

​     

# 넥슨 개발자센터 톺아보기

피파 온라인 API는 [넥슨 개발자센터](https://developers.nexon.com/fifaonline4/apiList)에서 제공하고 있다. 이 글에 관심 있는 독자라면 응당(!) 넥슨 아이디 하나 가지고 있으실 테니, 로그인 후 마이페이지에 들어가 보자.

## API Key 발급받기

![마이페이지](/images/fifa-online-project/fifa-online-mypage.PNG)

내 어플리케이션 목록 오른쪽에 있는 '새 어플리케이션 등록' 버튼을 누르고 API Key를 발급받으면 된다. <u>게임</u>은 EA SPORTS™ FIFA 온라인 4를 선택하고, <u>API Key 타입</u>은 개발을 선택하자.

​    

## 이용 가이드

맨바닥에 헤딩하듯 프로젝트를 시작할 때 공식 문서가 많은 도움이 되었다. 특히 API 응답코드 표는 내 코드가 어디에서 에러를 일으켰는지 확인해 볼 수 있는 지표이니, 개발하다 문제가 생길 때마다 돌아와서 읽으면 좋다.

![에러 코드](/images/fifa-online-project/fifa-online-error-code.PNG)

​    

#  TOP 10,000 랭커의 선수 운용 분석

상위 10,000등 안에 드는 랭커들은 <u>상위 선수를 어떤 방식으로 운용하는지</u> 분석하려고 한다. 참고로 분석에 사용한 API는 [링크](https://developers.nexon.com/fifaonline4/api/11/22)를 클릭하여 볼 수 있다. 

문서에 적혀 있는 API 정보를 읽어보면 이렇게 요약할 수 있다. 

- 기능 : 상위 10,000 랭커 유저가 사용한 선수의 20경기 평균 스탯 조회

- 입력 : 1) 선수의 고유 식별자와 포지션 목록을 `{players}` 파라미터로 전송 2) 매치 종류를 `{matchtype}` 파라미터로 전송

  - `{players}` : "id", "po"필드를 가지고 있는 Json Object Array

    - id : 선수 고유 식별자 (spid, /metadata/spid API 참고) 

    - po : 선수 포지션 (spposition, /metadata/spposition API 참고)

- 주의사항 : 한번에 50명 내외의 선수를 호출할 것, 너무 많은 선수목록을 입력하면 에러 반환 가능

​    

## 모듈 설치

```python
import json
import pandas as pd
import requests

from pandas import DataFrame
from tqdm.notebook import tqdm
```

Json Object Array를 이용하기 위해 `json` 모듈, 호출을 위한 `requests` 모듈, 그리고 데이터프레임을 활용하기 위해 `pandas` 모듈을 임포트했다. `tqdm` 모듈 임포트는 선택인데, 나는 분석에 걸리는 시간을 시각화해서 보고 싶어서(답답해서) 임포트하여 함께 사용했다.

이제 넥슨 개발자센터에서 발급받은 API Key를 입력해 두자

```python
# 넥슨 개발자센터에서 발급받은 키를 입력하세요
api_key = 'your_api_key'
```

​       

## 매치 종류

우선 `{matchtype}` 파라미터에 전송할 매치 종류부터 살펴보자. 매치 종류의 메타데이터는 아래의 코드로 조회할 수 있고, 이 글에서는 공식경기 결과를 분석에 사용하도록 하겠다.

```python
match_url = requests.get('https://static.api.nexon.co.kr/fifaonline4/latest/matchtype.json')
match_parsed_data = match_url.json()
match_type = pd.DataFrame(match_parsed_data)
```

|      | matchtype | desc        |
| ---- | --------- | ----------- |
| 0    | 30        | 리그 친선   |
| 1    | 40        | 클래식 1on1 |
| 2    | 50        | 공식경기    |
| 3    | 52        | 감독모드    |
| 4    | 60        | 공식 친선   |

​    

## 선수 고유 식별자

선수 고유 식별자는 시즌아이디 3자리와 선수아이디 6자리로 구성되어 있다. 선수 고유 식별자는 다음의 코드로 조회할 수 있다. 

```python
spId_url = requests.get('https://static.api.nexon.co.kr/fifaonline4/latest/spid.json')
spId_parsed_data = spId_url.json()
spId = pd.DataFrame(spId_parsed_data)
```



[EA Sports](https://www.ea.com/ko-kr/games/fifa/fifa-20/ratings/fifa-20-player-ratings-top-100)에서 제공하는 FIFA 20 선수 등급 상 상위 세 선수인 메시, 호날두, 네이마르를 분석해 보자. 각 선수의 선수 고유 식별자는 다음과 같다. 

| 메시           | 호날두         | 네이마르      |
| -------------- | -------------- | ------------- |
| id : 206158023 | id : 206020801 | id : 20719087 |

​     

## 선수 포지션

선수 포지션의 메타데이터도 다음의 코드로 조회할 수 있다.

```python
spposition_url = requests.get('https://static.api.nexon.co.kr/fifaonline4/latest/spposition.json')
spposition_parsed_data = spposition_url.json()
spposition = pd.DataFrame(spposition_parsed_data)
```

0번 GK(골키퍼)부터 28 SUB(후보선수)까지 총 29개의 플레이어블 포지션이 존재한다.

​        

## 조회할 선수 목록 만들기

```python
messi_positions = json.dumps([{"id":206158023, "po":x} for x in positions])
ronaldo_positions = json.dumps([{"id":206020801, "po":x} for x in positions])
neymar_positions = json.dumps([{"id":207190871, "po":x} for x in positions])
```

`json.dumps`를 이용하여 JSON 데이터 형태를 다룰 수 있도록 코드를 수정했다.  위의 코드는 메시, 호날두, 네이마르에 대해 상위 10,000등 랭커들이 <u>어떤 포지션으로 운용하였는지</u> 알기 위해 작성했다. 

​      

## API 호출하기

```python
headers = {'Authorization' : api_key}
jsonurl = "https://api.nexon.co.kr/fifaonline4/v1.0/rankers/status?matchtype=50&players="

try :
    for i in tqdm(range(len(positions))):
        response = requests.get(jsonurl+messi_positions, headers = headers)
        json = response.json()
        messi_ranker = pd.json_normalize(json)

except Exception as e :
```

`jsonurl`에 우리가 만든 JSON 타입의 데이터를 입력하여 호출한 다음, 응답값을 다루기 쉽게 데이터프레임으로 만들어 오는 코드이다.

![메시](/images/fifa-online-project/fifa-online-messi-request.PNG)

사진에서는 짤렸지만, `messi_ranker.head()` 명령어를 통해 데이터가 잘 들어온 것을 확인할 수 있다. 나머지 선수들에 대해서도 같은 방식으로 작업을 수행하면 된다.

​    

## 데이터 가공

### 평균 패스 성공률, 평균 드리블 성공률 만들기

평균 패스 성공률을 나타내는 `passRate`, 평균 드리블 성공률을 나타내는 `dribbleRate`를 만들자.

```python
messi_ranker['passRate'] = messi_ranker['status.passSuccess']/messi_ranker['status.passTry']
ronaldo_ranker['passRate'] = ronaldo_ranker['status.passSuccess']/ronaldo_ranker['status.passTry']
neymar_ranker['passRate'] = neymar_ranker['status.passSuccess']/neymar_ranker['status.passTry']

messi_ranker['dribbleRate'] = messi_ranker['status.dribbleSuccess']/messi_ranker['status.dribbleTry']
ronaldo_ranker['dribbleRate'] = ronaldo_ranker['status.dribbleSuccess']/ronaldo_ranker['status.dribbleTry']
neymar_ranker['dribbleRate'] = neymar_ranker['status.dribbleSuccess']/neymar_ranker['status.dribbleTry']
```

​     

### 분석에 사용하지 않을 컬럼 삭제하기

 ```python
del messi_ranker['createDate']
del ronaldo_ranker['createDate']
del neymar_ranker['createDate']
 ```

​      

### 컬럼 이름 쉽게 변경하기

`status.shoot`처럼 모든 컬럼 이름에 `status`가 붙어 있어 매번 컬럼 이름을 입력하기 번거로운 감이 있다. `status`를 떼고 이름을 새롭게 붙여주자.

```python

column_list = ['spId', 'spPosition','shoot', 'effectiveShoot', 'assist', 'goal',
              'dribble', 'dribbleTry', 'dribbleSuccess', 'passTry', 'passSuccess',
              'block', 'tackle', 'mathchCount', 'passRate', 'dribbleRate']

messi_ranker.columns = column_list
ronaldo_ranker.columns = column_list
neymar_ranker.columns = column_list
```

​     

## 저장하기

```python
messi_ranker.to_csv('messi.csv')
ronaldo_ranker.to_csv('ronaldo.csv')
neymar_ranker.to_csv('neymar.csv')
```

데이터의 양이 크지 않아 `csv` 포맷으로 저장했다. 만약 다루는 데이터가 매우 많다면 `pickle` 포맷 등 다른 데이터 포맷으로 저장하거나 각 데이터 타입을 지정해 주면 되겠다. 

​      

​     

분석에 사용한 코드 전문은 [링크](https://github.com/euneestella/fifa-online-ranker-analysis/blob/master/ranker_data_collecting.py)에서 확인할 수 있다.

Data based on NEXON DEVELOPERS
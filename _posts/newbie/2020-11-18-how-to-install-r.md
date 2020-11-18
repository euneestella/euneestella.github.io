---
layout: post
categories: newbie
title:  "뉴비를 위한 R 설치 방법"
excerpt: "프로그래밍 언어가 생소한 당신을 위한 뉴비 가이드"
tags: [R, newbie]
date: 2020-11-18
last_modified_at: 2020-11-18
---

이 글은 서울대학교 아시아연구소 [한국사회과학자료원(KOSSDA)](https://kossda.snu.ac.kr/) 방법론 교육 프로그램의 실습 강의를 위해 작성한 강의안을 일부 변형하였음을 알립니다.

​       

# 나도 뉴비였기 때문에

코딩은 정말 남의 일인 줄 알았다. 내가 해 본 코딩 비스무리한 건 ~~HTML~~로 홈페이지 좀 만들어 본 것 뿐 (?? : 저는 HTML로 프로그래밍 해요~)

여튼, 필자는 R과 파이썬을 대학에 들어와 처음 접했음을 밝힌다. 프로그래밍을 처음 접하면서 겪었던 이슈와 의문점들을 최대한 뉴비의 시각에서 소개하도록 하겠다.

​        

## R을 쓰려면 좋은 PC가 필요한가?

물론, 대용량 데이터를 분석하는 작업을 하려면 좋은 PC가 필요하다. 그러나 '웬만한' 사양의 컴퓨터로도 '웬만한' 수준의 작업을 할 수 있으니 너무 겁먹지 않아도 된다. 참고로 필자가 R을 돌리는 PC는 대략 이런 환경이다.

- Microsoft Windows 10 (64-bit)
- CPU : Intel i5-10400
- RAM : 8GB
- GPU : 내장 그래픽

​       



# R 설치

무엇이든지 공식 문서와 공식 홈페이지를 통해 정보를 얻는 습관을 기르자. [The R Project](https://www.r-project.org/)의 Download 탭에서 R을 다운로드받을 수 있다.

​     

# Console 출력 언어를 영어로 설정하기

구성 요소 설치 단계에서 [Message translations] 옵션을 해제하자. 해제하면 R Console의 출력 언어가 영어로 설치된다.

![install option](/images/newbie-r/r_install_option.png)

R을 사용하다 보면 각종 에러 메시지를 보게 될 텐데, 영어로 된 에러 메시지는 구글링으로 해결 방법을 찾기에 매우 유용하다.

​     

# 이미 설치해 버렸는데 언어를 바꾸고 싶은 경우

- [Edit] → [GUI Preferences..] → [Language for menus and messages]에서 [en]으로 바꾼 후 [Save] → [OK]
- [C:\Program Files\R\R-4.0.3\etc]로 이동 후 [Rconsole] 파일을 워드패드 등 편집기에서 열고 다음 부분을 찾아 수정 후 저장

```markdown
## Language for messages
language = en
```

```language = en``` 으로 설정해 준 다음, R을 종료 후 재실행하면 출력 언어가 영어로 변경되어 있을 것이다.

​     

## Rconsole 파일을 수정할 수 없는 경우

그런데 **<u>관리자 권한</u>**이 없는 상태에서 [Rconsole] 파일을 수정하려고 하면 '액세스가 거부되었습니다' 라는 팝업 메시지가 뜬다.

Rconsole 파일을 마우스 우클릭 하여 [속성] → [보안] → [사용 권한 편집] 탭에 접근한 다음, 현재 사용하고 있는 계정의 사용 권한에 [수정] 허용 체크박스를 클릭하면 된다.

![language setting](/images/newbie-r/language_setting.png)



그래도 한글로 출력된다면(혹은 Rstudio 같은 IDE에서는 한글로 출력된다면), 삭제 후 R을 재설치할 때 [Message translations] 옵션을 해제하고 진행하는 편이 속 편하다.






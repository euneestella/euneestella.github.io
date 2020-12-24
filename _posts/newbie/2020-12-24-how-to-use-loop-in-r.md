---
layout: post
categories: newbie
title:  "뉴비를 위한 R 반복문의 이해"
excerpt: "프로그래밍 언어가 생소한 당신을 위한 뉴비 가이드"
tags: [R, newbie]
use_math : true
date: 2020-12-24
last_modified_at: 2020-11-19
---

<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>


이 글은 서울대학교 아시아연구소 [한국사회과학자료원(KOSSDA)](https://kossda.snu.ac.kr/) 방법론 교육 프로그램의 실습 강의를 위해 작성한 강의안을 일부 변형하였음을 알립니다.



# 반복문

반복문은 매우 강력한 기능이자 동시에 프로그래밍을 배우는 사람들의 첫 고비라고 생각한다. 처음 R을 공부하기 시작했을 때 구구단을 출력하면서 고생하던 기억이 난다. 



<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Repeat_mark.png/1200px-Repeat_mark.png">

<center>source : https://ko.wikipedia.org/wiki/도돌이표</center>

​    

반복문의 기능(특히 for loop)은 악보의 <u>도돌이표</u>와 같다. 위의 사진에서 연주 순서는 A - B - C - D - E - D - E - F 인데, 도돌이표를 쓰지 않았더라면 매번 악보를 그리기 곤란했을 것 같다. 프로그래밍에서도 마찬가지인데, 특정한 조건을 만족할 때 일련의 작업을 반복하게 명령하는 것이 핵심이다. 



## for

가장 많이 사용하는 반복문으로, 보통 **반복횟수**를 정확히 아는 경우에 사용한다.

```r
for (i in data){

    i를 이용하여 반복할 문장

}
```

갑자기 `i`라는 요상한 녀석이 튀어나왔는데, 이를 반복 변수(iteration variable)이라고 부른다 (꼭 `i`가 아니어도 되고, `j`, `k` 등등 편하신 대로 작성하면 된다.) 팁이 있다면 반복 변수의 이름을 의미있게 지으면 코드를 다른 사람과 공유할 때 편리하다.

간단하게 구구단 3단을 출력해 보자.

```r
for (i in 1:9){
  result <- 3 * i
  print(result)
}

[1] 3
[1] 6
[1] 9
[1] 12
[1] 15
[1] 18
[1] 21
[1] 24
[1] 27
```

1부터 9까지의 integer `i`에 대해, `3*i`를 `result`로 저장한 다음 `result`을 출력하는 간단한 예제이다. 아! 참고로 반복문의 흐름 제어를 위해 산술 연산자 등을 활용할 수도 있다. 예를 들어 **1부터 30까지 짝수의 합**을 구하는 코드를 작성한다고 해 보자.

```r
res <- 0
for (i in 1:30){        ## 1부터 30까지 자연수 i에 대해
  if (i%%2 == 0)        ## 만약 i를 2로 나눈 나머지가 0이라면
    res <- res + i      ## res에 i를 더한다
}
```

   

### next와 break

**R의 object type**들로 벡터를 만들어 보자

```r
object_type <- c("vector", "matrix", "array", "list", "dataframe", "factor")
```

`object_type`의 요소들을 출력하되, `matrix`은 **빼고** 출력하는 반복문을 어떻게 만들 수 있을까? 

```
next
object_type <- c("vector", "matrix", "array", "list", "dataframe", "factor")

for (name in object_type){
  if (name == "matrix"){
    next
  }
  print(name)
}

[1] "vector"
[1] "array"
[1] "list"
[1] "dataframe"
[1] "factor"
```

- `object_type`의 각 요소를 `name`이라고 하자
- 만약 `name` 이 `matrix`와 같다면, 이하의 내용은 실행하지 않고 다시 위로 올라가 반복문을 이어서 실행한다.
- `matrix`를 제외한 모든 이름들이 출력된다.

```
break
object_type <- c("vector", "matrix", "array", "list", "dataframe", "factor")

for (name in object_type){
  if (name == "matrix"){
    break
  }
  print(name)
}

[1] "vector"
```

위의 코드와 비슷해 보이지만, `next`를 `break`로 바꾸어 주었다. 이 경우에는 어떤 차이가 생길까?

- `object_type`의 각 요소를 `name`이라고 하자
- 만약 `name`이 `matrix`와 같다면. 반복문을 강제 종료한다. (<u>반복문 자체가 종료</u>)

​    

`next`와 `break`은 비슷한 기능을 하는 것 같지만 `break`은 <u>반복문 자체를 종료</u>한다는 점이 중요하다.

​    

## while

지금까지 사용한 `for` loop은 반복할 횟수를 알고 있는 경우에 주로 사용했다. 그런데 '반복할 횟수는 잘 모르겠고, 일단 반복만 하면 안돼?' 처럼 반복횟수를 정확히 모르는 경우도 당연히 있기 마련이다. 이 경우에는 `while` loop을 사용하는데, 조건이 참인 경우 반복문은 작동하고 그렇지 않으면 작동하지 않는다.

```r
while (조건){
    
    조건이 참일 때 실행할 문장

}
```

그러면 `for` 보다 `while`을 이용하는 게 더 범용성 면에서 좋지 않을까? 하지만 나는 크게 두 가지 이유에서 `for`와 `while`을 구분해서 쓰는 걸 선호하는데

- 같은 작업을 수행할 때 `for` loop의 실행시간이 `while` loop보다 짧다.
- `while` loop에서 적절한 흐름 제어를 하지 않으면 무한 루프에 빠진다.

그래서 웬만하면 상황에 따라 `for`와 `while`을 구분해서 사용하려는 편이다. 

`while` loop이 어떻게 작동하는지 알아보기 위해 `while`을 이용해 구구단 3단을 출력해 보자.

```r
i<- 1
while (i<=9){
  result <- 3 * i
  print(result)
  i <- i + 1
}

[1] 3
[1] 6
[1] 9
[1] 12
[1] 15
[1] 18
[1] 21
[1] 24
[1] 27
```

여기서 i=1을 `while` <u>밖에서</u> 지정해 주었는데, 조건에 `i`가 <u>직접적으로 사용</u>되기 때문이다. `(i≤=9)`

처음에 이야기했던 '반복횟수를 모르는 경우'를 상상해 보자. 구구단이 몇단까지 있는지 모른다고 생각하고(3을 어디까지 곱해야 하는지 모른다고 해 보자) , 대신 `result`은 30보다 작아야 한다고 생각해 보자. 

```r
i<-1
while (T){
  result <- 3*i
  if (result >= 30) break
  print(result)
  i <- i + 1
}
```

- `i = 1`로 반복문 밖에서 미리 지정
- 반복 횟수를 모른다
- `3*i` : `3*i` 결과를 `result`로 저장한다
- 만약 `result`가 `30` 이상인 경우 반복문을 강제 종료한다
- `result`을 출력한다
- `i`에 `1`을 더한다
- 반복문의 맨 처음으로 다시 돌아간다

​      

## repeat

`while` loop에 주어진 조건이 T, 즉 `while(T)` 인 경우를 특별히 `repeat` 반복문이라 부른다. `while`과 마찬가지로  당연히 탈출 조건(`break`)를 만들어 주어야 무한 루프에 빠지는 불상사를 막을 수 있다.

```r
repeat{

    실행할 문장

}
```

`repeat`을 이용하여 방금 전에 `while(T)`로 만들었던 반복문을 아래와 같이 수정하여 작성할 수 있다.

```r
i<-1
repeat{
  result <- 3*i
  if (result >= 30) break
  print(result)
  i <- i + 1
}
```
---
layout: post
categories: newbie
title:  "뉴비를 위한 R 객체(object)의 이해"
excerpt: "프로그래밍 언어가 생소한 당신을 위한 뉴비 가이드"
tags: [R, newbie]
date: 2020-11-19
last_modified_at: 2020-11-19
---

<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>


이 글은 서울대학교 아시아연구소 [한국사회과학자료원(KOSSDA)](https://kossda.snu.ac.kr/) 방법론 교육 프로그램의 실습 강의를 위해 작성한 강의안을 일부 변형하였음을 알립니다.

​       

# Object

R에서 모든 것은 object다. 객체지향이니, 어려운 말은 우선 넣어두자. 간단히 말해 R이 다루는 **가장 기본 단위**라고 이해하면 된다.

- object에 `<-` 기호를 사용하여 value를 할당할 수 있다.

![object types](/images/newbie-r/object_types.png)

source : https://ryuhyun.tistory.com/14

object에 대해 소개하기 앞서 우선 element에 대해 짚고 오자.

​       

# Element

## element의 종류

- logical : TRUE와 FALSE의 값만 가지는 요소 (간단히 T와 F로 쓸 수도 있다)
- integer : 정수
- double : 유리수
- complex : 복소수
- character : 문자열
- raw : 기타

`typeof()` 명령어로 element type을 확인할 수 있다.

logical → character로 갈수록 많은 정보를 포함하고 있다. 정보가 적은 element와 많은 element가 동시에 있다면, 강제로 정보가 많은 element로 변하며, 이를 강제 변환(coercion)이라고 한다.

여기까지만 알아 두고, 다시 object 이야기로 돌아오자.

​     

## Object의 종류

핵심은 **vector**다! 먼저 머릿속에 넣고 시작하자.

​    

### ◼ Vector

![vector](/images/newbie-r/r_vector.png)

: **동일한 element**를 가진 요소로 이루어진 **1차원 object**로,  `c()` 명령어로 벡터를 만들 수 있다.

```r
vec_lo <- c(TRUE, FALSE, TRUE, TRUE) ## logical vector
vec_in <- c(1L, 2L, 3L, 4L) ## integer vector
vec_do <- c(1, 2, 3, 4) ## double vector
vec_ch <- c("1", "2", "3", "4") ## character vector
```

아까 정보와 적은 element와 많은 element가 동시에 있다면, 많은 element로 바뀐다고 말했는데, 예를 들어

```r
cb1 <- c(1,2,3,"4")
typeof(cb1)

> typeof(cb1)
[1] "character"
```

double과 character를 같은 벡터에 넣었더니, 더 많은 정보를 가지고 있는 character로 변했다.

vector에 서로 다른 element를 넣으면, 더 많은 정보를 갖고 있는 걸로 강제변환(coercion) 해서 **결과적으로 '동일한 특성을 가진 요소'로 이루어지도록** 하는 것이다.

🤔 : 그럼 vector끼리 새로운 vector를 만들 수 있을까?

```r
cb2 <- c(vec_lo, vec_in)

> cb2
[1] 1 0 1 1 1 2 3 4
```

vector가 잘 만들어진다. `vec_lo` 의 `TRUE`와 `FALSE`값이 각각 `1`과 `0`으로 바뀌었다는 점에 주목하자!

​    

### ◼ Matrix

![matrix](/images/newbie-r/r_matrix.png)

: **두개 이상의 벡터**로 만들어진 행렬로, 행과 열로 이루어진 **2D 데이터 형태**이다.

vector가 핵심! 이라고 강조했었는데, matrix만 보아도 알 수 있다. matrix는 vector로 만들어진다. `matrix()` 명령어로 행렬을 만들 수 있고, 이 때에는 input data와 함께 **column과 row**의 수를 함께 지정해 주어야 한다.

```r
basic_mt <- matrix(1:12, ncol = 3, nrow = 4)

> basic_mt
     [,1] [,2] [,3]
[1,]    1    5    9
[2,]    2    6   10
[3,]    3    7   11
[4,]    4    8   12
```

​       

**이미 존재하는 벡터**들로 행렬을 만들기 위해서는

- `cbind()` : 열을 기준으로
- `rbind()` : 행을 기준으로

두 명령어를 사용할 수 있다.

​      

vector는 동일한 특성을 가진 element들로만 이루어져 있으니까 matrix도 마찬가지일 것이다. logical vector와 integer vector로 matrix를 만들어 보자.

```r
mt1 <- cbind(vec_lo, vec_in)

> mt1
     vec_lo vec_in
[1,]      1      1
[2,]      0      2
[3,]      1      3
[4,]      1      4
```

예상했던 것처럼 matrix가 잘 만들어진다. 참고로 `rbind`를 하면 이런 결과가 나오는데, 상황에 따라 `cbind`를 사용할지 `rbind`를 사용할지는 사용자가 판단하면 된다.

```r
> rbind(vec_lo, vec_in)
       [,1] [,2] [,3] [,4]
vec_lo    1    0    1    1
vec_in    1    2    3    4
```



​      

🤔 : R에서도 행과 열을 **전치**(transpose, 반전)할 수 있을까?

R에서는 `t()` 명령어로 행렬의 전치가 가능하며, 행렬의 다양한 연산도 가능하다.

- 행렬의 덧셈과 뺄셈 : `+`, `-`

- 행렬의 성분별 곱연산 : `*`

  $$mt1\quad =\quad \begin{bmatrix} 1 & 1 \\ 0 & 2 \\ 1 & 3 \\ 1 & 4 \end{bmatrix}$$의 각 성분별 연산이 가능할까? R에서는 `*` 연산자로 성분별 곱을 계산할 수 있다.

  ```r
  > mt1*mt1
       vec_lo vec_in
  [1,]      1      1
  [2,]      0      4
  [3,]      1      9
  [4,]      1     16
  ```

  ​      

- 행렬의 곱 : `%*%`

  $$\begin{bmatrix} 1 & 1 \\ 0 & 2 \\ 1 & 3 \\ 1 & 4 \end{bmatrix}\times \begin{bmatrix} 1 & 1 \\ 0 & 2 \\ 1 & 3 \\ 1 & 4 \end{bmatrix}$$은 불가능한 행렬 연산이다. 행렬곱은 $$(m \times n) \times (n \times r)$$ 형태일 때 $$m \times r$$ 크기 행렬로 나타낼 수 있기 때문인데, 행렬곱에 대한 자세한 설명은 [위키피디아](https://ko.wikipedia.org/wiki/%ED%96%89%EB%A0%AC_%EA%B3%B1%EC%85%88)를 참고하면 되겠다.

  ​     

  그러니 `t()` 명령어로 전치한 뒤 행렬의 곱을 연산할 수 있다.

  ```r
  > t(mt1)%*%mt1
         vec_lo vec_in
  vec_lo      3      8
  vec_in      8     30
  
  > mt1 %*% t(mt1)
       [,1] [,2] [,3] [,4]
  [1,]    2    2    4    5
  [2,]    2    4    6    8
  [3,]    4    6   10   13
  [4,]    5    8   13   17
  ```

  ​     

- 역행렬 : `solve()`

  역행렬은 **어떤 행렬 A와 곱했을 때 단위행렬 $$I$$**가 나오게 하는 행렬을 의미한다. 그런데 왜 inverse가 아니라 `solve()` 로 정의했을까?

  연립방정식 $$Ax=b$$ 이 주어졌을 때 해를 $$x={A}^{-1}b$$로 구할 수 있기 때문입니다. 즉, 역행렬을 구한다는 것은 <u>연립방정식의 해를 구하는 것</u>이기 때문에 `solve()` 도 자연스러운 네이밍이다.

  ```r
  mt2 <- matrix(1:4, nrow = 2, ncol = 2)
  imt2 <- solve(mt2)
  
  > imt2
       [,1] [,2]
  [1,]   -2  1.5
  [2,]    1 -0.5
  ```

​       

행렬에서 **행과 열의 이름**을 바꾸어 줄 수 있다.

- `rownames()` : 행의 이름을 바꾸는 함수

- `colnames()` : 열의 이름을 바꾸는 함수

    

아까 전에 만들었던 행렬 `mt2`를 확인해 보자.

```r
> mt2
     [,1] [,2]
[1,]    1    3
[2,]    2    4
```

행렬 `mt2`는 이미 존재하는 벡터들로 만든 행렬이 아니기 때문에, 임의로 행과 열의 이름이 붙여져 있다.

`[,1]`, `[,2]` 같은 이름은 아무런 의미가 없는데, 이름을 붙여 준다면 행렬을 더 잘 활용할 수 있겠다. 매번 이 column이 어떤 데이터들인지 일일히 코드북을 확인하지 않아도 되기 때문이다. `colnames()`함수를 이용하여 행렬의 열 이름을 `male`, `female`로 붙여보자.

```r
colnames(mt2) <- c("male", "female")
> mt2
     male female
[1,]    1      3
[2,]    2      4
```

​       

### ◼ Array

![matrix](/images/newbie-r/r_array.png)

: 동일한 **2D 데이터 구조**를 쌓아 올린 형태를 의미한다. 행렬이  2D 차원인 것이라면, array는 행렬들을 n개의 층으로(n 차원으로) 쌓아 올린 것이라고 이해하면 된다.

`array()` 명령어로 array를 만들 수 있는데, `array()`에는 input data 외에 추가적으로 차원(dimension)을 설정해 주어야 함에 유의하자.

```r
a1 <- array(1:12, c(2,3,2))

> a1
, , 1

     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

, , 2

     [,1] [,2] [,3]
[1,]    7    9   11
[2,]    8   10   12
```

위 예시는 `1:12`를 이용하여 행렬을 만들되, $$2\times3$$ matrix로, 2차원으로 만들라는 명령어이다.

​      

### ◼ factor

: **범주형 변수**의 레벨을 만들어 integer로 저장하여(dummy 변수화), 범주형 변수를 효율적으로 다루는 방법이다.

```r
vec_on <- c("하나", "둘", "셋", "둘")
vec_onf <- factor(vec_on)

> typeof(vec_onf)
[1] "integer"
```

🤔 : 그렇다면 `factor()` 명령어로 만들어진 벡터끼리 연산할 수 있을까?

```r
> vec_onf + vec_onf
[1] NA NA NA NA
Warning message:
In Ops.factor(vec_onf, vec_onf) : ‘+’ not meaningful for factors
```

`interger`처럼 보이지만 실제 연산을 할 수는 없다. 본질적으로 범주형 변수이기 때문!

​       

### ◼ list

![matrix](/images/newbie-r/r_list.png)

: 벡터와 유사하지만, **여러 자료형의 데이터**를 보존하면서 저장이 가능하다.

```r
vec <- c(v1,v2)
> vec
[1] "1"   "2"   "3"   "1"   "1.2" "2"
```

벡터에 서로 다른 자료형을 입력했을 때 오류가 발생하지는 않았다. 대신 적은 정보를 가지고 있는 element를 많은 정보를 가지고 있는 element로 강제 변환하여, **결론적으로 '같은 자료형의 element'**를 가지고 있게끔 만든다.

​       

반면 리스트는 벡터와 달리 **서로 다른 자료형**을 보존하면서 저장할 수 있다는 장점이 있다.

```r
## list
v1 <- c(1,2,3)      ## double
v2 <- c("1",1.2,2)  ## character
lst <- list(v1,v2)  ## both double and character

> lst
[[1]]
[1] 1 2 3

[[2]]
[1] "1"   "1.2" "2"
```

참고로 리스트에 이름(label)을 붙여 관리할 수 있다.

```r
lst2 <- list(double=v1, character=v2)

> lst2
$double
[1] 1 2 3

$character
[1] "1"   "1.2" "2"
```

`names()` 명령어로 리스트의 이름들을 확인할 수 있으며, 활용해서 **이름을 뒤늦게 붙여줄 수도 있다.**

```r
labeled_list <- list(c(1,2,3), c(10,20,30))
names(labeled_list) <- c("male", "female")

> labeled_list
$male
[1] 1 2 3

$female
[1] 10 20 30
```

​       

### ◼ data frame (✨ R에서 가장 빈번하게 사용)

![data frame](/images/newbie-r/r_dataframe.png)

: 각 **열이 서로 다른 데이터 타입**을 가질 수 있으나, **열 안에서 데이터 타입은 동일**해야 하며, **변수의 길이가 같아야** 한다는 조건이 있다(엑셀 시트를 떠올려 보자). 단, 하나의 객체 크기가 다른 객체 크기의 배수이면 recycle하여 생성한다.

`data.frame()` 명령어로 data frame을 만들 수 있다.

​         

그럼 이런 의문이 자연스럽게 들 수 있는데,

🤔 : list와 data frame 모두 서로 다른 데이터 타입을 보존하는데, 그럼 자유도가 더 높은 list를 쓰는 게 맞지 않은가?

맞는 이야기이지만, 실무에서 다루는 데이터의 특징을 생각해 보면 왜 data frame을 자주 사용하는지 이해할 수 있을 것이다. 통계 문제를 해결하기 위해서는 **데이터 간의 관계**가 필수적으로 고려되어야 하는데,  표 형태의 표현은 데이터 간의 관계를 매우 직관적으로 보여줄 수 있는 방법이기 때문이다. (왜 엑셀 시트를 많이 사용하는지 떠올려 보자)

![example of data frame](/images/newbie-r/r_dataframe_ex.png)

source : https://www.geeksforgeeks.org/dataframe-operations-in-r/

위의 예시를 이용해 데이터프레임을 만들어 보자.

```r
vec1 <- c("john", "terry", "evan")
vec2 <- c(10,20,30)
df <- data.frame(vec1, vec2)
```

vec1과 vec2 열로 데이터프레임을 만들었다. 그렇다면 행렬에서처럼 데이터프레임의 이름을 바꾸어 줄 수 있을까? `names()` 함수를 이용해 열의 이름을 바꾸어 줄 수 있다.

```r
names(df) <- c("name", "age")
df
```

엑셀을 비롯한 스프레드시트를 사용하실 때 열의 이름은 건드렸어도 행의 이름을 건드린 기억은 거의 없을 것이다. 데이터프레임을 이용할 때에도 행의 이름을 바꾸는 거의 없지만, `dimnames()` 함수를 이용해 행의 이름을 바꿀 수도 있다.

​      

### ◼ function

element와 object를 설명하는 과정에서 다양한 함수를 사용했다. R에서는 function 역시 object으로 취급한다.



​      

이렇게 function을 제외한 5가지(vector, matrix, array, list, data frame)가 R에서 주로 사용하는 object이다. object type별로 사용할 수 있는 함수가 다른 경우가 종종 있으니, object type(그 중에서도 특히 vector와 data frame)은 잘 숙지해 두는 것이 좋겠다.


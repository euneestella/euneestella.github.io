---
date: '2025-10-25'
draft: false
title: '내가 이해한 Flow Models (개념)'
description: 'ODE와 vector field'
math: true
tags:
  - generative-models
  - flow-models
categories:
  - tutorial
---

## 🔑 Key Ideas 
- Flow model의 목적은 ODE를 풀어, $p_{\text{init}}$ 을 $p_{\text{data}}$ 로 변화시키는 것이다.
- Flow는 데이터(의 확률분포)를 변화시키는 deterministic한 흐름이고, vector field에 의해 정의된다.
- 뉴럴넷의 학습 대상은 `vector field`다.

## 🧭 용어 정리
- Vector field : 각 데이터 포인트가 ① 어디로 ② 얼마나 이동해야 하는지
- Trajectory : vector field $u_t$로 정의된 데이터들의 경로로, ODE의 한 solution을 정의
- Flow : initial condition이 매우 많은 ODE 해들의 집합 (시간 $t$에 따른 공간 전체의 변형을 매핑)

개인적으로 헷갈렸던 부분은 `trajectory`와 `flow`의 차이였는데, 일반화 수준의 차이라고 이해하면 된다. 특정 한 지점으로부터의 경로는 `trajectory`, 이런 trajectory를 한번에 표현할 수 있는 매핑은 `flow`다.
### Vector field → ODE 정의 → trajectory를 얻는 과정
> Vector field $u_t$를 따르는 (follows along the lines) trajectory $X$를 얻자. ODE만으로는 여러 해가 나올 수 있으니, 시작점 $X_0=x_0$을 주고 유일한 trajectory를 결정해 보자.

$$
\frac{d}{dt} X_t = u_t(X_t) \quad \triangleright \text{ODE}
$$
$$
X_0 = x_0 \quad \triangleright \text{initial conditions}
$$

### Flow: trajectory의 일반화 (모든 시작점에 대한 trajectory들의 집합)
> ODE의 solution (=trajectory) 들을 <u>mapping function</u>을 이용해 한번에 표현해 보자.

$$
\psi : \mathbb{R}^d \times [0,1] \mapsto \mathbb{R}^d , \quad (x_0,t) \mapsto \psi_t(x_0)
$$

$$
\frac{d}{dt}\psi_t(x_0) = u_t(\psi_t(x_0)) \quad \triangleright \text{flow ODE}
$$

$$
\psi_0(x_0) = x_0 \quad \triangleright \text{flow initial conditions}
$$
여기서 flow ($\psi_t$)는 <u>모든 가능한 시작점</u> $x_0 \in \mathbb {R}^d$ 에 대해 <u>모든 가능한 시간</u> $t\in[0,1]$ 뒤 위치를 반환하는 매핑 함수다. 따라서 어떤 시작점 $X_0=x_0$이 주어졌을 때 flow를 통과시킨 $X_t=\psi_t(X_0)$는 자체로 한 trajectory를 복원할 수 있는 기능을 한다.

## 🎮 ODE의 샘플과정을 시뮬레이션해 보자
어떤 데이터 $x_0$에서 $x_1$로 갈 수 있는 최적의 vector field는 $u_t(x)=x_1-x_0$이다. 그렇다면 Euler method를 이용해서 손쉽게 ODE를 시뮬레이션할 수 있다. 이 방식으로 ODE를 시뮬레이션하기 위해 vector field와 initial condition과 함께 step 수가 필요하다.
- Vector field
- Initial condition
- Step $n$ 수 → step size $h=\frac{1}{n}$ 결정

<img src="assets/images/flow-model-algorithm.png">


## 📄 참고 자료
- Holderrieth, Peter and Ezra Erives. *Introduction to Flow Matching and Diffusion Models.*
  2025. [flowsanddiffusions2025](https://diffusion.csail.mit.edu/)

```bibtex
@misc{flowsanddiffusions2025,
  author       = {Peter Holderrieth and Ezra Erives},
  title        = {Introduction to Flow Matching and Diffusion Models},
  year         = {2025},
  url          = {https://diffusion.csail.mit.edu/}
}
```

- https://youtu.be/wFikV0Y7NIk?si=yCtoOCq_Hwmzfy3J

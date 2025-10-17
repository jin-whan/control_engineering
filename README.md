# control_engineering
### 이 rpository는,
* Remote repository의 생성
* Local로 Clone
* Local에서 수정
* Local에서 Commit
* Remote로 Push

### 를 연습하기 위해 만들어졌습니다.

# 5주차 1차시 강의 내용 요약

### State Variables  

이전 수업까지 system을 풀기 위해서 미분방정식을 이용해서 수학적인 모델을 세워왔습니다.  
이때 일종의 직관에 의존을 해서 물리적인 현상으로부터 수식으로 바로 유도를 했습니다.  
그러나 계산을 계속 손으로만 할 순 없습니다. 따라서 컴퓨터나 분업을 통해서 체계적인 문제풀이가 필요한데,  
이 체계를 위해서 State 라는 개념이 도입되었습니다.  
system 안에 뭔가 의미있는 부분을 state로 정의하고 이 state를 중심으로 문제를 푸는 것입니다.  
물론 state는 input에도 영향을 받고 state 끼리도 영향을 주고받을 것입니다.  
마지막으로 output도 input 뿐만아니라 state에까지 영향을 받을것입니다.  
그렇다면 어떻게 system을 state로 정의하는지 연습해보겠습니다.

EX1: spring-mass-damper system
---

<center>

![EX1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FV8pkg%2FbtsRacg8oLx%2FAAAAAAAAAAAAAAAAAAAAAB-w0tzIsDn8ZEzW2wKHDiObe2-tnRc01ZJntedRaoAi%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DroFtSZ5QN6AJKau6EFXRQoxImmU%253D)

</center>

$$M\frac{d^2y(t)}{dt^2}+b\frac{dy(t)}{dt}+ky(t)=r(t)$$

$$x_1(t)=y(t)$$ , $$x_2(t)=\frac{dy}{dt}$$

$$\frac{dx_1(t)}{dt}=x_2(t)$$

$$\frac{dx_2(t)}{dt}=\frac{-b}{M}x_2(t)-\frac{-k}{M}x_1(t)+\frac{1}{M}r(t)$$

$$y(t)=x_1(t)$$

EX2: R-L-C circuit system
---

<center>

![EX1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FboZ04f%2FbtsRadG6kul%2FAAAAAAAAAAAAAAAAAAAAADq_uqk5OFDYd_F34v6kLqHgRFluo-Nd04chWrjfbJah%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DEfFeG1jbZRvHkzjtCBX6pO7DBlM%253D)

</center>

x1과 x2는 항등식만 아니면 됩니다. 예를 들면 i_L과 v_o는 옴의 법칙에 의해 단순히 R이라는 상수가 곱해진 관계로 state로 사용할수 없습니다.

$$x_1(t)=v_c(t)$$ , $$x_2(t)=i_L(t)$$

$$u(t)=C\frac{dx_1(t)}{dt}+x_2(t)$$ $\rightarrow$ $$\frac{dx_1(t)}{dt}=\frac{1}{C}[-x_2(t)+u(t)]$$

$$L\frac{x_2(t)}{dt}+Rx_2(t)-x_1(t)=0$$ $\rightarrow$ $$\frac{dx_2(t)}{dt}=\frac{1}{L}[x_1(t)-Rx_2(t)]$$

$$\frac{dx_1(t)}{dt}=\frac{1}{C}[-x_2(t)+u(t)]$$

$$\frac{dx_2(t)}{dt}=\frac{1}{L}[x_1(t)-Rx_2(t)]$$

$$y(t)=Rx_2(t)$$

왜 State Space Equation을 사용하는가?
---

1차 미분방정식은 다음과 같이 time division에서 transition from initial state 부분과 Effect of input 부분으로 나눌 수 있습니다. 이는 system 분석에 큰 도움이 됩니다.

$$\dot{x}=ax(t)+bu(t)$$

$$sX(s)-x(0)=aX(s)+bU(s)$$

$$X(s)=\frac{1}{s-a}x(0)+\frac{1}{s-a}bU(s)$$

$$x(t)=e^{at}x(0)+\int{e^{a(t-\tau)}bu(\tau)d\tau}$$

$$x(t)=\Phi(t) \cdot x(0)+\int{\Phi(t-\tau)bu(\tau)d\tau}$$

$$\Phi(t) \cdot x(0)$$는 transition from initial state , $$\int{\Phi(t-\tau)bu(\tau)d\tau}$$는 Effect of input

State vector and state space equation
---
고차 미분방정식은 기본적으로 state가 하나가 아니라 여러개이므로 state는 보통 vector form 으로 정의하게 됩니다.

$$\mathbf{x}(t)=\begin{pmatrix}x_1(t) \\\\ x_2(t) \\\\ \vdots \\\\ x_n(t)\end{pmatrix}$$

이러면 Ex2 에서처럼 여러개의 1차 미분방정식을 행렬로 나타낼 수 있게 됩니다.

$$\dot{\mathbf{x}}(t)=\begin{bmatrix}0&-\frac{1}{C}\\\\\frac{1}{L}&-\frac{R}{L}\end{bmatrix}\mathbf{x}(t)+\begin{bmatrix}\frac{1}{C}\\\\0\end{bmatrix}\mathbf{u}(t)$$

$$\mathbf{y}(t)=\begin{bmatrix}0&R\end{bmatrix}\mathbf{x}(t)+0\&middot\mathbf{u}(t)$$

이는 마치

$$\dot{\mathbf{x}}(t)=\mathbf{Ax}(t)+\mathbf{Bu}(t)$$ 과 같은 State differential equation

$$\mathbf{y}(t)=\mathbf{Cx}(t)+\mathbf{Du}(t)$$ 과 같은 Output equation 이라고 하고

이를 통합해 State space equation 까지 만들어내면 State에 의미가 부여되어있기 때문에 체계적이므로 의미관계를 파악할 수 있고, 행렬로 이루어진 1차 미분방정식이기 때문에 컴퓨터의 계산속도도 매우 빨라집니다.

마찬가지로 time division 으로 끌고 올때도 다음과 같이 가능합니다.

$$\mathbf{x}(s)=[s\mathbf{I-A}]^{-1}\mathbf{x}(0)+[s\mathbf{I-A}]^{-1}\mathbf{BU}(s)$$

$$\mathbf{\Phi}(s)=[s\mathbf{I-A}]^{-1}$$

$$\mathbf{\Phi}(t)=\mathcal{L}^{-1}[\mathbf{\Phi}(s)]$$

$$\mathbf{x}(t)=\mathbf{\Phi}(t)\mathbf{x}(0)+\int{\mathbf{\Phi}(t-\tau)\mathbf{bu}(\tau)d\tau}$$

이때 $$\mathbf{\Phi}(t)$$ 는 State transition matrix 라고 합니다.

.
.
.
.
.
.
.
.

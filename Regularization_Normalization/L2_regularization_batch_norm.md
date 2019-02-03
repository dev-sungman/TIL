# L2 Regularization & Batch-Normalization

우연히 좋은 블로그를 발견하여, 공부하며 기록에 남기려 한다. 

항상 Regularization, Normalization 은 헷갈리는 개념이였지만, 쏟아져 나오는 논문들을 읽어보고 구현해보느라 깊숙히 들어가 보지는 못했다. 이번을 기회삼아 한번 공부를 해보려 한다. 



## Intro

BatchNormalization 이 포함된 최근의 CNN의 아키텍쳐는 L2 penalty 의 의미가 많이 줄었다. 

그렇지만 상당히 많이 사용되는 개념임은 확실하다. 

열심히 공부를 하며 정리하려고 했는데, 어찌저찌 하다보니 축약 번역본 정도가 된 것 같아 아쉽다.



## L2 Regularization / Weight Decay

우선 L2 regularization의 수식을 살펴보면, 



**L2 regularization form :**

 					$Loss(w, x) = DataLoss(w, x) + \frac 1 2c||w||^2​$



**During gradient descent,**

​					$w    := w - \alpha\frac d{dw}Loss(w, x)$

​						$= w-\alpha\frac d{dw}(DataLoss(w,x) + c \frac 12||w||^2)$				

​						$= w - \alpha \frac {dDataLoss(w,x)}{dw} - \alpha cw$

​						$	= w(1-\alpha c) - \alpha \frac {dDataLoss(w,x)}{dw}$



위의 수식을 보면, L2 Regularization은 매 optimization step 마다 gradient를 따라 weights를 0으로 줄여나가는 역할을 하는 것을 알 수 있다. ($1-\alpha c​$ 만큼)

L2 Regularization 의 가장 큰 목적은 overfitting을 피하는 것에 있다. 

**weight decay는 모델의 weights를 작게 만들어서, training data 에서의 에러를 증가시킨다. 그러나, 데이터에서의 정규성을 잡아낸 중요한 weights 들은 다음 스탭에서 다시 복구 하며, 에러를 다시 감소시킨다.  반대로 데이터의 수가 적고 오류에 영향을 미치지 않거나 특정 배치의 노이즈인 weights 값은 쉽게 복구되지 않는다. 따라서 최종 모델은 노이즈가 제거된 regular한 형태의 모델이 된다. 이 것이 L2 regularization 이 동작하는 과정이다.** 



## Batch Normalization

Batch Normalization은 CNN의 구조에 사용되는 기법으로, 이전 레이어 에서의 채널별 평균과 크기를 normalization 해준다. 주로 Convolution layer와 activation function 사이에서 사용된다. 



아래 수식에서 $BN$은 batch normalization layer의 출력, $L$은 batch normalization layer의 입력, $w$는 이전 layer의 weight 이다. 



​				$BN(w) = \frac {L(w) - \mu_L(w)} {\sqrt {\sigma_L(w)^2} + \epsilon }$



where:

​				$\mu_L(w)[c] = \frac 1 {NXY} \sum L(w)[n, x, y, c]$

​				$\sigma_L(w)[c]^2 = \frac 1 {NXY} \sum (L(w)[n, x, y, c] - \mu_L(w)[c]^2)$



$\epsilon$ 은 거의 영향을 끼치지 않는 상수를 나타낸다. 위의 수식을 보면, $BN$ 은 각 채널마다 $L$ 을 shift 하고 rescale 하여 평균이 0이고 표준 편차가 1인 것으로 만드는 것과 같다. 



BN은 2015년 loffe and Szegedy의 논문에서 소개가 되었는데, training 과정 중의 layer의 activation들을 안정화 시키는데 목적 이 있었다. 이 뿐만 아니라 깊은 신경망이 saturate 되거나, diverge 되는 불안정성을 줄여주었다. 그러나 이것이 왜 도움을 주는지는 연구의 한 주제이다. 



## TODO

* What Happens When Both Are Used Together
* No More L2 Regularization Mechanism
* New Effect on Gradient Scale and Learning Rate





## References

https://blog.janestreet.com/l2-regularization-and-batch-norm/





​			



​			






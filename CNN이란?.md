# About-CNN-Convolution-Neural-Network
#### 2019.10.25 양지현 작성

![cnn](https://t1.daumcdn.net/cfile/tistory/213C6141583ED6AB0A)

## 1. Convolutional Layer
컨볼루셔널 레이어는 input data 로부터 특징을 추출하는 역할을 한다.
컨볼루셔널 레이어는 특징을 추출하는 기능을 하는 필터(Filter)와, 이 필터의 값을 비선형 값으로 바꾸어 주는 액티베이션 함수(Activiation 함수)로 이루어진다.

### 1.1 Filter  
필터는 그 특징이 데이터에 있는지 없는지를 검출해주는 함수이다.
쥐 그림에서 좌측 상단의 이미지 부분을 잘라내서 필터를 적용해보자.
![ㅇ](https://t1.daumcdn.net/cfile/tistory/224E1641583ED6AC34)

잘라낸 이미지와 필터를 곱하면
![ㅇ](https://t1.daumcdn.net/cfile/tistory/237FE245583ED70A27)  
이처럼 곡선에 해당하는 부분은 큰 수가 나온다.

하지만 아래 그림처럼 쥐 그림에서 곡선이 없는 부분에 같은 필터를 적용해보면
![ㅇ](https://t1.daumcdn.net/cfile/tistory/2723C841583ED6AC23)  
결과값이 0에 수렴하게 된다.

*즉 필터는 입력받은 데이타에서 그 특성을 가지고 있으면 결과 값이 큰값이 나오고, 특성을 가지고 있지 않으면 결과 값이 0에 가까운 값이 나오게 되서 데이타가 그 특성을 가지고 있는지 없는지 여부를 알 수 있게 해준다.*

### 1.2 다중 필터의 적용  
입력값에는 여러가지 특징이 있기 때문에 하나의 필터가 아닌 여러개의 다중 필터를 같이 적용하게 된다. 
![ㅇ](https://t1.daumcdn.net/cfile/tistory/221BE141583ED6AD1A)  

이렇게 각기 다른 특징을 추출하는 필터를 조합하여 네트워크에 적용하면, 원본 데이터가 어떤 형태의 특징을 가지고 있는지 없는지를 판단해 낼 수 있다.  

다음은 하나의 입력 데이터에 앞서 적용한 세로와 가로선에 대한 필터를 동시에 적용한 네트워크의 모양이다.  
![ㅇ](https://t1.daumcdn.net/cfile/tistory/2739D541583ED6AD2B)

### 1.3 Stride  
그러면 이 필터를 어떻게 원본 이미지에 적용할까? 

아래 그림을 보면, 5x5 원본 이미지가 있을때, 3x3인 필터를 좌측 상단에서 부터 오른쪽으로 한칸씩 그 다음 한줄을 내려서 또 왼쪽으로 한칸씩 적용해서 특징을 추출해낸다.  

오른쪽 Convolved Feature 행렬이 바로 원본 이미지에 3x3 필터를 적용하여 얻어낸 결과 이다.
![ㅇ](https://t1.daumcdn.net/cfile/tistory/210B0A39583EDBBB05)

*이렇게 필터를 적용 하는 간격 (여기서는 우측으로 한칸씩 그리고 아래로 한칸씩 적용하였다.) 값을 Stride라고 하고, 필터를 적용해서 얻어낸 결과를 Feature map 또는 activation map 이라고 한다.*


### 1.4 Padding  
앞에서 원본 데이터에 필터를 적용한 내용을 보면 필터를 적용한 후의 결과값은 필터 적용전 보다 작아졌다.  
즉, 5x5 원본 이미지가 3x3의 1 stride 값을 가지고 적용하니 결과 값은 3x3으로 크기가 작아졌다.  

그런데, CNN 네트워크는 하나의 필터 레이어가 아니라 여러 단계에 걸쳐서 계속 필터를 연속적으로 적용하여 특징을 추출해 나가는데, 필터 적용 후 결과 값이 작아지게 되면 처음에 비해서 특징이 많이 유실 될 수 있다.   
이를 방지 하기 위한 방법으로 padding 을 사용한다.  
padding은 결과 값이 작아지는 것을 방지하기 위해서 입력값 주위로 0 값을 넣어서 입력 값의 크기를 인위적으로 키워서, 결과값이 작아지는 것을 방지 하는 기법이다.  


다음 그림을 보자, 32x32x3 입력값이 있을때, 5x5x3 필터를 적용 시키면 결과값 (feature map)의 크기는 28x28x3 이 된다.  
여기에 input 계층 주위로 0을 둘러 싸서, 결과 값이 작아지는 것을 막는다.  

![ㅇ](https://t1.daumcdn.net/cfile/tistory/23083C43583ED7621D)


* Q. 필터는 어떻게 만드는 것일까?  
그렇다면 CNN에서 사용되는 이런 필터는 어떻게 만드는 것일까? CNN의 신박한 기능이 바로 여기에 있는데, 데이터를 넣고 학습을 시키면, 자동으로 학습 데이타에서 학습을 통해서 특징을 인식하고 필터를 만들어 낸다.


### 1.5 Pooling(or Sub sampling)  
이렇게 convolutional layer를 거쳐서 추출된 특징들은 필요에 따라서 서브 샘플링 sub sampling이라는 과정을 거친다.  

convolutional layer를 통해서 어느정도 특징이 추출 되었으면, 이 모든 특징을 가지고 판단을 할 필요가 없다.  
쉽게 예를 들면, 우리가 고해상도 사진을 보고 물체를 판별할 수 있지만, 작은 사진을 가지고도 그 사진의 내용이 어떤 사진인지 판단할 수 있는 원리이다.  

따라서 추출된 Activation map을 인위로 줄이는 작업을 하는데, 이 작업을 sub sampling 또는 pooling 이라고 한다.  
Sub sampling 방법에는 max pooling, average pooling, L2-norm pooling 등이 있고 그중에서 max pooling 이라는 기법이 많이 사용된다. 

* Max Pooling

아래 그림을 보면 4x4 Activation map에서 2x2 맥스 풀링 필터를 stride를 2로 하여 2칸씩 이동하면서 맥스 풀링을 한 예인데, 좌측 상단에서는 6이 가장 큰 값이기 때문에 6을 뽑아내고, 우측 상단에는 2,4,7,8 중 8 이 가장 크기 때문에 8을 뽑아 내었다.

![ㅇ](https://t1.daumcdn.net/cfile/tistory/2121E641583ED6AF23)

이런 sampling을 통해서 얻을 수 있는 장점은
1. 전체 데이타의 사이즈가 줄어들기 때문에 연산에 들어가는 컴퓨팅 리소스가 적어지고
2. 데이터의 크기를 줄이면서 소실이 발생하기 때문에, 오버피팅을 방지할 수 있다.


## 2. Fully connected Layer
convolutional layer에서 특징이 추출이 되었으면 이 추출된 특징 값을 기존의 neural network에 넣어서 분류를 한다.  
따라서 CNN의 최종 네트워크 모양은 다음과 같이 된다.

![ㅇ](https://t1.daumcdn.net/cfile/tistory/23630641583ED6B01E)

* Softmax 함수  
Softmax도 앞에서 언급한 sigmoid나 ReLu와 같은 액티베이션 함수의 일종이다.  
Sigmoid 함수가 이산 분류 (결과값에 따라 참 또는 거짓을 나타내는) 함수라면, Softmax 는 여러개의 분류를 가질 수 있는 함수이다.
예를 들어서 사람을 넣었을때, 설현일 확률 0.9, 지현인 확율 0.1 식으로 0~1 사이의 확률로 표현해주는게 softmax 함수이다. 


[출처](https://bcho.tistory.com/1149)

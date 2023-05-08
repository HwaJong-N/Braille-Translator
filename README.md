# Braille-Translator

## 2023년 1학기 졸업설계

## 작품 설명

시각장애인 분들은 글씨를 읽을 수 없어 점자에 의존해야한다. 하지만 실제로 점자를 읽으실 수 있는 분들은 10 %도 되지 않는다는 기사를 본 적이 있다. 졸업작품으로 조금 의미있는 것을 하고 싶어서 점자를 대신 읽어주는 단말기를 주제로 선정하였다

<br>
<br>

## 맡은 역할

초기에는 openCV 전체를 담당했다. 그러다가 역할 분담을 정하면서 다른 팀원과 함께 openCV 를 맡게 되었다. 그 후에 openCV 를 이미지 처리 부분과 점자 판단 부분으로 나누어 진행하기로 하였으며, 이 때까지 모든 코드는 내가 작성을 했었기 때문에 내가 조금 더 잘 할 수 있는 점자 판단 부분 로직을 맡게 되었다


<br>
<br>


## 간단 소개

### 이미지 스티칭

#### 1. 특징점 추출


![](https://velog.velcdn.com/images/hj_/post/80d90778-15f7-499d-82f4-8289d2082371/image.JPG)


* 단말기를 통해 촬영한 이미지이다

* 빛을 이용해야하기 때문에 카메라 단말기를 만들고, LED 를 부착하였다

* 반짝거리는 것으로 표시된 것이 추출된 특징점이다


<br>


#### 2. 특징점 매칭

![](https://velog.velcdn.com/images/hj_/post/6110b8c8-33e6-4b5f-989f-c27c00bed612/image.JPG)


* SIFT 알고리즘을 사용하여 두 개의 이미지에 대해 특징점을 추출하고 매칭을 진행하였다


<br>


#### 3. 특징점 필터링

![](https://velog.velcdn.com/images/hj_/post/d8035667-f640-481b-b768-6376cb6c3bb6/image.JPG)


* RANSAC 과 Homography 를 이용해 매칭된 특징점들을 필터링하였다


<br>


#### 4. 이미지 스티칭 결과

![](https://velog.velcdn.com/images/hj_/post/0c2a9423-d1b0-4c3a-ab1e-cab9a6a2b9a0/image.jpg)


* 직접 구현한 스티칭 알고리즘을 통해 두 개의 이미지를 합쳤다



<br>
<br>
<br>



### 이미지 처리

#### 1. 이미지 전처리

![](https://velog.velcdn.com/images/hj_/post/7f1ebd22-8dca-4946-8f95-5db854e23bc2/image.JPG)


* 이미지 스티칭이 완료된 이미지를 가져와 원본 이미지로 사용한다

* 디노이징과 정규화를 거친 원본 이미지이다

* 디노이징은 사진 상의 먼지 같이 작은 이물질을 제거하기 위해 사용하였다

* 정규화는 빛을 이용한 이진화를 사용하기 때문에 사진에서 차이를 극명하게 하기 위해 사용하였다


<br>

#### 2. 이미지 이진화

![](https://velog.velcdn.com/images/hj_/post/706a3d47-fbef-4ef9-b438-515c06902a0f/image.JPG)



* 원본 이미지를 이진화 처리하였다


<br>


#### 3. Contour 로 좌표 추출

![](https://velog.velcdn.com/images/hj_/post/0558178f-98b6-480f-a6bd-e0aff2c244fc/image.JPG)


* contour 를 통해 추출한 좌표들이다

* 이 때의 좌표들은 아직까지 보정을 거치지 않았기 때문에 같은 수평선 혹은 수직선의 좌표여도 조금씩 어긋나있다


<br>

#### 4. 분류 알고리즘을 적용해 좌표 보정

![](https://velog.velcdn.com/images/hj_/post/108dfa6f-b641-434b-8688-7776cff007bf/image.JPG)


* 분류 알고리즘을 통해 비슷한 좌표들을 하나로 합친 결과이다

* 같은 수직선 혹은 수평선에 있는 비슷한 좌표들을 하나로 합쳐서 하나의 점자로 사용하였다

* 비슷한 좌표들의 y 좌표를 통일시키고, x 좌표를 통일시키는 과정을 거치기 때문에 결론적으로 비슷한 좌표 자체를 통일시키게 된다


<br>

#### 5. 점자 판단

![](https://velog.velcdn.com/images/hj_/post/8e50f1dc-123b-4d85-a298-7ad075d98125/image.JPG)



* 내가 담당한 부분의 마지막 최종 결과이다

* 이렇게 찾아진 점자가 어떤 점자인지 가능한 모든 좌표와 비교하여 어떤 점자인지 나타내고 이를 튜플로 전달한다


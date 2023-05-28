# Braille-Translator

## 2023년 1학기 졸업설계

## 작품 설명

시각장애인 분들은 글씨를 읽을 수 없어 점자에 의존해야한다. 하지만 실제로 점자를 읽으실 수 있는 분들은 10 %도 되지 않는다는 기사를 본 적이 있다. 졸업작품으로 조금 의미있는 것을 하고 싶어서 점자를 대신 읽어주는 단말기를 주제로 선정하였다

<br>
<br>

## 맡은 역할

초기에는 openCV 전체를 담당했다. 그러다가 역할 분담을 정하면서 다른 팀원과 함께 openCV 를 맡게 되었다. 그 후에 openCV 를 이미지 처리 부분과 점자 판단 부분으로 나누어 진행하기로 하였으며, 이 때까지 모든 코드는 내가 작성을 했었기 때문에 내가 조금 더 잘 할 수 있는 점자 판단 부분 로직을 맡게 되었다

추가적으로 이미지 스티칭 부분을 맡아 추출기를 이용한 특징점 추출, 매칭점 추출 및 필터링을 진행하였다 그리고 최종 결합 부분에서 매칭점괴 기울기를 이용해 이미지 결합하는 알고리즘을 직접 만들었다


<br>
<br>


## 과정 설명

### 1. 특징점 추출


![](https://velog.velcdn.com/images/hj_/post/d6a9c38c-c2d3-4852-bd75-78b2d0f59fb6/image.JPG)



* 촬영된 이미지에서 특징점을 추출한다

* 결합된 이미지라면 바로 이전에 결합된 부분을 제외하고 특징점 추출을 진행한다

* 즉, **1 과 2 가 결합된 이미지와 3 을 결합한다고 했을 때, 결합된 이미지에서 1 에 해당하는 부분에서는 특징점을 추출하지 않는다**



<br>



### 2. 특징점 매칭


![](https://velog.velcdn.com/images/hj_/post/0bd6ee74-ca06-4683-871c-8d6b48a96304/image.JPG)


* SIFT 알고리즘을 사용하여 두 개의 이미지에 대해 특징점을 추출하고 매칭을 진행하였다

* **SIFT 추출기를 생성할 때, 이미지 피라미트의 계층을 5로 하였고, sigma 값을 3.2로 하였다**


<br>


### 3. 특징점 필터링

![](https://velog.velcdn.com/images/hj_/post/41de2cc9-d4b2-41df-8fae-e7377b498692/image.JPG)



* 올바르지 않은 매칭점이 있을 수도 있기 때문에 특징점들을 필터링하였다

* RANSAC 과 Homography 를 이용해 매칭된 특징점들을 필터링하였다



<br>



### 4. 이미지 스티칭 결과



![](https://velog.velcdn.com/images/hj_/post/9a1a86af-e92e-44f7-9722-f96d632dc097/image.JPG)



* 촬영된 모든 이미지를 결합한다

* 3번 과정에서 매칭점을 필터링해도 올바르지 않은 매칭점이 존재한다

* **올바르지 않은 매칭점이 존재해도 정확하게 결합될 확률을 높이기 위해 기울기를 계산한 후, 최빈값 혹은 가장 작은 기울기 값을 이용하였다**




<br>


### 5. 이미지 전처리

![](https://velog.velcdn.com/images/hj_/post/0d9800c4-4dfb-4e2b-b4a3-dad1c9834d07/image.JPG)



* 이미지 스티칭이 완료된 이미지를 가져와 원본 이미지로 사용한다

* 디노이징과 정규화를 거친 원본 이미지이다

* 디노이징은 사진 상의 먼지 같이 작은 이물질을 제거하기 위해 사용하였다

* 정규화는 빛을 이용한 이진화를 사용하기 때문에 사진에서 차이를 극명하게 하기 위해 사용하였다


<br>


### 6. 이미지 이진화

![](https://velog.velcdn.com/images/hj_/post/34b30c22-f912-4eba-8b90-aecc132e9483/image.JPG)


* 원본 이미지를 이진화 처리하였다


<br>


### 7. Contour 로 좌표 추출

![](https://velog.velcdn.com/images/hj_/post/f4a86dd0-b00f-439f-a5ec-29754db7194f/image.JPG)



* contour 를 통해 추출한 좌표들이다

* 좌표를 추출할 때, 해당 contour 의 면적에 조건을 두어 너무 크거나 너무 작은 면적을 가진 좌표들은 추출하지 않았다

* **이 때의 좌표들은 아직까지 보정을 거치지 않았기 때문에 같은 수평선 혹은 수직선의 좌표여도 조금씩 어긋나있다**

* **또한, 빛의 영향으로 하나의 점자에 여러 개의 좌표들이 잡힌 경우도 존재한다**


<br>



### 8. 분류 알고리즘을 적용해 좌표 보정

![](https://velog.velcdn.com/images/hj_/post/3f69bd63-3f12-4357-b7c9-0ea7caacf4bc/image.JPG)


* 분류 알고리즘을 통해 비슷한 좌표들을 하나로 합친 결과이다

* **같은 수직선 혹은 수평선에 있는 비슷한 좌표들을 하나로 합쳐서 하나의 점자로 사용하였다**

* **비슷한 좌표들의 y 좌표를 통일시키고, x 좌표를 통일시키는 과정을 거치기 때문에 결론적으로 하나의 점자에 여러 개의 좌표가 있는 문제 역시 자동적으로 해결된다**



<br>



### 9. 점자 판단

![](https://velog.velcdn.com/images/hj_/post/ce678569-0a1a-4a8c-9e9e-19518b0cc181/image.JPG)



* 구해진 점자들로 가능한 모든 점자들을 생성 및 사진 상의 점자들과 매칭을 진행하여 점자의 모양을 판단한다



<br>



### 10. 점자 해석

![](https://velog.velcdn.com/images/hj_/post/aa51d02b-48c0-4139-93eb-a4c4a089f771/image.JPG)



* 5번에서 찾아진 점자가 어떤 점자인지 가능한 **모든 좌표와 비교하여 어떤 점자인지 튜플로 나타내고 이를 리스트에 담아 번역부에 전달한다**

* 번역부가 추출된 점자의 리스트를 받아 번역한 결과이다

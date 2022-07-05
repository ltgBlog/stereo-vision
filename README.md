# stereo-vision
종합설계 프로젝트1 스테레오 비전

sv1-캘리브레이션   
sv2-특징점/디스크립터 추출, 매칭   
sv3-렉티피케이션   
# SW Tool To Use
* 개발환경 : 우분투(Ubuntu), Virtual Box
* 언어 : C / C++
* 


# 추진배경
* AR, MR(혼합현실)을 이용한 HoloLens와 같은 장비가 출시되고 있으며, 산업현장에서도 
이를 활용하는 Bin Picking 등의 수요가 증가하고 있음.
* MR의 원천 기술이 되는 스테레오 비전, 특히 단일 카메라 기반의 스테레오 비전 기술
에 대해 이론을 분석하고 구현하고자 함. 

# 1. 프로젝트 수행 배경
## 1.1 스테레오 비전 기술 개발의 필요성

![그림 1](image/그림(1).png)    
[그림 1] 사람의 3차원 지각

![그림 2](image/그림(2).png)   
[그림 2] 카메라 3차원 인식
* 스테레오 비전이란 사람의 시각 시스템을 모방한 기술이다. 사람은 두 눈으로부터 좌/우 차이가 존
재하는 2차원 영상을 각막에 입력받고, 그 영상들을 뇌로 융합되는 과정을 통해 3차원을 인지한다.
* 이와 동일한 메커니즘으로, 스테레오 비전 기술은 카메라를 통해 입력되는 다른 2차원의 이미지 2개
를 3차원으로 인지할 수 있도록 한다.
* 단순히 이미지만으로는 크고 먼 물체와 작고 가까운 물체를 비슷한 화상으로 인식하는데, 이 기술을 
통해 컴퓨터에서 이미지 속 물체의 3차원 좌표를 얻게 되는 것이다.
* [그림 5]는 왼쪽에서 얻은 이미지와 오른쪽에서 얻은 이미지를 스테레오 비전 기술을 통해 물체의 깊
이를 측정하고 그 값을 색으로 표현한 모습이다.

## 1.2 스테레오 비전 기술 개발의 필요성
![그림 3](image/그림(3).png)   
[그림 3] 머신 비전 시장의 성장 
* 최근 들어, 스테레오 비전 기술이 사용되는 사례들을 많이 볼 수 있다. 포켓몬 고, 자율주행 자동차, Bin Picking 외에도 HoloLens나 로봇의 시각 등에도 사용되고 있다.
* [그림 3]의 그래프를 보면 스테레오 비전 기술이 기반이 되는 머신 비전 시장의 규모는 계속해서 커
지는 추세임을 확인할 수 있다.
* 하지만 관련 기술에는 미흡한 점이 많아 상용화가 되지 않고 있다. 특히 사람의 생명과 직결되는 상
품일 경우는 더욱 조심스럽다. 아래는 자율주행 자동차와 관련된 사고 일부이다. 
-2016년 테슬라 자율주행 자동차가 앞차와 충돌하는 사고 
-2018년 자율주행 자동차가 옆에서 나오는 사람을 치는 사고 
더 안전하고 정확한 서비스를 제공하기 위해서는 기술을 발전시킬 필요가 있다. 
* 우리는 위의 제품들의 원천 기술인 스테레오 비전 기술을 분석하고 구현함으로써 더 나은 기술을 위
한 발판이 되고자 한다. 단일 카메라로부터 얻은 2D 이미지들로부터 물체의 위치를 계산하여 3D 좌
표를 생성하는 기능을 구현하는 것이 최종 목표이다.

# 2. 프로젝트 목표 : 단일 카메라와 OpenCV를 이용한 스테레오 비전 기술 개발
## 2.1 Calibration
![그림 4](image/그림(4).png)  
[그림 4] 왜곡된 인지

![그림 5](image/그림(5).png)  
[그림 5] Calibration 
* 카메라는 [그림 4]처럼 왜곡된 장면을 보이는 그대로 받아들인다.
* 카메라의 내부 파라미터를 구하여 왜곡된 이미지를 [그림 5]의 오른쪽 그림과 같이 보정한다.
* 내부 파라미터에는 렌즈의 굴절률, 렌즈와 이미지 센서와의 거리 등이 있다.
## 2.2 Feature Detection & Matching
![그림 6](image/그림(6).png)    
[그림 6] 특징점 추출

![그림 7](image/그림(7).png)  
[그림 7] 이미지의 특징점 매칭
* 다른 위치에서 촬영한 두 정지된 영상에서 특징점을 비교하여 유사한 특징을 가진 점들을 매칭한다.
## 2.3 Fundamental Matrix & Rectification
![그림 8](image/그림(8).png)  
[그림 8] Rectification

* 앞 단계에서 구한 매칭점들로 카메라의 외부파라미터를 구한다.
* 외부파라미터에는 카메라가 이동한 거리, 회전한 각도 등이 있다.
* 외부파라미터를 통해 두 이미지 속 물체의 크기와 높이를 동일하게 맞춘다.
## 2.4 Disparity Map
![그림 9](image/그림(9).png)
[그림 9] Disparity Map
* Rectification한 두 이미지에서 매칭점들의 좌우 좌표 차이를 통해 물체의 깊이를 구한다.
* 깊이 값을 Disparity Map으로 표현한다.

# 3. 프로젝트 수행 결과 
* 기술 발전을 목적으로, 조원 모두 같은 를 맡아 단계마다 더 좋은 코드를 채택하여 결과물을 도출하였다. 
## 3.1 Calibration
![그림 10](image/그림(10).png)  
[그림 10] 이미지 좌표와 world 좌표의 관계 
* [x,y]는 2D 점의 좌표, [X, Y, Z]는 3D 점의 좌표, [R | t]는 회전/이동변환 행렬, A는 카메라의 내부파
라미터 행렬이다. 
* 이때, 회전/이동변환 행렬은 카메라의 외부파라미터를 의미한다.
* 내부파라미터는 렌즈 종류, 이미지 센서와의 거리인 초점거리, 광학축이 이미지 센서와 만나는 점인 주점이다.
* 외부파라미터는 사진 찍었을 때, 카메라의 이동과 회전율을 의미하므로 후에 다른 방법으로 측정한 값을 사용한다.  
## [방법1]  
![그림 11](image/그림(11).png)  
[그림 11] Calibration Code 1-1
* chessboard의 실제 좌표를 생성하여 World 좌표를 만든다.
* World 좌표는 사람이나 카메라가 인식한 장면이 아닌 왜곡이 없는 실제 3차원 좌표를 뜻한다.
![그림 12](image/그림(12).png)  
[그림 12] Calibration Code 1-2 
* 개인 카메라로 찍은 chessboard를 인식하여 사진 위에 표시한다.
![그림 13](image/그림(13).png)  
[그림 13] Calibration Code 1-3 
* calibrateCamera()함수로 World 좌표와 카메라에 의해 왜곡된 chessboard를 비교하여 내부파라미터
와 외부파라미터를 구한다.
![그림 14](image/그림(14).png)  
[그림 14] Calibration Code 1-4 
* undistort()함수로 내부파라미터를 이용하여 이미지의 왜곡을 없앤다.  

## [결과1]  
![그림 15](image/그림(15).png)    
[그림 15] calibration 결과(1)  
![그림 16](image/그림(16).png)    
[그림 16] calibration 결과(2)  
* [그림18]는 인터넷에 있는 자료로 테스트한 결과이다.
* [그림18]의 아래 그림이 calibration한 결과로 위의 그림과 비교했을 때 휘어진 chessboard가 펴진 것을 확인할 수 있다.
* [그림19]는 직접 촬영한 사진이다.
* 왜곡된 chessboard를 복원하면서 사진 모서리가 휜 것을 확인할 수 있다.
![그림 17](image/그림(17).png)  
[그림 17] 카메라의 내부 파라미터와 왜곡계수 
* 3X3 행렬은 카메라의 행렬이다. l 1X5 행렬은 카메라의 왜곡계수이다.    
[방법2]  
![그림 18](image/그림(18).png)  
[그림 18] Calibration Code 2-1   

* 이미지들을 읽고, 체크보드 모서리를 찾는다. 모두 읽었다면, 이미지마다 CalibrateCamera 함수로 내
부 파라미터와 외부 파라미터, 왜곡 계수를 얻어낸다.
* 읽은 이미지들에서 찾은 모서리에 표시를 해두는 drawChessboardConers를 실행한다.
![그림 19](image/그림(19).png)  
[그림 19] Calibration Code 2-2 
* 구했던 내부 파라미터와 왜곡계수를 이용하여 initUndistortRectifyMap 함수를 이용하여 교정행렬 
map1, map2를 구한다.
* map1, map2를 remap 함수에 넣어서 왜곡이 제거된 이미지를 얻어낸다.     
## [결과2]  
![그림 20](image/그림(20).png)        
[그림 20] 왜곡된 이미지  
![그림 21](image/그림(21).png)        
[그림 21] 왜곡 제거된 이미지  
* [그림 23]은 찍은 사진에서 체스 보드 모서리를 표시한 왜곡되어있는 이미지이고, [그림 24]은 
Calibration을 수행하고 왜곡이 제거된 이미지이다.  
![그림 22](image/그림(22).png)     
[그림 22] Calibration 결과   
* .yml 파일에 Calibration의 결과인 내부/외부 파라미터, 왜곡계수가 저장되었다.
# 3.2 Feature Detection
[방법1]  
![그림 23](image/그림(23).png)    
[그림 23] Feature Detection Code 1-1
KeyPoints와 Descriptors를 추출해주는 SURF Detector를 생성한다.
* SURF : 속도를 높이기 위해 적분 영상을 사용하여 특징점을 추출하는 알고리즘이다.
![그림 24](image/그림(24).png)  
[그림 24] Feature Detection Code 1-2 
* KeyPoints와 Descriptors를 추출하고, drawKeypoints함수로 KeyPoints를 이미지에 그린다.  
[결과1]  
![그림 25](image/그림(25).png)    
[그림 25] Feature Detection 결과1    
![그림 26](image/그림(26).png)    
[그림 26] Feature Detection 결과2  

* [그림 28]과 [그림 29]는 각각의 이미지에서 특징점을 추출하고 이미지에 그린 결과이다.    
[방법2]  
![그림 27](image/그림(27).png)    
[그림 27] Feature Detection Code 2-1
* ORB, KAZE, MSER, SURF 등 다양한 Detector 비교하여 출력한다.
* ORB (Oriented Fast and Rotated BRIEF) : 벡터를 특징점의 픽셀값을 기준으로 0, 1 이진값으로 나타
내어 리소스 낭비를 방지하고 빠르고 정확하게 표현하는 알고리즘이다.
* KAZE : 비선형 스케일 공간에서 비선형확신 필터링으로 특징점을 검출한다.
* MSER (Maximally Stable External Regions) : 그레이 스케일 영상 또는 컬러영상에서 주변보다 더 밝
거나 더 어두운 영역으로 임계값의 범위에서 안정적인 영역을 특징으로 검출한다.  
[결과2]  
![그림 28](image/그림(28).png)    
[그림 28] Feature Detection 결과 2
* 같은 이미지라도 Detector의 종류에 따라 추출되는 특징점이 다르다는 것을 확인할 수 있다.
# 3.2 Feature Matching
[방법1]  
![그림 29](image/그림(29).png)    
[그림 29] Feature Matching Code 1-1 
* Descriptor : 검출된 특징점 주위의 밝기, 색상, 방향 등의 정보를 계산한다.
* DescriptorMatcher : Descriptor와 매칭을 한 번에 수행한다.
* FLANN DescriptorMatcher를 통해 매칭한다.
* FLANN은 Fast Library for Approximate Nearest Neighbors의 약자로 큰 이미지에서 특징점을 매칭
하는 것에 최적화된 알고리즘이다. 큰 이미지에 대해서는 BF매칭보다 빠르게 동작한다.
* descriptor1, descriptor2 : Feature Extraction에서 구한 두 이미지의 특징점을 담은 행렬이다.
![그림 30](image/그림(30).png)    
[그림 30] Feature Matching Code 1-2  
* 다른 매칭 쌍들과 비교하여 잘못된 값은 제외시키고 잘 매칭된 것만 선택한다.  
[결과1]  
![그림 31](image/그림(31).png)    
[그림 31] 특징점 매칭 결과
* 물체는 고정시키고 카메라만 이동시켜 촬영한 사진을 테스트 이미지로 사용하였다.
* 3~5개 정도의 점을 제외한 모든 점이 잘 매칭된 것을 확인할 수 있다.
# 3.3 Fundamental Matrix
[방법1]  
![그림 32](image/그림(32).png)    
[그림 32] Fundamental Matrix Code 
* points1과 points2에 두 이미지에 대해 매칭된 descriptors에 대한 인덱스를 각각 저장한다.
* 2차원 좌표를 표현할 수 있는 Point2f 타입으로 변환한다.
* 타입을 변환한 매칭 쌍들로 Fundamental Matrix를 구한다.    
[결과1]  
![그림 33](image/그림(33).png)  
[그림 33] Fundamental Matrix 결과
* Fundamental Matrix : 카메라 파라미터를 포함한 두 이미지의 좌표 사이의 기하학적 관계를 표현하는 행렬
* 기하학적 관계는 내부파라미터와 외부파라미터를 의미한다.
# 3.4 Rectification
[방법1]  
![수식 1](image/수식(1).png)  
[수식 1] Fundamental Matrix와 Essential Matrix의 관계
* E : Essential Matrix
* F : Fundamental Matrix- 10 -
* K : 카메라의 내부 파라미터 행렬
* Essential Matrix : Fundamental Matrix에서 카메라의외부파라미터만 표현한 행렬이다. 두 이미지 사
이의 이동, 회전 변환 행렬을 의미한다.  
![그림 34](image/그림(34).png)      
[그림 34] Rectification Code 
* [수식1]을 이용하여 카메라의 이동, 회전 변환 행렬을 구한다. 
* 첫 번째 단계인 calibration에서 구한 카메라 내부 파라미터를 이용하여 왜곡을 제거한다.
* 이동, 회전 변환 값으로 Rectification을 한다.
* Rectification한 결과를 원본 이미지에 mapping하여 나타낸다.
* 두 이미지를 가로로 붙여 Rectification의 결과를 확인한다.    
[결과1]  
![그림 35](image/그림(35).png)      
[그림 35] Rectification 전
![그림 36](image/그림(36).png)      
[그림 36] Rectifiaction 후
* [그림 36]에서는 두 사진에서의 물체 크기와 상하위치가 다르다.
[그림 36]은 Rectification후의 이미지로, 두 사진에서의 물체 크기와 상하위치가 같다.
# 3.5 Disparity Map
[방법1]  
![그림 37](image/그림(37).png)      
[그림 37] Disparity Map Code
Rectification된 이미지의 Depth map을 생성해주는 StereoBM과 StereoSGBM 객체를 생성한다.
* StereoBM은 속도가 빠르지만 불분명하고 StereoSGBM은 느리지만 정확한 결과를 얻을 수 있다.
* compute함수를 이용하여 두 이미지의 시차(disparity)를 계산한다.
* 시차의 최소값과 최대값을 출력하고 Depth map 이미지를 출력한다.    
[결과1]  
![그림 38](image/그림(38).png)    
[그림 38] Disparity Map 결과
* [그림 38]의 왼쪽이 StereoBM으로 얻은 결과이고, 오른쪽이 StereoSGBM으로 얻은 결과이다.
* 거리가 가까울수록 흰색에 가깝다.
* 거리가 a~b 범위라고 하면, 이를 밝기 0~100에 scaling하여 나타낸 것이다. 
# 4. 기대 효과 및 활용 방안
## 4.1 드론
![그림 39](image/그림(39).png)    
[그림 39] Feature Map  
![그림 40](image/그림(40).png)    
[그림 40] 3D Map  

* 단일 카메라 기반 스테레오 비전 기술을 활용하여, 카메라를 무인비행장치, 또는 드론에 장착하여 지
형/건물/시설 등을 촬영하여 [그림 39]과 같이 특징점들을 추출 해낼 수 있다.
* 2차원 데이터를 바탕으로 카메라와 객체 간의 상대적인 위치 상관관계를 파악하여 3차원 정보를 생
성하고, 이를 활용하여 [그림 40]과 같이 3차원 지도를 구현할 수 있다.
* 3D map을 이용하여 위치를 파악하고 실제 지형/건물의 색을 입혀서 현실의 모습과 비슷하게 만들어
서 구축하면, 파괴되거나 붕괴된 재난 현장에서 사람이 직접 파악하기 어려운 부분들을 안전한 원거
리 범위에서 파악하여 효율적인 재난 상황 파악 및 지형/건물/시설들의 복구 기술로 활용될 수 있고, 그 밖에도 주요 시설물 점검 및 관리, 3차원 공간 정보 구축 등 지형기반 분야에 효율적인 기여를 
할 수 있을 것이다.
## 4.2 Bin Picking
![그림 41](image/그림(41).png)  
[그림 41] Bin Picking 
* 다른 물건이 섞여 있어도 원하는 물체를 집어내는 [그림 44]의 빈 피킹 기술에도 효율적으로 사용될 
수 있다.
* 카메라로 로봇이 집어내는 구역을 촬영하여 3차원 정보를 획득하여 얼마나 움직여야 하는지를 결정
하고 특징점과 디스크립터를 추출하여 원하는 물체를 집을 수 있게 된다.
* 단일 카메라의 경제적인 이점과 딥러닝 기술을 함께 사용하면 훨씬 효율적인 기술로 발전해나갈 수 
있을 것이다.
* 이 밖에도 스테레오 카메라를 활용하여 지능형 자동차 자율주행, 로봇 네비게이션, 사물 크기 측정, 부품 자동 조립, 제스쳐 인식 기반의 interactive game, 의료 분야에서의 사진 분석, 문자 인식 분야 
등 많은 분야에 사용될 것으로 예상된다. 

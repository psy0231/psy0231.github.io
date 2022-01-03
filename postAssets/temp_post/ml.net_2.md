---
title: ML.NET 
date: 2021-04-06 12:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---

## ML.NET의 기계 학습 작업
- 기계 학습 작업은 요청되는 문제나  
질문 및 사용 가능한 데이터에 따라 수행되는  
예측 또는 추론의 종류입니다.  
- 예를 들어 
    - 분류 : 데이터를 범주에 할당 
    - 클러스터링 : 유사성에 따라 데이터 그룹화.
- 기계 학습은  
명시적으로 프로그래밍되는 것이 아니라  
데이터 패턴을 사용합니다.
- 이 문서에서는 사용자가 ML.NET에서 선택할 수 있는  
다양한 기계 학습 작업과 몇 가지 일반적인 사용 사례에 대해 설명.
- 시나리오에 적합한 작업을 선택했으면  
모델을 학습할 최상의 알고리즘을 선택해야 한다.  
사용 가능한 알고리즘은 각 작업의 섹션에 나열됩니다.

## Binary classification / 이진분류
- 두 클래스 중에 데이터의 인스턴스가 속한 클래스를 예측하는 데 사용  
- 분류 알고리즘의 입력은 레이블이 지정된 예제 집합.  
여기서 각 레이블은 0 또는 1의 정수.  
- 이진 분류 알고리즘의 출력은 분류자로,  
레이블이 없는 새 인스턴스의 클래스를 예측하는 데 사용.  
- 이진 분류 시나리오의 예.
    - “긍정적” 또는 “부정적”으로 Twitter 댓글의 감정 이해.
    - 환자에게 특정 질병이 있는지 여부 진단.
    - 전자 메일을 “스팸”으로 표시할지 여부 결정.
    - 사진이 특정 항목을 포함하는지 여부 확인(예: 개 또는 과일).
- 참고 :  Wikipedia - Binary classification

### Binary classification trainers
- AveragedPerceptronTrainer
    - 평균 퍼셉트론으로 훈련 된 선형 이진 분류 모델을 사용하여  
    대상을 예측하는 IEstimator \<TTransformer>.
- SdcaLogisticRegressionBinaryTrainer
    - 이진 로지스틱 회귀 분류 모델을 훈련하기위해  
    확률적 이중 좌표 상승 방법을 사용한 IEstimator \<TTransformer>.  
    - 훈련 된 모델은 보정되고 선형 함수의 출력 값을  
    PlattCalibrator에 공급하여 확률을 생성 할 수 있다. 
- SdcaNonCalibratedBinaryTrainer
    - 이진 로지스틱 회귀 분류 모델을 훈련하기위한   
    확률적 이중 좌표 상승 방법을 사용한 IEstimator \<TTransformer>.
- SymbolicSgdLogisticRegressionBinaryTrainer
    - IEstimator \<TTransformer>는 기호확률적 경사하강 법으로 훈련된  
    선형 이진 분류 모델을 사용하여 대상 예측.
- LbfgsLogisticRegressionBinaryTrainer
    - IEstimator \<TTransformer>는 L-BFGS 방법으로 훈련된  
    선형 로지스틱 회귀 모델을 사용하여 대상을 예측합니다.
- LightGbmBinaryTrainer
    - LightGBM을 사용하여 강화된 의사 결정 트리 이진 분류 모델을  
    학습시키기위한 IEstimator \<TTransformer>.
- FastTreeBinaryTrainer
    - FastTree를 사용하여 의사 결정 트리 이진 분류 모델을  
    교육하기위한 IEstimator \<TTransformer>입니다.
- FastForestBinaryTrainer
    - Fast Forest를 사용하여 의사 결정 트리 이진 분류 모델을  
    교육하기위한 IEstimator \<TTransformer>입니다.
- GamBinaryTrainer
    - 일반화 된 가법 모델 (GAM)을 사용하여  
    이진 분류 모델을 훈련하기위한 IEstimator \<TTransformer>.
- FieldAwareFactorizationMachineTrainer
    - IEstimator \<TTransformer>는 확률적 그래디언트 방법을 사용하여   
    훈련된 필드 인식 분해 기계 모델을 사용하여 대상을 예측.
- PriorTrainer
    - 이진 분류 모델을 사용하여  
    대상을 예측하기위한 IEstimator \<TTransformer>입니다.
- LinearSvmTrainer
    - IEstimator \<TTransformer>는 Linear SVM으로 학습 된  
    선형 이진 분류 모델을 사용하여 대상을 예측.

### Binary classification inputs and outputs
- 이진 분류에서 최상의 결과를 내려면  
학습 데이터의 균형이 이루어져야 함
    - 긍정 및 부정 학습 데이터 수 일치.  
    - 누락 값은 학습 전에 처리되어야 한다.
- 입력 레이블 열 데이터는 Boolean이다.  
입력 기능 열 데이터는 고정 크기의 Single 벡터.
- 트레이너의 출력.

    |출력 열 이름|열|유형 설명|
    |---|---|---|
    |Score|Single|모델에서 계산된 원시 점수|
    |PredictedLabel|Boolean|점수 부호에 따라 예측된 레이블<br> 음수 점수는 false에<br> 양수 점수는 true에 매핑.

## Multiclass classification
- 데이터 인스턴스의 클래스를 예측하는데 사용되는  
감독된 기계 학습 작업입니다.  
- 분류 알고리즘의 입력은 레이블이 지정된 예제 집합입니다.  
- 각 레이블은 일반적으로 텍스트로 시작.  
그런 다음, TermTransform을 통해 실행되어 키(숫자) 유형으로 변환.  
- 분류 알고리즘의 출력은 분류자로,  
레이블이 없는 새 인스턴스의 클래스를 예측하는 데 사용할 수 있습니다.  
- 다중 클래스 분류 시나리오의 예는 다음과 같습니다.
    - 개의 품종을 “시베리안 허스키”, “골든 리트리버”, “푸들” 등으로 결정.
    - 동영상 리뷰를 “긍정적”, “중립” 또는 “부정적”으로 이해.
    - 호텔 리뷰를 “위치”, “가격”, “청결도” 등으로 범주화.
- 참고 : Wikipedia - Multiclass classification

### Multiclass classification trainers
- LightGbmMulticlassTrainer
    - LightGBM을 사용하여 강화 된 의사 결정 트리 다중 클래스 분류 모델을 훈련하기위한 IEstimator \<TTransformer>.
- SdcaMaximumEntropyMulticlassTrainer
    - 최대 엔트로피 다중 클래스 분류기를 사용하여  
    대상을 예측하는 IEstimator \<TTransformer>입니다.  
    - 훈련 된 모델 MaximumEntropyModelParameters는  
    클래스의 확률을 생성합니다.
- SdcaNonCalibratedMulticlassTrainer
    - TheIEstimator \<TTransformer>는 선형 다중 클래스 분류기를 사용하여 대상을 예측합니다.  
    - 훈련 된 모델 LinearMulticlassModelParameters는  
    클래스의 확률을 생성합니다.
- LbfgsMaximumEntropyMulticlassTrainer
    - IEstimator \<TTransformer>는 L-BFGS 방법으로 훈련된  
    최대 엔트로피 다중 클래스 분류기를 사용하여 대상을 예측합니다.
- NaiveBayesMulticlassTrainer
    - 이진 특성 값을 지원하는 다중 클래스 Naive Bayes 모델을 학습하기위한 IEstimator \<TTransformer>입니다.
- OneVersusAllTrainer
    - 지정된 이진 분류기를 사용하는 일대 다 다중 클래스 분류기를 훈련하기위한 IEstimator \<TTransformer>입니다.
- PairwiseCouplingTrainer
    - 지정된 이진 분류기를 사용하는 쌍 결합 다중 클래스 분류기를 훈련하기위한 IEstimator <TTransformer>입니다.
- ImageClassificationTrainer
    - 이미지를 분류하기 위해 심층 신경망 (DNN)을 훈련하기위한 IEstimator <TTransformer>입니다.

### Multiclass classification inputs and outputs
- 입력 레이블 열 데이터는 키 형식.  
기능 열은 고정된 크기의 Single 벡터.
- 트레이너 출력.

    |출력 이름|형식|설명|
    |---|---|---|
    |Score|Single 벡터|모든 클래스의 점수.<br>값이 높을수록 연결된 클래스에 해당할 가능성이 높음.<br>i번째 요소의 값이 가장 크다면 예측된 레이블 인덱스는 i가 된다.<br>i는 0부터 시작하는 인덱스.|
    |PredictedLabel|키 형식|예측된 레이블의 인덱스.<br>값이 i라면 실제 레이블은 키 값 입력 레이블 형식에서 i번째 범주.|

## Regression - 회귀
- 관련 기능 집합에서 레이블 값을 예측하는 데 사용되는 감독된 기계 학습.  
- 레이블은 실제 값일 수 있고,  
분류 작업과 같이 한정된 값 집합에 속하지 않음.  
- 회귀 알고리즘 모델은 기능 값이 달라질 때  
레이블이 변경되는 방식을 결정하기 위한  
관련 기능에 대한 레이블의 종속성.  
- 회귀 알고리즘의 입력은 알려진 값의 레이블이 포함된 예제 집합. 
- 회귀 알고리즘의 출력은  
새 입력 기능 집합의 레이블 값을 예측하는 데 사용할 수 있는 함수.  
- 회귀 시나리오의 예는 다음과 같습니다.
    - 침실 수, 위치 또는 규모와 같은 주택 특성을 기반으로 주택 가격 예측.
    - 과거 데이터 및 현재 시장 추세를 기반으로 미래 재고 가격 예측.
    - 광고 예산을 기반으로 제품 판매 예측.

### Regression trainers
- LbfgsPoissonRegressionTrainer
    - 포아송 회귀 모델을 학습하기위한 IEstimator \<ITransformer>.
- LightGbmRegressionTrainer
    - LightGBM을 사용하여 강화 된 의사 결정 트리 회귀 모델을 훈련하기위한 IEstimator \<TTransformer>.
- SdcaRegressionTrainer
    - 확률적 이중 좌표 상승 방법을 사용하여  
    회귀 모델을 훈련하기위한 IEstimator \<TTransformer>.
- OlsTrainer
    - 선형 회귀 모델의 매개 변수를 추정하기 위해  
    일반 최소 제곱 (OLS)을 사용하여  
    선형 회귀 모델을 훈련하기위한 IEstimator \<TTransformer>.
- OnlineGradientDescentTrainer
    - 선형 회귀 모델의 매개 변수를 추정하기 위해  
    OGD (Online Gradient Descent)를 사용하여  
    선형 회귀 모델을 훈련하기위한 IEstimator \<TTransformer>.
- FastTreeRegressionTrainer
    - FastTree를 사용하여 의사 결정 트리 회귀 모델을  
    교육하기위한 IEstimator \<TTransformer>.
- FastTreeTweedieTrainer
    - Tweedie 손실 함수를 사용하여  
    의사 결정 트리 회귀 모델을 훈련하기위한 IEstimator\<TTransformer>.  
    - 이 트레이너는 푸 아송, 복합 푸 아송 및 감마 회귀의 일반화입니다.
- FastForestRegressionTrainer
    - Fast Forest를 사용하여 의사 결정 트리 회귀 모델을 교육하기위한 IEstimator \<ITransformer>입니다.
- GamRegressionTrainer
    - 일반화 된 가법 모델 (GAM)을 사용하여 회귀 모델을 훈련하기위한 IEstimator \<TTransformer>.

### Regression inputs and outputs
- 입력 레이블 열 데이터는 Single.
- 트레이너는 출력.

    |출력 이름|	형식|	설명
    |---|---|---|
    |Score|	Single|	모델에서 예측된 원시 점수

## Clustering
- 데이터 인스턴스를 비슷한 특성을 포함하는 클러스터로 그룹화하는데 사용되는 감독되지 않는 기계 학습 작업입니다.  
- 클러스터링을 사용하여 검색 또는 단순 관찰을 통해  
논리적으로 파생될 수 없는 데이터 세트의 관계를 식별할 수도 있습니다. 
- 클러스터링 알고리즘의 입력 및 출력은 선택한 방법에 따라 다릅니다.  
분산, 중심, 연결 또는 밀도 기반 방법을 사용할 수 있습니다.  
- 현재 ML.NET은 K-평균 클러스터링을 사용하는 중심 기반 방법을 지원합니다.  
- 클러스터링 시나리오의 예는 다음과 같습니다.
    - 호텔 선택의 습관과 특성을 기반으로 호텔 투숙객 세그먼트 이해.
    - 대상 광고 캠페인을 구축하는 데 도움이 되는 고객 세그먼트 및 인구 통계 식별.
    - 제조 메트릭을 기반으로 인벤토리 범주화.

### Clustering trainer
- KMeansTrainer
    - KMeans 클러스터 러 훈련을위한 IEstimator \<TTransformer>

### Clustering inputs and outputs
- 입력 기능 데이터는 Single이어야 합니다.  
레이블이 필요하지 않습니다.
- 트레이너 출력.

    |출력 이름|	형식|설명
    |---|---|---|
    |Score|	Single 벡터|모든 클러스터의 중심에서 특정 데이터 지점까지의 거리
    |PredictedLabel	|키 형식|모델에서 예측한 가장 가까운 클러스터의 인덱스

## Anomaly detection
- 이 작업은 PCA(보안 주체 구성 요소 분석)를 사용하여  
변칙 검색 모델을 만듭니다.  
- PCA 기반 변칙 검색은 유효한 트랜잭션과 같은,  
한 클래스의 학습 데이터를 얻기는 쉽지만  
대상 변칙의 충분한 샘플을 얻기는 어려운 시나리오에서  
모델을 빌드하는 데 도움이 됩니다.
- 기계 학습의 검증된 기술인 PCA는  
데이터의 내부 구조를 나타내고 데이터 차이를 설명하기 때문에  
탐색적 데이터 분석에서 자주 사용됩니다.  
- PCA는 여러 변수를 포함하는 데이터를 분석하여 작동합니다.  
변수 간의 상관 관계를 찾고  
결과에서 차이점을 가장 잘 캡처하는 값의 조합을 확인합니다.  
- 이러한 조합된 기능 값이 보안 주체 구성 요소라는  
보다 간결한 기능 공간을 만드는 데 사용됩니다.
- 변칙 검색은 다음과 같은 기계 학습의 여러 중요한 작업을 포함합니다.
    - 잠재적으로 사기성이 있는 트랜잭션 식별.
    - 네트워크 침입이 발생했음을 나타내는 학습 패턴.
    - 비정상적인 환자 클러스터 찾기.
    - 시스템에 입력된 값 확인.
- 변칙은 정의상 거의 발생하지 않으므로  
모델링에 사용할 데이터의 대표 샘플을 수집하기 어려울 수 있다.  
이 범주에 포함된 알고리즘은 특히  
불균형 데이터 세트를 사용하여 모델을 빌드하고  
학습시키는 핵심 과제를 해결하도록 설계되었습니다.

### Anomaly detection trainer
- RandomizedPcaTrainer
    - Randomized SVD 알고리즘을 사용하여  
    대략적인 PCA를 훈련하기위한 IEstimator \<TTransformer>.

### Anomaly detection inputs and outputs
- 입력 기능은 고정 크기의 Single 벡터.
- 트레이너 출력.

    |출력 이름|	형식|	설명
    |---|---|---|
    |Score	|Single	|변칙 검색 모델에서 계산된 음수가 아니며 바인딩되지 않은 점수
    |PredictedLabel	|Boolean|	입력이 변칙(PredictedLabel=true)인지 아닌지(PredictedLabel=false) 여부를 나타내는 true/false 값입니다.

## Ranking
- 순위 지정 작업은 레이블이 지정된 예제 세트에서 순위매기기를 구성합니다. - 이 예제 세트는 지정된 기준에 따라  
점수가 매겨질 수 있는 인스턴스 그룹으로 이루어져 있습니다.  
- 순위 레이블은 각 인스턴스마다 {0, 1, 2, 3, 4}입니다.  
순위매기기는 각 인스턴스마다  
점수를 알 수 없는 새 인스턴스 그룹의 순위를 지정하도록 학습됩니다.  
- ML.NET 순위 지정 학습자는 기계 학습 순위 지정을 기반으로 합니다.

### Ranking training algorithms
- LightGbmRankingTrainer
    - LightGBM을 사용하여 강화 된 의사 결정 트리 순위 모델을 교육하기위한 IEstimator \<TTransformer>.
- FastTreeRankingTrainer
    - FastTree를 사용하여 의사 결정 트리 순위 모델을 교육하기위한 IEstimator \<TTransformer>입니다.

### Ranking input and outputs
- 입력 레이블 데이터 형식은 키 형식 또는 Single.  
레이블의 값은 관련성을 결정하며 값이 클수록 관련성이 높습니다.  
레이블이 키 형식이면 키 인덱스가 관련성 값이며  
가장 작은 인덱스가 가장 관련도가 적습니다.  
레이블이 Single이면 값이 클수록 관련도가 높습니다.
- 기능 데이터는 고정 크기의 Single 벡터여야 하며  
입력 행 그룹 열은 키 형식이어야 합니다.
- 이 트레이너는 다음을 출력합니다.

|출력 이름|	형식|	설명|
|---|---|---|
|Score|	Single|	모델이 예측을 판단하기 위해 계산한 바인딩되지 않은 점수

## Recommendation
- 권장 사항 작업을 통해 권장 제품 또는 서비스 목록을 생성할 수 있습니다. - ML.NET은 카탈로그에 기록 제품 등급 데이터가 있는 경우  
공동 작업 필터링 알고리즘인 MF(행렬 인수)를 권장 사항에 사용합니다.  
- 예를 들어 사용자에 대한 기록 영화 등급 데이터가 있고,  
사용자가 다음에 시청할 가능성이 큰 다른 영화를 추천하려고 합니다.

### Recommendation training algorithms
- MatrixFactorizationTrainer
    - IEstimator <TTransformer>는 행렬 분해 (협업 필터링 유형이라고도 함)를 사용하여 행렬의 요소를 예측합니다.

## Forecasting
- 예측 작업은 과거 시계열 데이터를 사용하여  
향후 동작에 대한 예측을 수행합니다.  
- 예측에 적용되는 시나리오에는  
날씨 예측, 계절별 매출 예측, 예측 유지 관리 등이 있습니다.

### Forecasting trainers
- ForecastBySsa
    - 일변량 시계열 예측을위한 SSA (Singular Spectrum Analysis) 모델.
    - 모델에 대한 자세한 내용은 http://arxiv.org/pdf/1206.6910.pdf를 참조하십시오.

## Image Classification
- 이미지의 클래스(범주)를 예측하는 데 사용되는 감독된 기계 학습 작업입니다.  
입력은 레이블이 지정된 예제 세트입니다.  
각 레이블은 일반적으로 텍스트로 시작됩니다.  
그런 다음, TermTransform을 통해 실행되어 키(숫자) 유형으로 변환됩니다.  
이미지 분류 알고리즘의 출력은 새 이미지의 클래스를 예측하는 데 사용할 수 있는 분류자입니다.  
이미지 분류 작업은 다중 클래스 분류의 한 유형입니다.  
이미지 분류 시나리오의 예는 다음과 같습니다.
  - 개의 품종을 “시베리안 허스키”, “골든 리트리버”, “푸들” 등으로 결정./
  - 제조 제품에 결함이 있는지 여부를 확인합니다.
  - "장미", "해바라기" 등으로 꽃 종류를 결정합니다

### Image classification trainers
- ImageClassificationTrainer
  - IEstimator \<TTransformer> 분류 이미지에 깊은 신경망 (DNN)을 훈련합니다.

### Image classification inputs and outputs
-입력 레이블 열 데이터는 키 형식이어야 합니다.  
기능 열은 Byte의 가변 크기 벡터여야 합니다.

- 이 트레이너는 다음 열을 출력합니다.

  |출력 이름|형식|설명|
  |---|---|---|
  |Score|Single	|모든 클래스의 점수입니다. 값이 높을수록 연결된 클래스에 해당할 가능성이 높습니다. i번째 요소의 값이 가장 큰 경우 예측된 레이블 인덱스는 i가 됩니다. i는 0부터 시작하는 인덱스입니다.|
  |PredictedLabel|키 유형	|예측된 레이블의 인덱스입니다. 값이 i라면 실제 레이블은 키 값 입력 레이블 형식에서 i번째 범주입니다.|

## Object Detection
- 이미지의 클래스(범주)를 예측하는 데 사용되지만  
해당 범주가 이미지 내에 있는 경계 상자를 제공하는 감독된 기계 학습 작업입니다.  
이미지에서 단일 개체를 분류하는 대신,  
개체 감지는 이미지 내에서 여러 개체를 검색할 수 있습니다.  
개체 감지의 예는 다음과 같습니다.
  - 도로 이미지에서 자동차, 표지판 또는 사람을 감지합니다.
  - 제품 이미지에서 결함 검색.
  - X-Ray 이미지에서 관심 영역 검색.
- 개체 감지 모델 학습은 현재 Azure Machine Learning을 사용하는 Model Builder에서만 사용할 수 있습니다.


---
title: ML.NET 
date: 2021-04-06 12:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---

## ML.NET??
- ML.NET은 온라인 또는 오프라인 시나리오에서  
.NET 애플리케이션에 기계 학습을 추가할 수 있는 기능을 제공합니다.  
- 이 기능을 사용하여 애플리케이션에  
사용 가능한 데이터를 사용하여 자동 예측할 수 있습니다.  
- 기계 학습 애플리케이션은  
명시적으로 프로그래밍하지 않아도 데이터의 패턴을 사용하여 예측합니다.

- ML.NET의 핵심은 기계 학습 모델 입니다.  
- 모델은 입력 데이터를 예측으로 변환하는 데 필요한 단계를 지정합니다.  
- ML.NET를 사용하면 알고리즘을 지정하여 사용자 지정 모델을 학습하거나  
미리 학습된 TensorFlow 및 ONNX 모델을 가져올 수 있습니다.

- 모델이 있는 경우, 애플리케이션에 이를 추가하여 예측할 수 있습니다.

- ML.NET은 .NET Core를 사용하여  
Windows, Linux 및 macOS에서 실행되거나  
.NET Framework를 사용하여 Windows를 실행됩니다.  
- 64비트는 모든 플랫폼에서 지원됩니다.  
32비트는 TensorFlow, LightGBM 및 ONNX 관련 기능을 제외하고  
Windows에서 지원됩니다.

- ML.NET을 사용하여 수행할 수 있는 예측 유형은 다음과 같습니다.

    |||
    |---|---|
    |분류/범주화|고객 피드백을 긍정과 부정 범주로 자동 구분|
    |재발/연속 값 예측|크기 및 위치에 따라 주택 가격 예측|
    |변칙 검색|사기성 은행 거래 검색|
    |권장 사항|이전 구매 내역에 기반하여 온라인 구매자가 구매하려는 제품 제안|
    |시계열/순차 데이터	|날씨/제품 판매 예측|
    |이미지 분류|의료 이미지에서 병리학 분류|
    
## Hello ML.NET World
- 이 예제에서는 집 크기 및 가격 데이터를 사용하여 주택 가격을 예측하도록 선형 회귀 모델을 구성합니다.
    ```c#
    using System;
    using Microsoft.ML;
    using Microsoft.ML.Data;

    class Program
    {
        public class HouseData
        {
            public float Size { get; set; }
            public float Price { get; set; }
        }

        public class Prediction
        {
            [ColumnName("Score")]
            public float Price { get; set; }
        }

        static void Main(string[] args)
        {
            MLContext mlContext = new MLContext();

            // 1. Import or create training data
            HouseData[] houseData = {
                new HouseData() { Size = 1.1F, Price = 1.2F },
                new HouseData() { Size = 1.9F, Price = 2.3F },
                new HouseData() { Size = 2.8F, Price = 3.0F },
                new HouseData() { Size = 3.4F, Price = 3.7F } };
            IDataView trainingData = mlContext.Data.LoadFromEnumerable(houseData);

            // 2. Specify data preparation and model training pipeline
            var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
                .Append(mlContext.Regression.Trainers.Sdca(labelColumnName: "Price", maximumNumberOfIterations: 100));

            // 3. Train model
            var model = pipeline.Fit(trainingData);

            // 4. Make a prediction
            var size = new HouseData() { Size = 2.5F };
            var price = mlContext.Model.CreatePredictionEngine<HouseData, Prediction>(model).Predict(size);

            Console.WriteLine($"Predicted price for size: {size.Size*1000} sq ft= {price.Price*100:C}k");

            // Predicted price for size: 2500 sq ft= $261.98k
        }
    }
    ```

## Code workflow
- 다음 다이어그램은 반복적인 모델 개발 프로세스뿐만 아니라  
애플리케이션 코드 구조를 나타냅니다.
    - 학습 데이터를 수집하여 IDataView 개체로 로드
    - 기능을 추출하고 기계 학습 알고리즘을 적용할 작업 파이프라인 지정
    - 파이프라인에서 Fit() 를 호출하여 모델 학습
    - 모델을 평가하고 반복하여 개선
    - 애플리케이션에서 사용할 수 있도록 모델을 이진 형식으로 저장
    - 모델을 ITransformer 개체로 다시 로드
    - CreatePredictionEngine.Predict() 를 호출하여 예측

<!-- ![image]() -->

## Machine learning model
- 예측된 출력에 도달하기 위해 입력 데이터에서 수행할 변형이 포함된 개체.
### Basic
- 가장 기본적인 모델은 위의 주택 가격 예에서와 같이  
하나의 지속적인 수량이 다른 것과 비례하는 2차원 선형 회귀.

<!-- - ![image]() -->
    - 모델은 $Price = b+Size∗w$입니다.  
    매개 변수 b 및 w는 쌍 세트(크기, 가격)에 줄을 맞춰 추정됩니다.  
    모델의 매개 변수를 찾는 데 사용되는 데이터를 학습 데이터 라고 합니다.  
    이 기계 학습 모델의 입력을 특성(feature) 이라고 합니다.  
    - 이 예제에서는 Size가 유일한 특성입니다.  
    - 기계 학습 모델을 학습하는 데 사용하는 실측 자료(ground-truth) 값은 레이블 이라고 합니다.  
    여기에서는 학습 데이터 세트의 Price 값이 레이블입니다.

### More complex
- 더 복잡한 모델은 거래 텍스트 설명을 사용하여 금융 거래를 범주로 분류합니다.
- 각 거래 설명은 중복되는 단어와 문자를 제거하고  
단어와 문자 조합을 계산하여 일련의 기능으로 세분화됩니다.  
- 기능 집합은 학습 데이터에서 범주 집합에 기반하여  
선형 모델을 학습하는 데 사용됩니다.  
새로운 설명이 학습 집합의 설명과 유사할수록  
동일한 범주에 할당될 가능성이 커집니다.
- image 
<!-- ![image]() -->
    - 주택 가격 책정 모델 및 텍스트 분류 모델 둘 다 선형 모델입니다.  
    - 데이터의 성격과 해결하려는 문제에 따라  
    결정 트리, 일반화된 부가 모델 등을 사용할 수도 있습니다.  
    작업에서 모델에 대해 더 자세히 알아볼 수 있습니다.

## Data preparation
- 대부분의 경우에서 사용자가 가지고 있는 사용 가능한 데이터는  
기계 학습 모델을 학습하기 위해 직접 사용하기에는 적합하지 않습니다.  
- 원시 데이터는 준비하거나 사전 처리되어야  
모델의 매개 변수를 찾기 위해 사용할 수 있습니다.  
- 데이터를 문자열 값에서 숫자 표현으로 변환해야 합니다.  
사용자의 입력 데이터에서 중복되는 정보가 있을 수 있습니다.  
입력 데이터의 크기를 축소 또는 확장해야 할 수 있습니다.  
데이터를 정규화 또는 크기 조정해야 할 수 있습니다.
- ML.NET 자습서는 특정 기계 학습 작업에 사용되는  
텍스트, 이미지, 숫자 및 시계열 데이터에 대한  
다른 데이터 처리 파이프라인에 대해 알려줍니다.
- 데이터 준비 방법은 데이터 준비를 더 일반적으로 적용하는 방법을 보여줍니다.
- 리소스 섹션에서 모든 사용 가능한 변환의 부록을 확인할 수 있습니다.

## Model evaluation
- 모델을 학습한 후에는 미래의 예측을 얼마나 잘 할지 어떻게 알까요?  
- ML.NET를 사용하여 새로운 테스트 데이터를 기준으로 모델을 평가할 수 있습니다.
- 기계 학습 작업의 각 유형에는  
테스트 데이터 세트를 기준으로  
모델의 정확성 및 정밀도를 평가하는 데 사용하는 메트릭이 있습니다.
- 주택 가격 예의 경우 회귀 작업을 사용했습니다.  
- 모델을 평가하려면 원래 샘플에 다음 코드를 추가합니다.
    ```c#
    HouseData[] testHouseData =
    {
        new HouseData() { Size = 1.1F, Price = 0.98F },
        new HouseData() { Size = 1.9F, Price = 2.1F },
        new HouseData() { Size = 2.8F, Price = 2.9F },
        new HouseData() { Size = 3.4F, Price = 3.6F }
    };

    var testHouseDataView = mlContext.Data.LoadFromEnumerable(testHouseData);
    var testPriceDataView = model.Transform(testHouseDataView);

    var metrics = mlContext.Regression.Evaluate(testPriceDataView, labelColumnName: "Price");

    Console.WriteLine($"R^2: {metrics.RSquared:0.##}");
    Console.WriteLine($"RMS error: {metrics.RootMeanSquaredError:0.##}");

    // R^2: 0.96
    // RMS error: 0.19
    ```
    - 평가 메트릭은 오류가 낮은 수준이며  
    예측한 출력과 테스트 출력 간 상관 관계가 높다는 것을 알려줍니다.  
    이 과정은 아주 쉽습니다!  
    실제 사례에서는 훌륭한 모델 메트릭을 얻는 데 더 많은 튜닝이 필요합니다.

## ML.NET architecture
- 이 섹션에서는 ML.NET의 아키텍처 패턴을 살펴봅니다.  
숙련된 .NET 개발자라면 이러한 패턴의 일부는 익숙하고 일부는 덜 익숙해집니다.  
자세히 살펴보는 동안 긴장을 놓지 마세요!
- ML.NET 애플리케이션은 MLContext 개체로 시작합니다.  
이 싱글톤 개체는 카탈로그를 포함합니다.  
카탈로그는 데이터 로드 및 저장, 변환, 트레이너, 모델 작동 구성 요소를 위한 팩터리입니다.  
각 카탈로그 개체마다 다른 유형의 구성 요소를 만드는 메서드가 있습니다.

|||||
|---|---|---|---|
|데이터 로드 및 저장||DataOperationsCatalog||
|데이터 준비||TransformsCatalog||
|학습 알고리즘|이진 분류|BinaryClassificationCatalog||
||다중 클래스 분류|MulticlassClassificationCatalog||
||변칙 검색|AnomalyDetectionCatalog||
||클러스터링|ClusteringCatalog||
||예측|ForecastingCatalog||
||순위 지정|RankingCatalog||
||재발|RegressionCatalog||
||권장|RecommendationCatalog|Microsoft.ML.Recommender NuGet 패키지 추가|
||TimeSeries|TimeSeriesCatalog|Microsoft.ML.TimeSeries NuGet 패키지 추가|
|모델 사용||ModelOperationsCatalog||

- 위의 각 범주에서 만들기 메서드로 이동할 수 있습니다.  
카탈로그는 Visual Studio를 사용하여 IntelliSense를 통해 나타납니다.

<!-- ![image]()			 -->

### Build the pipeline
- 각 카탈로그 내에는 확장 메서드의 집합이 있습니다.  
확장 메서드를 사용하여 학습 파이프라인을 만드는 방법을 살펴 보겠습니다.
```c#
var pipeline = mlContext
                .Transforms.Concatenate("Features", new[] { "Size" })
                            .Append(mlContext.Regression.Trainers
                            .Sdca(labelColumnName: "Price", maximumNumberOfIterations: 100));
```
- 코드 조각에서 Concatenate 및 Sdca는 카탈로그에 있는 두 메서드입니다.  
이러한 메서드는 파이프라인에 연결된 IEstimator 개체를 만듭니다.

### Train the model
- 파이프라인에서 개체가 만들어지면 모델을 학습하는 데 데이터를 사용할 수 있습니다.
    ```c#
    var model = pipeline.Fit(trainingData);
    ```
    - Fit() 호출 시 입력 학습 데이터를 사용하여 모델의 매개 변수를 예상합니다.  
    이를 모델 학습이라고 합니다.  
    - 단, 위의 선형 회귀 모델에서는 두 가지 모델 매개 변수,  
    즉 편차 와 가중치 가 있었습니다.  
    - Fit() 호출 후 매개 변수의 값이 알려집니다.  
    대부분 모델에는 이것보다 더 많은 매개 변수가 있습니다.
- 모델 학습 방법에서 모델 학습에 대해 자세히 알아볼 수 있습니다.

- 결과 모델 개체는 ITransformer 인터페이스를 구현합니다.  
즉, 모델은 입력 데이터를 예측으로 변환합니다.
    ```c#
    IDataView predictions = model.Transform(inputData);
    ```

### Use the model
- 입력 데이터를 대량의 예측으로 변환하거나  
한 번에 하나의 입력을 변환할 수 있습니다.  
- 주택 가격 예에서는 두 가지를 수행했습니다.  
    - 모델 평가 목적으로는 대량, 
    - 새로운 예측을 하기 위해서는 한 번에 하나씩
- 단일 예측하기를 살펴 보겠습니다.
    ```c#
    var size = new HouseData() { Size = 2.5F };
    var predEngine = mlContext.CreatePredictionEngine<HouseData, Prediction>(model);
    var price = predEngine.Predict(size);
    ```
    - CreatePredictionEngine() 메서드는  
    입력 클래스 및 출력 클래스를 사용합니다.  
    필드 이름 및/또는 코드 특성은  
    모델 학습 및 예측 중 사용된 데이터 열의 이름을 결정합니다.  
    자세한 내용은 학습된 모델로 예측을 참조하세요.

### Data models and schema
- ML.NET 기계 학습 파이프라인의 핵심에는 DataView 개체가 있습니다.
- 파이프라인의 각 변환에는 입력 스키마 (변환 시 입력에서 보길 원하는 데이터 이름, 유형 및 크기)와  
출력 스키마(변환 후 해당 변환이 생성하는 데이터 이름, 유형 및 크기)가 있습니다.
-  파이프라인에서 하나의 변환으로 인한 출력 스키마가  
다음 변환의 입력 스키마와 일치하지 않는 경우 ML.NET은 예외를 throw합니다.
- 데이터 뷰 개체에는 열과 행이 있습니다.  
각 열에는 이름과 유형 및 길이가 있습니다.  
    - 예: 주택 가격 예의 입력 열에는 크기와 가격 이 있습니다.  
    두 가지 유형이며, 벡터 수량이라기 보다는 스칼라 수량입니다.

<!-- - ![image]() -->

- 모든 ML.NET 알고리즘은 벡터인 입력 열을 찾습니다.  
기본적으로 이 벡터 열은 특성(feature) 이라고 합니다.  
이것이 바로 주택 가격 예제에서  
Features 이라고 하는  새 열로 크기 열을 연관시킨 이유입니다.
    ```c#
    var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
    ```
- 또한 모든 알고리즘은 예측을 수행한 후 새 열을 만들기도 합니다.  
이러한 새 열의 고정된 이름은 기계 학습 알고리즘의 유형에 따라 결정됩니다.  
회귀 작업의 경우 새 열 중 하나가 점수 라고 합니다.  
이것이 바로 이 이름으로 가격 데이터의 이름을 지정한 이유입니다.
    ```c#
    public class Prediction
    {
        [ColumnName("Score")]
        public float Price { get; set; }
    }
    ```
    - Machine Learning 작업 가이드에서  
    다른 기계 학습 작업의 출력 열에 대해 더 자세히 알아볼 수 있습니다.

- DataView의 중요한 속성은 이들이 느리게 평가된다는 점입니다.  
데이터 뷰는 모델 학습 및 평가, 데이터 예측 중에만 로드되고 작동됩니다.  
ML.NET 애플리케이션을 쓰고 테스트하는 동안에는  
Visual Studio 디버거를 사용하여 미리 보기 메서드를 호출하여  
어떠한 데이터 뷰 개체도 살펴볼 수 있습니다.
    ```c#
    var debug = testPriceDataView.Preview();
    ```
    - 디버거에서 debug 변수를 보고 내용을 살펴볼 수 있습니다.  
    성능이 크게 저하되므로  
    프로덕션 코드에서는 미리 보기 메서드를 사용하지 마세요.

### Model Deployment
- 실제 애플리케이션에서 모델 학습과 평가 코드는 예측과 구분됩니다.  
- 사실,이 두 가지 활동은 별도의 팀에서 수행하는 경우가 많습니다.  
- 모델 개발 팀은 예측 애플리케이션에서 사용하기 위해 이 모델을 저장할 수 있습니다.

#  제품 리뷰 기반 평점 예측 모델 (Rating Prediction Project)

이 프로젝트는 Amazon 제품의 출시 초기 리뷰 텍스트 데이터를 분석하여, 해당 제품의 장기적인 **평균 평점** 을 예측하는 딥러닝 모델을 구축한 연구입니다. 사용자 정보나 메타데이터 없이 순수 자연어 처리 기법만으로 시장 반응을 예측하는 것을 목표로 합니다.

## 1. 프로젝트 개요 (Overview)
* **문제 정의**: 신상품 출시 초기, 부족한 리뷰 수로 인한 소비자의 신뢰도 저하 및 시장 반응 파악의 어려움.
* **핵심 가설**: 초기 5~10개의 리뷰에 담긴 감성적/언어적 특징이 제품의 최종 평점과 높은 상관관계를 가질 것이다.
* **기대 효과**: 이커머스 플랫폼에서 신상품 출시 전략 수립 및 초기 대응을 위한 실질적 데이터 제공.

## 2. 데이터 및 전처리 (Data & Preprocessing)
* **데이터셋**: 2023년 Amazon 제품 리뷰 데이터 (**asin** , **text** , **rating** , **time_stamp** 포함).
* **데이터 선별**: 
    * 최소 10건 이상의 리뷰를 보유한 상품 선별.
    * 제품 출시 후 6개월 이내의 리뷰만을 '초기 리뷰'로 간주하여 추출.
* **텍스트 전처리**:
    * 소문자 변환, URL, 특수문자, 불용어(**stopwords** ) 제거.
    * **Keras Tokenizer** 를 통해 상위 10,000개 단어 사용 및 길이 500으로 패딩 처리.
* **데이터 구성**: 개별 사용자의 리뷰를 하나로 결합(**concatenate** )하여 상품 전체에 대한 종합 평가 텍스트로 변환.

## 3. 모델 아키텍처 (Model Architecture)
모델은 **LSTM** 과 **Attention** 메커니즘을 결합한 구조로 설계되었습니다.

* **Embedding Layer**: 10,000개 단어를 64차원 벡터로 변환하며, 리뷰 데이터에 적합한 표현을 스스로 학습합니다.
* **LSTM Layers**: 
    * 첫 번째 층: 64 units, 전체 시퀀스 반환 (**return_sequences=True** ).
    * 두 번째 층: 32 units, 시퀀스 정보 압축.
    * 각 계층 사이 **Dropout** 을 적용하여 과적합(**Overfitting** ) 방지.
* **Attention Mechanism**: **Self-Attention** 구조를 통해 시퀀스 내 중요한 단어에 가중치를 부여하고 핵심 문맥 벡터(**Context Vector** )를 추출합니다.

## 4. 학습 결과 및 성능 (Results)
* **성능 지표**:
    * **MAE (Mean Absolute Error)** : **0.3893** (목표치 0.5 이내 달성).
    * **MAPE (Mean Absolute Percentage Error)** : **11.19%**.
* **결과 해석**:
    * 3.5점 이상의 높은 평점대에서 실제값과 예측값의 일치도가 매우 높음.
    * 잔차 분석 결과, 오차가 0을 중심으로 대칭적인 정규분포를 형성하여 모델의 안정성 확인.

## 5. 결론 및 제언 (Conclusion)
* **성능 검증**: 간단한 구조의 **LSTM** 및 **Attention** 결합 모델만으로도 유의미한 예측 정확도 확보가 가능함을 입증함.
* **향후 개선 방향**:
    * **Transformer** 기반 모델 도입을 통한 장기 시퀀스 처리 능력 고도화.
    * 평점 불균형 해소를 위한 샘플링 기법 및 손실 함수(**Loss Function** ) 조정.

## 6. 참조 (Reference)
* [Amazon Reviews Data 2023 (McAuley Lab)](https://www.kaggle.com/datasets/wajahat1064/amazon-reviews-data-2023)
* [Attention Mechanism (Wikidocs)](https://wikidocs.net/22893)
* [Keras Tokenizer (Wikidocs)](https://wikidocs.net/182469)

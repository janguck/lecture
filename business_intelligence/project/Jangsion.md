# Kaggle 후기
   - Two Sigma Connect: Rental Listing Inquiries
   - 목적 : 집의 특성을 가지고 입주자의 만족도 조사(Multiclassification problem ex) high ,medium ,low)
   - 성적 : 0.51392(266/2488)

 
## 데이터 종류

   - bathrooms
   - bedrooms
   - building_id
   - created
   - description
   - display_address
   - features
   - latitude
   - listing_id
   - longitude
   - manager_id
   - photos
   - price
   - street_address
   - interest_level

## Feature Engineering

1.base_feature(A)
 - 경도를 가지고 Feature를 생성함
 - 가격을 13000 기준으로 생성함
 - 가격을 log함수로 생성함
 - 침실과 화장실을 더해 총 방의 개수를 생성함
 - 평당 가격을 생성함
 - 집의 특징을 단어의 갯수로 생성함
 - 생성일시를 년도, 월, 날짜, 시간에 맞추어 생성함
 - manager_id를 통한 만족도를 예측함 (경험기반으로 한 방식)
 - Feature를 가지고 CountVectorizer를 생성하여 위의 Feature 병합

 2.base_feature(B)
 - base_feature(A) + 알파
 - 알파 : 사진부분의 생성날짜 (월, 주, 일, 시간, 등)

 3.base_feature(C)
 - base_feature(A) + 베타
 - 베타 : 구글의 s2sphere라는 프로그램

## Model

1. xgboost
2. ensemble
   - AdaBoostClassifier
   - KNeighborsClassifier
   - GradientBoostingClassifier
   - RandomForestClassifier
   - ExtraTreesClassifier

## Oversampling
 - 데이터의 비율을 똑같게 3063개로 1:1:1로 9189개 실험
 - 데이터의 비율 제시한 그대로 유지

## 실험결과

### Feature - A - XGboost_No_Oversampling
     - Accuracy = 0.76

|                     | <h2>       0        </h2>    | <h2>       1        </h2>     | <h2>      2   </h2>  | <h2> avg / total </h2>|
| :------------:      | :--------------------------: |  :--------------------------: |  :--------------------------: |  :-------------: |
|   <h2>precision</h2>| <h3>  0.60   </h3>           |  <h3>  0.52   </h3>           |  <h3>  0.83   </h3>           |  <h3>  0.74  </h3>|
|   <h2>recall   </h2>| <h3>  0.34   </h3>           |  <h3>  0.44   </h3>           | <h3>  0.92  </h3>           |  <h3>  0.76   </h3>|
|   <h2>f1-score </h2>| <h3>  0.43   </h3>           |  <h3>  0.47   </h3>           | <h3>  0.87   </h3>           | <h3>  0.75   </h3>|
|   <h2>support  </h2>| <h3>  776    </h3>           |  <h3>  2229   </h3>           | <h3>  6866    </h3>           | <h3>  9871   </h3>|

### Feature - B - Ensemble_No_Oversampling
     - Accuracy = 0.73

|                     | <h2>       0        </h2>    | <h2>       1        </h2>     | <h2>      2   </h2>  | <h2> avg / total </h2>|
| :------------:      | :--------------------------: |  :--------------------------: |  :--------------------------: |  :-------------: |
|   <h2>precision</h2>| <h3>  0.29   </h3>           |  <h3>  0.55   </h3>           |  <h3>  0.84   </h3>           |  <h3>  0.73  </h3>|
|   <h2>recall   </h2>| <h3>  0.54   </h3>           |  <h3>  0.26   </h3>           | <h3>  0.90  </h3>           |  <h3>  0.73   </h3>|
|   <h2>f1-score </h2>| <h3>  0.38   </h3>           |  <h3>  0.35   </h3>           | <h3>  0.87   </h3>           | <h3>  0.71   </h3>|
|   <h2>support  </h2>| <h3>  776    </h3>           |  <h3>  2229   </h3>           | <h3>  6866    </h3>           | <h3>  9871   </h3>|

### Feature - B - XGboost_No_Oversampling
     - Accuracy = 0.77

|                     | <h2>       0        </h2>    | <h2>       1        </h2>     | <h2>      2   </h2>  | <h2> avg / total </h2>|
| :------------:      | :--------------------------: |  :--------------------------: |  :--------------------------: |  :-------------: |
|   <h2>precision</h2>| <h3>  0.61   </h3>           |  <h3>  0.54   </h3>           |  <h3>  0.84   </h3>           |  <h3>  0.76  </h3>|
|   <h2>recall   </h2>| <h3>  0.33   </h3>           |  <h3>  0.47   </h3>           | <h3>  0.92  </h3>           |  <h3>  0.77   </h3>|
|   <h2>f1-score </h2>| <h3>  0.43   </h3>           |  <h3>  0.50   </h3>           | <h3>  0.88   </h3>           | <h3>  0.76   </h3>|
|   <h2>support  </h2>| <h3>  776    </h3>           |  <h3>  2229   </h3>           | <h3>  6866    </h3>           | <h3>  9871   </h3>|

### Feature - C - Ensemble_Yes_Oversampling
     - Accuracy = 0.56

|                     | <h2>       0        </h2>    | <h2>       1        </h2>     | <h2>      2   </h2>  | <h2> avg / total </h2>|
| :------------:      | :--------------------------: |  :--------------------------: |  :--------------------------: |  :-------------: |
|   <h2>precision</h2>| <h3>  0.16   </h3>           |  <h3>  0.45   </h3>           |  <h3>  0.95   </h3>           |  <h3>  0.77  </h3>|
|   <h2>recall   </h2>| <h3>  0.81   </h3>           |  <h3>  0.31   </h3>           | <h3>  0.61  </h3>           |  <h3>  0.56   </h3>|
|   <h2>f1-score </h2>| <h3>  0.27   </h3>           |  <h3>  0.61   </h3>           | <h3>  0.74   </h3>           | <h3>  0.62   </h3>|
|   <h2>support  </h2>| <h3>  776    </h3>           |  <h3>  2229   </h3>           | <h3>  6866    </h3>           | <h3>  9871   </h3>|

### Feature - C - XGboost_Yes_Oversampling
     - Accuracy = 0.68

|                     | <h2>       0        </h2>    | <h2>       1        </h2>     | <h2>      2   </h2>  | <h2> avg / total </h2>|
| :------------:      | :--------------------------: |  :--------------------------: |  :--------------------------: |  :-------------: |
|   <h2>precision</h2>| <h3>  0.36   </h3>           |  <h3>  0.41   </h3>           |  <h3>  0.92   </h3>           |  <h3>  0.76  </h3>|
|   <h2>recall   </h2>| <h3>  0.68   </h3>           |  <h3>  0.54   </h3>           | <h3>  0.73 </h3>           |  <h3>  0.69   </h3>|
|   <h2>f1-score </h2>| <h3>  0.47   </h3>           |  <h3>  0.47   </h3>           | <h3>  0.82   </h3>           | <h3>  0.71   </h3>|
|   <h2>support  </h2>| <h3>  776    </h3>           |  <h3>  2229   </h3>           | <h3>  6866    </h3>           | <h3>  9871   </h3>|

### Feature - C - Ensemble_No_Oversampling
     - Accuracy = 0.73

|                     | <h2>       0        </h2>    | <h2>       1        </h2>     | <h2>      2   </h2>  | <h2> avg / total </h2>|
| :------------:      | :--------------------------: |  :--------------------------: |  :--------------------------: |  :-------------: |
|   <h2>precision</h2>| <h3>  0.32   </h3>           |  <h3>  0.56   </h3>           |  <h3>  0.82   </h3>           |  <h3>  0.72  </h3>|
|   <h2>recall   </h2>| <h3>  0.45   </h3>           |  <h3>  0.26   </h3>           | <h3>  0.92 </h3>           |  <h3>  0.73   </h3>|
|   <h2>f1-score </h2>| <h3>  0.37   </h3>           |  <h3>  0.36   </h3>           | <h3>  0.87   </h3>           | <h3>  0.71   </h3>|
|   <h2>support  </h2>| <h3>  776    </h3>           |  <h3>  2229   </h3>           | <h3>  6866    </h3>           | <h3>  9871   </h3>|

### Feature - C - XGboost_No_Oversampling
     - Accuracy = 0.77

|                     | <h2>       0        </h2>    | <h2>       1        </h2>     | <h2>      2   </h2>  | <h2> avg / total </h2>|    
| :------------:      | :--------------------------: |  :--------------------------: |  :--------------------------: |  :-------------: | 
|   <h2>precision</h2>| <h3>  0.62   </h3>           |  <h3>  0.53   </h3>           |  <h3>  0.84   </h3>           |  <h3>  0.75  </h3>|   
|   <h2>recall   </h2>| <h3>  0.35   </h3>           |  <h3>  0.46   </h3>           | <h3>  0.92 </h3>           |  <h3>  0.77   </h3>| 
|   <h2>f1-score </h2>| <h3>  0.45   </h3>           |  <h3>  0.49   </h3>           | <h3>  0.88   </h3>           | <h3>  0.76   </h3>| 
|   <h2>support  </h2>| <h3>  776    </h3>           |  <h3>  2229   </h3>           | <h3>  6866    </h3>           | <h3>  9871   </h3>| 

### 증명사진

![Alt text](https://github.com/janguck/lecture/blob/master/business_intelligence/project/img/jangsion.png "Optional title")


















🛳 Titanic Survival Prediction - GDGoC Final Submission

📌 프로젝트 개요
본 프로젝트는 타이타닉 탑승자의 정보를 바탕으로 생존 여부를 예측하는 분류 문제입니다.
다양한 전처리 조합과 세 가지 모델(Logistic Regression, Random Forest, XGBoost)을 실험하고, Validation Accuracy 기준 가장 높은 성능을 낸 조합을 최종 제출로 선정하였습니다.

🔧 전처리 과정
데이터 전처리는 아래와 같은 방식으로 조합하여 실험을 진행하였습니다:

결측치 처리
Age: Pclass와 Sex 기준 그룹 평균으로 대체
Fare: Pclass 기준 평균값으로 대체
Embarked: 최빈값으로 대체

특성 엔지니어링
FamilySize: SibSp + Parch + 1 생성
AgeGroup: 나이를 카테고리(Child, Teen, Young, Adult, Senior)로 변환 (선택적 적용)
불필요한 열 제거
PassengerId, Name, Ticket, Cabin, SibSp, Parch 등

범주형 인코딩
Sex, Embarked 등은 Label Encoding
이상치 제거: Fare 열 기준으로 IQR 방식을 사용해 이상치 제거 여부 실험

🧪 실험 조합 (총 4가지 전처리 조합 × 3개 모델 = 12개 실험)
전처리 조합	AgeGroup 변환	이상치 제거	Accuracy (LogReg / RF / XGB)
①	✅	✅	0.7613 / 0.7677 / 0.7935
②	✅	❌	0.8212 / 0.8436 / 0.7877
③	❌	✅	0.8212 / 0.8436 / 0.7877
④ (최종)	❌	❌	0.8212 / 0.8436 / 0.7877

⚠️ 놀랍게도 전처리를 가장 적게 한 조합(④)이 가장 높은 정확도를 기록하였습니다.
특히 Random Forest 모델이 0.8436의 정확도를 기록하여, 최종 제출 모델로 선정하였습니다.

🏆 최종 제출 모델: Random Forest (with minimal preprocessing)
전처리: AgeGroup 변환 ❌, Fare 이상치 제거 ❌

모델: RandomForestClassifier 기본 설정

성능:

✅ Validation Accuracy: 0.8436

✅ f1-score: 0.81 ~ 0.87 (class 0/1 균형 양호)

⚙️ 추가 실험: Random Forest 하이퍼파라미터 튜닝
GridSearchCV로 최적의 파라미터를 찾았지만,
놀랍게도 기본 모델이 더 높은 성능을 보였습니다:

✅ Best Params:
{'n_estimators': 200, 'min_samples_split': 2, 'min_samples_leaf': 2, 'max_features': 'sqrt', 'max_depth': None}

❌ Accuracy: 0.8272 (기본 모델보다 낮음)

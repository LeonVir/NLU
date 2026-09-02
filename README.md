# Authorship Verification

> **Stylometric Ensemble & RoBERTa-Large Fine-tuning**
>
> COMP34812 Natural Language Understanding · Group 33

두 영어 텍스트 `text_1`과 `text_2`가 **동일한 저자에 의해 작성되었는지** 판별하는 이진 분류 프로젝트입니다. 서로 다른 방식의 두 모델을 구현하고, 문체적 특징과 문맥적 표현이 저자 동일성 판별에 미치는 영향을 비교했습니다.

## 접근 방식

### Model A - Stylometric Ensemble

단어·문자·기능어 기반 TF-IDF와 문장 길이, 구두점 사용, 어휘 다양성 등의 문체적 특징을 추출합니다. 이후 XGBoost와 LightGBM의 예측을 결합해 최종 결과를 생성합니다.

### Model C - RoBERTa-Large with Asymmetric Loss

RoBERTa-Large를 텍스트 쌍 분류 문제에 맞게 파인튜닝했습니다. Asymmetric Loss를 적용해 분류하기 어려운 샘플에 더 집중하도록 학습했습니다.

## 결과

개발 세트 5,993개 텍스트 쌍을 기준으로 평가했습니다.

| 모델 | Macro F1 | 특징 |
|---|---:|---|
| **Model A** | **0.7702** | 가볍고 CPU 추론 가능 |
| **Model C** | **0.8348** | 더 높은 성능, GPU 사용 권장 |

## 프로젝트 구성

- `Data/`: 학습·개발·테스트 데이터
- `Group_33_AV/Category_A/`: Model A 학습 및 데모 노트북
- `Group_33_AV/Category_C/`: Model C 학습 및 데모 노트북
- `Group_33_AV/NLU_Group33_Poster.pdf`: 프로젝트 포스터

실행 환경, 모델 가중치, 데이터 배치 및 재현 방법은 [상세 실행 가이드](./Group_33_AV/README.md)를 참고하세요.

## 팀

- Dongmyung Park
- Juho Kim

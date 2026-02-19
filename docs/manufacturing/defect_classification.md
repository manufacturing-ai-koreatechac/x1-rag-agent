# AI 기반 불량 분류

## 개요
딥러닝을 활용한 자동 불량 분류 시스템으로 검사 속도와 정확도를 향상시킵니다.

## 주요 알고리즘
- **CNN (Convolutional Neural Network)**: 이미지 특징 추출
- **Vision Transformer (ViT)**: 글로벌 패턴 학습
- **YOLO**: 실시간 객체 탐지 및 위치 파악

## 학습 데이터
- 정상 이미지: 10,000장
- 불량 이미지: 3,000장 (스크래치, 오염, 균열 등)
- 데이터 증강: 회전, 반전, 색상 조정으로 3배 확장

## 성능 목표
- 정확도 (Accuracy): 95% 이상
- 재현율 (Recall): 90% 이상 (불량 미검출 최소화)
- 추론 속도: 이미지당 100ms 이내

## 배포 환경
- Edge Device: NVIDIA Jetson Xavier NX
- 프레임워크: PyTorch + ONNX Runtime
- 카메라: 산업용 GigE Vision 카메라 (5MP, 30fps)
# X1-S2: 제조 데이터 파이프라인 & RAG 기초

> **[v1.7]** KOREATECH 제조AI 실무 교육과정 — X1 S2 세션 (이론 2h + 실습 2h)

## 학습 목표

- 제조 4가지 데이터 형태(센서·이미지·텍스트·MES) 파이프라인 이해
- 한국어 임베딩(ko-sroberta-multitask)으로 의미 기반 검색 구현
- 코사인 유사도 수학 원리 이해 + 히트맵 시각화
- 제조 설비 매뉴얼 RAG 챗봇 완성 (ChromaDB + LLM)

## 실습 구성

| 순서 | 내용 | 파일 | 소요 |
|:----:|------|------|:----:|
| 1 | 한국어 임베딩 생성 + 코사인 유사도 + 히트맵 시각화 | `01_korean_embeddings.ipynb` | 45분 |
| 2 | ChromaDB 벡터 DB 구축 + RAG 파이프라인 | `02_rag_system.ipynb` | 60분 |
| 3 | KAMP 가이드북 RAG 챗봇 완성 (Gradio UI) | `03_manufacturing_chatbot.ipynb` | 60분 |

**총 소요 시간**: 약 2.5시간

## X1 세션 흐름

```
S1 문제재정의 (이 repo: x1-s1-problem-reframing)
  ↓
S2 데이터파이프라인 & RAG 기초 ← 현재 리포 (x1-rag-agent)
  ↓
S3 Tool Use & 에이전트 기초 (x1-s3-rag-streamlit)
  ↓
S4 통합 에이전틱 봇 (x1-s4-agent-react) — Track A/B 신호 Tool 완전 연동
  ↓
S5 파일럿 설계서 (x1-s5-pilot-design)
```

## 시작하기

```bash
pip install -r requirements.txt
```

```bash
jupyter notebook notebooks/01_korean_embeddings.ipynb
```

## 사용 모델

| 모델 | 용도 |
|------|------|
| `jhgan/ko-sroberta-multitask` | 한국어 임베딩 (권장, MIT 라이선스) |
| `paraphrase-multilingual-MiniLM-L12-v2` | 다국어 임베딩 (오프라인 가능) |
| Claude API / Ollama Llama3 | RAG 응답 생성 |

## 데이터 소스

- **KAMP 설비 가이드북** PDF (RAG 소스 문서)
- **한국어 제조 도메인 문장 20개** (임베딩 실습용)
- Track A/B 결과 통합은 → S4(`x1-s4-agent-react`)에서 진행

## 관련 공개 데이터셋 / 문서 코퍼스

| # | 데이터셋·코퍼스 | 설명 | 형태 | 링크 |
|:-:|----------------|------|:----:|------|
| 1 | **KAMP 스마트제조 가이드북** | 한국 스마트제조 AI 플랫폼 공식 기술 문서. 설비·공정·품질 도메인 한국어 텍스트. RAG 소스 문서로 직접 활용. | PDF | [KAMP](https://www.kamp-ai.kr/front/contents/techReport.jsp) |
| 2 | **AI Hub 제조 AI 도입 사례** | 실제 제조 기업 AI 적용 사례 보고서 및 문서. 현장 용어와 맥락이 풍부한 한국어 코퍼스. RAG 검색 품질 향상에 기여. | PDF/DOCX | [AI Hub](https://www.aihub.or.kr/aihubdata/data/list.do?searchKeyword=%EC%A0%9C%EC%A1%B0) |
| 3 | **Wikipedia 제조 도메인 (한국어)** | 위키피디아 한국어 제조업·설비공학·품질관리 문서. LLM 임베딩 검증 및 도메인 적응 실습에 사용. 공개 라이선스(CC BY-SA). | 텍스트 | [HuggingFace](https://huggingface.co/datasets/wikipedia) |

## 연계 세션

- 선수: [X1-S1: 문제재정의](https://github.com/manufacturing-ai-koreatechac/x1-s1-problem-reframing)
- 후속: [X1-S3: Tool Use](https://github.com/manufacturing-ai-koreatechac/x1-s3-rag-streamlit)
- 전체: [manufacturing-ai-koreatechac](https://github.com/orgs/manufacturing-ai-koreatechac/repositories)

---
*KOREATECH 제조AI 실무 교육과정 v1.7 | X1-S2 | 2026-03-15*

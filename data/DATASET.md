# Part 3-1 데이터셋 설명

## 개요
제조 도메인 한국어 임베딩, RAG 시스템, 제조 챗봇 구축 실습. 텍스트 데이터를 노트북 내에서 동적 생성합니다.

## 데이터 생성 방식

### NB01: 한국어 임베딩 (Korean Embeddings)
- **생성 데이터**: 제조 도메인 한국어 문장 코퍼스
- **형식**: Python list → sentence-transformers 임베딩
- **내용**: 제조 용어, 설비 매뉴얼 문장, 품질 기준 문서 발췌
- **임베딩 모델**: `jhgan/ko-sroberta-multitask` (한국어 특화)

### NB02: RAG 시스템 (Retrieval-Augmented Generation)
- **생성 데이터**: 제조 도메인 지식 문서 (청크)
- **형식**: 텍스트 → ChromaDB 벡터 저장소
- **내용**: 설비 보전 매뉴얼, 품질 관리 절차, 안전 수칙 등
- **벡터 DB**: ChromaDB (outputs/chroma_db/에 저장)

### NB03: 제조 챗봇 (Manufacturing Chatbot)
- **생성 데이터**: Q&A 페어, 시스템 프롬프트
- **형식**: Python dict → LLM 프롬프트 체인
- **내용**: 제조 현장 FAQ, 설비 트러블슈팅 가이드

## outputs/ 디렉터리

| 파일 | 크기 | 설명 |
|------|------|------|
| `chroma_db/` | ~500KB | RAG 벡터 저장소 (NB02 실행 결과) |

## 필요 외부 모델

| 모델 | 용도 | 다운로드 |
|------|------|----------|
| `jhgan/ko-sroberta-multitask` | 한국어 문장 임베딩 | HuggingFace (자동) |
| `BM-K/KoSimCSE-roberta-multitask` | 유사도 비교 | HuggingFace (자동) |

> 노트북 최초 실행 시 HuggingFace에서 모델을 자동 다운로드합니다 (~500MB).
> 인터넷 연결이 필요하며, 이후 캐시되어 재다운로드 불필요합니다.

## KAMP 관련 참조 데이터

이 파트는 KAMP 센서 데이터가 아닌 **NLP/LLM 기반 제조 지식 시스템** 실습으로, 텍스트 코퍼스를 코드에서 직접 생성합니다. KAMP에서 제공하는 설비 매뉴얼 PDF 등을 활용하면 실제 환경에 가까운 RAG를 구축할 수 있습니다.

### 확장 데이터 (선택)
- **KAMP 설비 매뉴얼**: PDF 문서 → 텍스트 추출 → RAG 청크
- **제조 표준 문서**: KS 표준, 안전 수칙 등
- **설비 로그**: 실제 알람/이벤트 로그 텍스트

> 모든 기본 데이터가 노트북 내에서 코드로 생성되므로 외부 데이터 다운로드 없이 즉시 실행 가능합니다.

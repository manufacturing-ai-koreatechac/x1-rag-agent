# Part 3-1: 에이전틱 AI (Agentic AI)

> **v12 Enhanced**: 한국어 LLM + RAG + 제조 특화 AI 에이전트

---

## 🎯 학습 목표

- ✅ 한국어 오픈소스 LLM 활용
- ✅ 한국어 임베딩 및 벡터 DB
- ✅ RAG (Retrieval-Augmented Generation)
- ✅ 제조 문서 챗봇 구축
- ✅ AI 에이전트 설계 기초

---

## 📚 실습 구성

| 순서 | 실습 | 파일 | 소요 시간 | 난이도 |
|:----:|------|------|:---------:|:------:|
| 1 | 한국어 임베딩 | `01_korean_embeddings.ipynb` | 45분 | ⭐⭐ |
| 2 | RAG 시스템 구축 | `02_rag_system.ipynb` | 60분 | ⭐⭐⭐ |
| 3 | 제조 챗봇 | `03_manufacturing_chatbot.ipynb` | 60분 | ⭐⭐⭐ |

**총 소요 시간**: 약 2.5시간

---

## 🚀 시작하기

### 1️⃣ 환경 설정

```bash
# Part 3-1 폴더로 이동
cd practice-v12-enhanced/part3-1

# LLM 패키지 설치
pip install langchain langchain-community chromadb sentence-transformers

# 한국어 LLM (선택)
pip install bitsandbytes accelerate
```

---

## 📊 사용 모델

### 한국어 임베딩

| 모델 | 크기 | 용도 |
|------|:----:|------|
| `intfloat/multilingual-e5-large` | 560M | 다국어 임베딩 |
| `jhgan/ko-sroberta-multitask` | 110M | 한국어 특화 |

### 한국어 LLM

| 모델 | 크기 | 용도 |
|------|:----:|------|
| `beomi/KoAlpaca-Polyglot-12.8B` | 12.8B | 한국어 생성 |
| `nlpai-lab/KULLM3-11B` | 11B | 한국어 대화 |

---

## 🔧 실습 상세 내용

### 실습 1: 한국어 임베딩 (45분)

**학습 내용**:
- 임베딩 모델 이해
- 한국어 문장 임베딩
- 벡터 유사도 계산
- ChromaDB 사용법

**주요 코드**:
```python
from sentence_transformers import SentenceTransformer
import chromadb

# 한국어 임베딩 모델
model = SentenceTransformer('jhgan/ko-sroberta-multitask')

# 문서 임베딩
documents = [
    "CNC 가공 공정에서 절삭유의 온도가 중요합니다.",
    "베어링 불량은 주로 진동 증가로 나타납니다.",
    "품질 검사는 육안 검사와 AI 비전 검사를 병행합니다."
]

embeddings = model.encode(documents)

# ChromaDB에 저장
client = chromadb.Client()
collection = client.create_collection("manufacturing_docs")

collection.add(
    embeddings=embeddings.tolist(),
    documents=documents,
    ids=[f"doc_{i}" for i in range(len(documents))]
)

# 유사 문서 검색
query = "진동 센서 데이터 분석 방법"
query_embedding = model.encode([query])

results = collection.query(
    query_embeddings=query_embedding.tolist(),
    n_results=3
)

print("유사 문서:", results['documents'])
```

### 실습 2: RAG 시스템 구축 (60분)

**학습 내용**:
- RAG 아키텍처 이해
- 문서 청킹 및 인덱싱
- 검색 기반 생성
- 제조 매뉴얼 Q&A

**주요 코드**:
```python
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.vectorstores import Chroma
from langchain_community.embeddings import HuggingFaceEmbeddings
from langchain.chains import RetrievalQA
from langchain_community.llms import HuggingFacePipeline

# 문서 로드 및 청킹
with open('manufacturing_manual.txt', 'r', encoding='utf-8') as f:
    text = f.read()

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,
    chunk_overlap=50
)
chunks = text_splitter.split_text(text)

# 임베딩 및 벡터 DB
embeddings = HuggingFaceEmbeddings(
    model_name="jhgan/ko-sroberta-multitask"
)
vectordb = Chroma.from_texts(chunks, embeddings)

# RAG Chain
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff",
    retriever=vectordb.as_retriever(search_kwargs={"k": 3}),
    return_source_documents=True
)

# 질문
question = "CNC 가공 시 온도 관리는 어떻게 하나요?"
result = qa_chain({"query": question})

print("답변:", result['result'])
print("출처:", result['source_documents'])
```

### 실습 3: 제조 챗봇 (60분)

**학습 내용**:
- 대화형 AI 에이전트
- 컨텍스트 관리
- 멀티턴 대화
- 제조 특화 프롬프트

**주요 코드**:
```python
from langchain.memory import ConversationBufferMemory
from langchain.chains import ConversationalRetrievalChain

# 메모리
memory = ConversationBufferMemory(
    memory_key="chat_history",
    return_messages=True,
    output_key='answer'
)

# 대화형 RAG
conversation_chain = ConversationalRetrievalChain.from_llm(
    llm=llm,
    retriever=vectordb.as_retriever(),
    memory=memory,
    return_source_documents=True
)

# 챗봇 인터페이스
def chat(question):
    response = conversation_chain({"question": question})
    return response['answer']

# 대화
print(chat("진동 센서로 어떤 고장을 감지할 수 있나요?"))
print(chat("그렇다면 베어링 불량은 어떻게 알 수 있죠?"))  # 컨텍스트 유지
```

---

## 💡 학습 팁

### GPU 메모리 절약 (8-bit 양자화)

```python
from transformers import AutoModelForCausalLM, BitsAndBytesConfig

# 8-bit 로딩
quantization_config = BitsAndBytesConfig(load_in_8bit=True)

model = AutoModelForCausalLM.from_pretrained(
    "beomi/KoAlpaca-Polyglot-12.8B",
    quantization_config=quantization_config,
    device_map="auto"
)
```

### 한국어 프롬프트 템플릿

```python
from langchain.prompts import PromptTemplate

template = """
당신은 제조 공정 전문가입니다. 다음 정보를 바탕으로 질문에 답변하세요.

관련 정보:
{context}

질문: {question}

답변: 전문가로서 정확하고 실용적인 답변을 제공하세요.
"""

prompt = PromptTemplate(
    template=template,
    input_variables=["context", "question"]
)
```

---

## 📚 참고 자료

### 모델
- [KoAlpaca](https://github.com/Beomi/KoAlpaca)
- [KULLM](https://github.com/nlpai-lab/KULLM)
- [ko-sroberta](https://huggingface.co/jhgan/ko-sroberta-multitask)

### 프레임워크
- [LangChain](https://python.langchain.com/)
- [ChromaDB](https://www.trychroma.com/)
- [Sentence Transformers](https://www.sbert.net/)

---

## 🎓 학습 체크리스트

- [ ] 한국어 임베딩 모델을 사용했다
- [ ] 벡터 DB에 문서를 인덱싱했다
- [ ] RAG 시스템을 구축했다
- [ ] 제조 특화 챗봇을 만들었다

---

## 🚀 실전 프로젝트 아이디어

1. **제조 매뉴얼 Q&A 봇**
   - 수천 페이지 매뉴얼 검색
   - 자연어 질의응답

2. **불량 원인 분석 에이전트**
   - 불량 데이터 분석
   - 원인 추론 및 개선안 제시

3. **공정 최적화 어시스턴트**
   - 공정 파라미터 추천
   - 과거 데이터 기반 인사이트

---

*제조AI 교육 v12 Enhanced | Part 3-1 | 2025.02*

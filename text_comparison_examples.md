# 텍스트 비교 예

[OpenAI API 임베딩 엔드포인트](https://beta.openai.com/docs/guides/embeddings)를 사용하여 텍스트 간의 관련성 또는 유사성을 측정할 수 있습니다.

이러한 임베딩은 GPT-3의 텍스트 이해를 활용하여 비지도 학습 및 전이 학습 설정의 벤치마크에서 [첨단 결과를 달성했습니다](https://arxiv.org/abs/2201.10005).

임베딩은 시맨틱 검색, 추천, 클러스터 분석, 중복에 가까운 검색 등에 사용할 수 있습니다.

자세한 내용은 OpenAI의 블로그 게시물 공지 사항을 참조하세요.

* [Introducing Text and Code Embeddings (Jan 2022)](https://openai.com/blog/introducing-text-and-code-embeddings/)
* [New and Improved Embedding Model (Dec 2022)](https://openai.com/blog/new-and-improved-embedding-model/)

다른 임베딩 모델과 비교, see [Massive Text Embedding Benchmark (MTEB) Leaderboard](https://huggingface.co/spaces/mteb/leaderboard)

## 시맨틱 검색

임베딩은 자체적으로 또는 더 큰 시스템의 기능으로 검색에 사용할 수 있습니다.

검색에 임베딩을 사용하는 가장 간단한 방법은 다음과 같습니다.

* 검색 전(사전 계산):
  * 텍스트 말뭉치를 토큰 제한보다 작은 청크로 분할(`text-embedding-ada-002`의 경우 8,191 토큰)
  * 각 텍스트 청크 포함
  * 해당 임베딩을 자체 데이터베이스 또는 [Pinecone](https://www.pinecone.io), [Weaviate](https://weaviate.io) 또는 [Qdrant](https://qdrant.tech)와 같은 벡터 검색 공급자에 저장합니다. 
* 검색 시(라이브 컴퓨트):
  * 검색어 삽입
  * 데이터베이스에서 가장 가까운 임베딩 찾기
  * 상위 결과 반환

검색을 위해 임베딩을 사용하는 방법의 예는 [Semantic_text_search_using_embeddings.ipynb](examples/Semantic_text_search_using_embeddings.ipynb)에 나와 있습니다.

고급 검색 시스템에서 임베딩의 코사인 유사성은 검색 결과 순위 지정에서 많은 기능 중 하나로 사용될 수 있습니다.

## 질의응답

GPT-3에서 신뢰할 수 있고 정직한 답변을 얻는 가장 좋은 방법은 정답을 찾을 수 있는 소스 문서를 제공하는 것입니다. 위의 시맨틱 검색 절차를 사용하면 관련 정보에 대한 문서 코퍼스를 저렴하게 검색한 다음 프롬프트를 통해 해당 정보를 GPT-3에 제공하여 질문에 답할 수 있습니다. [Question_answering_using_embeddings.ipynb](examples/Question_answering_using_embeddings.ipynb)에서 시연합니다.

## 추천

권장 사항은 자유 형식 텍스트 쿼리 대신 입력이 집합의 항목이라는 점을 제외하면 검색과 매우 유사합니다.

추천에 임베딩을 사용하는 방법의 예는 [Recommendation_using_embeddings.ipynb](examples/Recommendation_using_embeddings.ipynb)에 나와 있습니다.

검색과 마찬가지로 이러한 코사인 유사성 점수는 자체적으로 항목의 순위를 매기거나 더 큰 순위 알고리즘의 기능으로 사용할 수 있습니다.

## 임베딩 사용자 지정

OpenAI의 임베딩 모델 가중치는 미세 조정할 수 없지만 교육 데이터를 사용하여 애플리케이션에 대한 임베딩을 사용자 지정할 수 있습니다.

[Customizing_embeddings.ipynb](examples/Customizing_embeddings.ipynb)에서 교육 데이터를 사용하여 임베딩을 사용자 지정하는 방법의 예를 제공합니다. 이 방법의 아이디어는 새로운 맞춤형 임베딩을 얻기 위해 임베딩 벡터를 곱하도록 맞춤형 매트릭스를 훈련하는 것입니다. 우수한 교육 데이터를 사용하면 이 사용자 지정 매트릭스는 교육 레이블과 관련된 기능을 강조하는 데 도움이 됩니다. 행렬 곱셈을 (a) 임베딩의 수정 또는 (b) 임베딩 간의 거리를 측정하는 데 사용되는 거리 함수의 수정으로 동등하게 고려할 수 있습니다.

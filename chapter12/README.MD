# **벡터 데이터베이스로 확장하기: RAG 구현하기**
RAG에서 데이터를 저장하고 검색할 때 저장소 역할을 하는것이 벡터 데이터베이스다. 챗GPT의 출시 이후 벡터 데이터베이스에 대한 관심이 높아지면서 
선택할 수 있는 벡터 데이터베이스의 수가 빠르게 늘고 있다.  
  
벡터 검색을 수행하는 가장 기본적인 방법은 모든 벡터 사이의 거리를 계산하는 KNN(K-Nearest Neighbor)검색이다. 하지만 KNN 검색은 데이터의 수가 
많아지면 느려지는데 이런 문제를 극복하기 위한 ANN(Approximate Nearest Neighbor)검색 방식이 있다. 다양한 ANN 검색 알고리즘 중에서 가장 많이 
활용되는 HNSW(Hierarchical Navigable Small World) 알고리즘이 있다.  
  
아래 명령어릴 실행해 라이브러리 설치  
!pip install pinecone-client==3.2.2 sentence-transformers==2.7.0 datasets==2.19.0 faiss-cpu==1.8.0 transformers==4.40.1 openai==1.25.2 llama-index==0.10.34 llama-index-vector-stores-pinecone==0.1.6  -qqq  
  


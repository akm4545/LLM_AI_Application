# **sLLM 학습하기**  
범용적인 성능이 뛰어난 상업용 LLM에 비해 비용 효율적이면서 특정 작업 또는 메도인에 특화된 sLLM이 주목받고 있다. 이번 장에서는 실습으로 자연어 
요청으로부터 적합한 SQL을 생성하는 Text2SQL(NL2SQL) sLLM을 만든다.  
  
![img.png](image/img.png)  
  
Text2SQL은 위 그림과 같이 사용자가 얻고 싶은 데이터에 대한 요청을 자연어로 작성하면 LLM이 요청에 맞는 SQL을 생성하는 작업을 말한다. 자동 완성, 
코드 리뷰 등으로 개발자의 생산성을 높여주는 깃허브 코파일럿은 LLM을 활용한 제품 중 가장 성공적인 제품으로 꼽히낟. SQL 생성을 LLM이 보조할 수 있다면 
데이터 인력의 생산성을 높일 수 있을 뿐만 아니라 현업 구성원의 SQL 진입 장벽을 낮출 수 있다는 점에서 중요한 과제라고 할 수 있다.  
  
이번 장의 실습을 마치고 나면 Text2SQL이 아닌 다른 작업에서도 sLLM 학습을 기획하고 실행할 수 있는 준비가 될 것이다. 실습 진행 전에 다음 명령을 
실행해 실습에 필요한 라이브러리를 설치한다.  
  
!pip install transformers==4.40.1 bitsandbytes==0.43.1 accelerate==0.29.3  
datasets==2.19.0 tiktoken==0.6.0 -qqq  
!pip install huggingface_hub==0.22.2 autotrain-advanced==0.7.77 -qqq  
  
# **Text2SQL 데이터셋**  
# **대표적인 Text2SQL 데이터셋**  
대표적인 Text2SQL 데이터셋으로는 WikiSQL과 Spider가 있다. SQL을 생성하기 위해서는 크게 두 가지 데이터가 필요하다. 먼저 어떤 데이터가 있는지 알 수 
있는 데이터베이스 정보(테이블과 컬럼)가 필요하다. 다음으로 어떤 데이터를 추출하고 싶은지 나타낸 요청사항(request 또는 question)이 필요하다. 
WikiSQL 데이터를 예시로 살펴면 아래 예제와 같다. 테이블(table) 항목에 테이블 이름, 컬럼 이름(header), 컬럼 형식(types)이 있고 요청(question) 
항목으로 어떤 데이터가 필요한지 요청사항이 적혀 있다. 마지막으로 SQL 항목에 정답 SQL 데이터가 있다.  
  
![img.png](image/img2.png)  
  
WikiSQL은 하나의 테이블만 사용하고 SELECT 문에 컬럼을 1개만 사용하거나 조건절에 최대 3개의 조건만 사용하는 등 비교적 쉬운 SQL 문으로 구성돼 
있다. Spider는 좀 더 현실적인 문제 해결을 위해 구축된 데이터셋으로 ORDER BY, GROUP BY, HAVING, JOIN 등 비교적 복잡한 SQL 문도 포함하고 있다. 
하지만 두 데이터셋 모두 요청사항이 영어로 되어 있어 한국어 실습을 위한 데이터셋으로는 적합하지 않다. Spider 데이터셋에 관한 더 자세한 사항은 
Spider 데이터셋 소개 페이지(https://yale-lily.github.io/spider)에서 확인할 수 있다.  
  
# **한국어 데이터셋**  
한국어 Text2SQL 데이터셋으로는 AI 허브에서 구축한 자연어 기반 질의(NL2SQL) 검색 생성 데이터가 있고 AI 허브 NL2SQL 데이터셋 페이지 링크(https://
www.aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=data&dataSetSn=71351)에서 자세한 정보를 확인할 수 있다. 
하지만 2023년 12월 시점으로 데이터 보완 작업을 위한 공개가 중단돼 있고 AI 허브에서 구축한 데이터셋은 모델 학습을 위한 목적으로 활용할 수 있으나 
사용하지 않는다. 이번 실습에서 활용한 코드를 사용해 이후에 AI 허브의 데이터로도 학습을 진행한다면 좋은 연습이 될 것이다.  
  


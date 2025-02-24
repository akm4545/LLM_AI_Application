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
  


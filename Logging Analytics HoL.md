
1. 환경 구성
   
모던 데이터플랫폼 데모 아키텍쳐
![](Pasted%20image%2020231115112427.png)

데모 시나리오
   ![](Pasted%20image%2020231115113301.png)
1) Log Group 생성
2) Logging Analytics 관리메뉴에서 Field, Parser, Source에 대해 정의
3) SCH(Service Connector Hub)생성
4) 데이터 확인(Streaming, Logging Analytics)
5) Log Explorer에서 다양한 조회에 따른 Query 저장
6) 대시보드 생성

1. 환경 구성 > 1) 리전 선택(Seoul)
   ![image](img/Pasted image 20231115112427.PNG)

1. 환경 구성 > 2) Log Groups 생성
   ![](Pasted%20image%2020231115113533.png)
   • Observability & Management > Logging Analytics > Administration
   • 개인 별 Compartment 선택
   • “Create Log Group” **클릭**
   ![](Pasted%20image%2020231115122742.png)
   • <name>_dataplatform_grp로 이름 입력
   • “Create” 클릭
   
1. 환경 구성 > 3) Service Connector 연결을 통한 Logging Analytics 연결
   
   • Connector name 입력 : <name>-sch
   • Configure service connector 지정
		- Source : Streaming  
		- Target: Logging Analytics
    • Configure source 지정
	    - Stream pool : DefaultPool
	    - Stream : crawled_stream

1. 환경 구성 > 4) Service Connector 연결을 통한 Logging Analytics 연결

![](Pasted%20image%2020231116155616.png)


![](Pasted%20image%2020231116155634.png)




![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116164043.png)


![[assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116165354.png]]

1. 데이터 확인
2. 대시보드 만들기
3. 레퍼런스
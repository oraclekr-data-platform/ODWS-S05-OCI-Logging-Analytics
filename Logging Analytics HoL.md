
### 1.환경 구성
- 모던 데이터 플랫폼 데모 아키텍처
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116204823.png)
- 데이터 플랫폼 데이터 시각화 - Logging Analytics를 활용한 분석
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116222254.png)
- **데모 시나리오
1) Log Group 생성
2) Logging Analytics 관리메뉴에서 Field, Parser, Source에 대해 정의
3) SCH(Service Connector Hub)생성
4) 데이터 확인(Streaming, Logging Analytics)
5) Log Explorer에서 다양한 조회에 따른 Query 저장
6) 대시보드 생성
___

### 1.환경 구성
##### 1. 환경 구성 > 1) 리전 선택(Seoul)
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116222440.png)
#####  1. 환경 구성 > 2) Log Groups 생성
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116222637.png)
- Observability & Management > Logging Analytics > Administration
- 개인 별 Compartment 선택
- “Create Log Group” **클릭**
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116230857.png)
   
- {name}_"dataplatform_grp로 이름 입력
- "Create" 클릭
#####  1. 환경 구성 > 3) Service Connector 연결을 통한 Logging Analytics 연결
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116231443.png)
- Connector name 입력 : {name}-sch
- Configure service connector 지정
	- Source : Streaming  
	- Target: Logging Analytics
-  Configure source 지정
	- Stream pool : DefaultPool
	- Stream : crawled_stream
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116232047.png)
- Configure target 지정 
	- Log group : {name}_dataplatform_grp
	- Log Source Identifier : lab_source (검색해서 찾기)
- Policies 지정 
	-  서비스 커넥터가 구획의 스트리밍에서 읽을 수 있도록 허용하는 default policy 생성(create 클릭)
	-  이 서비스 커넥터가 구획의 Logging Analytics에 쓸 수 있도록 허용하는 default policy 생성(create 클릭)

---
### 2.데이터 확인
##### 2. 데이터 확인 >  Log Explorer 탐색 
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116232639.png)
- Observability & Management > Logging Analytics > Log Explorer 클릭

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116232805.png)
- Log Source(lab_source)로 들어온 데이터 확인

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116232848.png)
- 상세 데이터 확인을 위해  Drill down 클릭

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116232952.png)
- 다양한 쿼리를 통해서 로그 데이터에 대한 좀 더 상세한 분석

---
### 3.대시보드 만들기
##### 3. 대시보드 만들기 > 1)Custom 대시보드 생성 
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116233333.png)
- Observability & Management > Logging Analytics > Dashboards
- “Create Dashboard” 클릭

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116233503.png)
- Dashboard 이름 입력
- “Save changes” 클릭

##### 3. 대시보드 만들기 > 2) 기본 쿼리 사용하여 Custom 대시보드에 저장 
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116233631.png)
- Observability & Management > Logging Analytics > Log Explorer
- Query bar에 다음 query 입력 : 'Log Source' = lab_source | stats count('Log Source') as 'Total livelabs’
- Visualizations에 Tile 선택

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116233718.png)
- 앞에 생성했던 Dashboard에 저장하기 위해 “Save as…” 클릭

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116233739.png)
- Search Name 입력
- “Add to dashboard” 체크박스 체크
- Select Dashboard 선택

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116233824.png)
- Query bar에 다음 query 입력 : ''Log Source' != 'OCI Audit Logs' | stats count as logrecords by 'Log Source'’
- Visualizations에서 Tree Map선택

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116233911.png)
- Search Name 입력
- “Add to dashboard” 체크박스 체크
- Select Dashboard 선택

##### 3. 대시보드 만들기 > 3) 기타 다양한 쿼리를 사용하여 Custom 대시보드에 저장 
- 같은 방식으로 아래 Query들을 활용하여 나머지 대시보드로 만듭니다
	- * | stats count as logrecords by 'Log Source’  -> Visualizations : Pie 선택
	- 'Log Source' = lab_source | stats count by 'Log Source' | rename Count as 'Total livelabs’ -> Visualizations : Pie 선택
	- 'Log Source' = lab_source | search 'python' | timestats count as logrecords by 'Log Source' | rename logrecords as 'Livelabs with python’ -> Visualizations : Line 선택
	- 'Log Source' = lab_source | cluster -> Visualizations : Cluster 선택

##### 3. 대시보드 만들기 > 4) Custom 대시보드 데이터 확인 
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116234306.png)
- “Dashboards” 클릭

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116234421.png)
- Dashboard name 클릭
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116234528.png)
- Dashboard 정렬을 위해 Edit 클릭

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116234602.png)
- 아래와 같이 마우스 드래그 앤 드랍을 통해서 이동 후 “Save changes” 클릭

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116234628.png)
- 최종 결과물 확인

---
### 4.레퍼런스
- Logging Analytics – get started  
[https://docs.oracle.com/en-us/iaas/logging-analytics/index.html](https://docs.oracle.com/en-us/iaas/logging-analytics/index.html)

- Logging analytics 관련 링크  
[https://docs.oracle.com/en-us/iaas/logging-analytics/doc/logging-analytics1.html](https://docs.oracle.com/en-us/iaas/logging-analytics/doc/logging-analytics1.html)

- Getting started with OCI Logging Analytics dashboards  
[https://blogs.oracle.com/cloud-infrastructure/post/start-oci-logging-analytics-dashboards](https://blogs.oracle.com/cloud-infrastructure/post/start-oci-logging-analytics-dashboards)

- Logging Analytics 관련 livelabs  
[https://apexapps.oracle.com/pls/apex/f?p=133:100:1226308095696::::SEARCH:logging%20analytics](https://apexapps.oracle.com/pls/apex/f?p=133:100:1226308095696::::SEARCH:logging%20analytics)


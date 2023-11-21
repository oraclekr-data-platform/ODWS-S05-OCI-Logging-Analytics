
### 0. Prerequisite
- Logging Analytics는 리전 서비스입니다.(리전마다 별도 설정)
- 시작하기 전 리전을 선택 합니다.
- User 그룹 생성, Compartment 생성, User 그룹에 대한 액세스 정책 정의와 같은 사전 필수 작업을 완료한 후 Oracle Logging Analytics에 액세스하여 사용할 수 있도록 활성화할 수 있습니다. Oracle Logging Analytics를 활성화하려면 관리자 그룹의 구성원이어야 합니다.
- Observability & Management > Logging Analytics 클릭
- 이 리전에서 서비스를 처음 사용하는 경우 서비스에 대한 높은 수준의 세부 정보와 Oracle Logging Analytics 서비스 사용을 시작하기 위한 옵션을 제공하는 온보딩 페이지로 이동하게 됩니다.
- 로깅 분석 활성화(Enable Logging Analytics) 대화 상자가 표시됩니다. 여기에는 아직 존재하지 않는 경우 최소 필수 정책 및 로그 그룹이 생성됩니다.
- 로그 수집을 시작하려면 수집 설정(Set Up Ingestion)을 클릭합니다.
- 호스트에서 지속적으로 로그를 수집하기 위해 Management Agent를 설치할 것인지 확인하세요. 또한 분석을 위해 OCI 감사 로그를 수집할지 여부를 확인할 수 있습니다. 기본 설정에 따라 정책이 생성되고 적절한 작업이 수행됩니다.
- 온보딩이 완료되면 Oracle Logging Analytics를 탐색할 수 있습니다.
   자세한 내용은 아래 링크를 참조합니다.  
   https://docs.oracle.com/en-us/iaas/logging-analytics/doc/quick-start.html
   
   https://docs.oracle.com/en-us/iaas/logging-analytics/doc/enable-access-logging-analytics-and-its-resources.html
   
   https://docs.oracle.com/en/cloud/paas/logging-analytics/logqs/#before_you_begin
   
- IAM policies 참고 사항  
  
  별도 유저 그룹을 생성하여 사용하는 경우 아래 Policies를 참고하여 설정해줘야 합니다.  
  https://docs.oracle.com/en-us/iaas/logging-analytics/doc/prerequisite-iam-policies.html#GUID-4CA8D8F4-2218-4C14-AF73-40111C459270
  
### 1.환경 구성
- 모던 데이터 플랫폼 데모 아키텍처
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116204823.png)
- 데이터 플랫폼 데이터 시각화 - Logging Analytics를 활용한 분석
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231116222254.png)

- 데모 시나리오
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

#####  1. 환경 구성 > 3) Field  정의 
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231121175030.png)
- Logging Analytics > Administration > Fields 선택
- "Create Field" 클릭

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231121175311.png)
- Name : 적절한 이름을 입력 (예: livelab_key), Data Type: String 선택
- "Create" 클릭
  
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231121175644.png)
- Name : 적절한 이름을 입력 (예: livelab_value), Data Type: String 선택
- "Create" 클릭
#####  1. 환경 구성 > 4) Parser  정의 
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231121180656.png)
- Logging Analytics > Administration > Parsers 선택
- "Create Parser" 클릭해서 JSON type 선택

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231121181202.png)
- 1) Name 입력 (예 : lab_parser)
  2) Type : JSON
  3) Example log content
	`{
	  "key": "a2V5MQ==",
         "value": "dmFsdWUx"
	}`
  4) $.key와 $.value는 정의했던 Field를 선택 (예 : $.key: livelab_key, $.value: livelab_value)

- "Create Parser" 클릭하여 생성
  
#####  1. 환경 구성 > 5) Source  정의 
![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231121182302.png)
- Logging Analytics > Administration > Sources 선택
- "Create Source" 클릭

![](assets/Logging%20Analytics%20HoL/Pasted%20image%2020231121182615.png)
- 1) Name 입력 (예 : lab_source)
  2) Source Type : File
  3) Entity Types : Host(Linux)
  4) Parser : Specific parser(s) : lab_parser
- "Create Source" 클릭
#####  1. 환경 구성 > 6) Service Connector 연결을 통한 Logging Analytics 연결
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
	- \* | stats count as logrecords by 'Log Source’  -> Visualizations : Pie 선택
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


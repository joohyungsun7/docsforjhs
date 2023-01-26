Practice of Redfish
=====

.. _Introduction:

레드피쉬는 서버, 네트워크, 스토리지, 설비 장비, 그리고 데이터센터와 클라우드 인프라와 같은 하이브리드 IT 환경의 컨버지드 인프라(CI, Converged Infrastructure*) 요소를 관리하기 위한 네트워크 표준 및 어플리케이션 프로그래밍 인터페이스(API)를 의미한다. 

.. note::

   (위키백과) Converged Infrastructure : CI는 여러 구성 요소를 하나의 최적화된 컴퓨팅 패키지로 그룹화하는 정보 기술 시스템을 구성하는 방법. 통합 인프라의 구성 요소에는 IT 인프라 관리, 자동화 및 오케스트레이션을 위한 서버, 데이터 저장 장치, 네트워킹 장비 및 소프트웨어가 포함됨

레드피쉬는 JSON 포맷과 CSDL을 사용하여 OData v4에 의해 정의된 스키마 기반의 RESTful 인터페이스를 HTTP를 통해 제공한다.

.. note::
   - RESTful: Representational State Transfer
   - HTTP: Hypertext Transfer Protocol
   - JSON: JavaScript Object Notation
   - CSDL: Common Schema Definition Language

레드피쉬는 iLO 5와 함께 상용 서버(e.g., Dell PowerEdge 13G/14G servers, Supermicro X10/X11 servers, HPE ProLiant servers)에서 사용된다. 소프트웨어에서는 OpenStack Ironic, Ansible, ManageIQ에서 사용된다. 레드피쉬는 Supermicro, Oracle, Cisco, Lenovo, Dell, HPE, Intel, Microsoft, Huawei 등 다양한 업체에 의해 지원되고 있다.

.. note::

   참조 :
    - https://www.paessler.com/it-explained/redfish

레드피쉬를 활용하게 된 배경은 다음과 같다.

1. 해당 서버의 시스템 정보를 쉽게/편하게 받아서 확인하기 위함 (특히, iLO의 MAC주소)

2. 사전에 정의된 CI 정보 추출, iLO에 CLI를 통한 접근, snmp로 데이터 수집 등 여러 방안 살펴보던 중에, iLO에서 이미 설정한 RESTful 기반의 프로토콜 연결을 확인함

3. iLO 연결 및 RESTful 요청을 통해, snmp를 통한 데이터 수집 설정, 부트 오더 확인 및 설정, iLO MAC 주소 등등 많은 활용 방안이 가능해짐

4. 해당 활용 방안(특히, 레드피쉬)이 구체화된 파이썬 기반 오픈소스를 찾았고, 이를 사용하기로 결정함

.. note::

   참조 :
    - https://hewlettpackard.github.io/ilo-rest-api-docs/ilo5/#key-benefits-of-the-ilo-restful-api
    - https://github.com/HewlettPackard/python-ilorest-library
    
Setting. 내부 정보 비공개
----------------

[bmc-ip]: 원격 관리 IP / BMC 주소 / iLO 주소?, 예시) 12.12.12.123

[bmc-usname]: 원격 관리자 이름, 예시) admin

[bmc-passwd]: 원격 관리자 비밀번호, 예시) p1234
    
Practice 1. iLO RESTful API 
----------------

가장 기본적인 확인 방법 연습
 
.. code-block:: console

   curl http://12.12.12.123/xmldata?item=all

시리얼 넘버, UUID/cUUID, 제품ID, NIC 정보 (IP, MAC 등), 펌웨어 정보, 상태 정보 등을 요청하고 그 결과값을 출력함

Practice 2. iLO RESTful API
----------------

일단, HP iLO 5에는 레드피쉬가 기본으로 설정되어 있는 것으로 보이며, 이를 테스트하면 다음과 같음

.. code-block:: console

   curl http://12.12.12.123/redfish/v1/systems/1/bios/settings/ -i --insecure -u admin:p1234 -L

Http-get요청을 통해 BIOS 설정 값을 JSON 포맷으로 받아옴. URL에 따라 다양한 값을 받아올 수 있는 것으로 보임

사전에 제공되는 URL 정보를 수집하던 중, HP에서 iLO RESTful API를 파이썬 기반 오픈소스로 제공 중임을 확인했고, 이를 테스트하기로 계획

Practice 3. 파이썬 기반 오픈소스 iLO RESTful API (테스트 일자: 2023-01-26)
----------------

`redfish`를 모듈로 설치하여 활용하는 오픈소스로, 연관된 모듈 설치가 필요하여 모듈 dependency 체크 및 테스트 진행

파이썬 기반 오픈소스 python-ilorest-library-master 깃헙(HP제공) 내용을 참조하여, `Building from zip file source`를 수행하기로 함

깃헙 다운로드 후, 설치용 zip 파일 생성

.. code-block:: console

   python setup.py sdist --formats=zip
   cd dist
   # check python-ilorest-library-4.0.0.zip (pip install 대상)

기본 python3 설치 (일반적인 경우 기반)

.. code-block:: console

   yum install python3 pip3

Installing:
 python3                 x86_64      3.6.8-10.el7        RHEL7-server-rpms       69 k
Installing for dependencies:
 python3-libs            x86_64      3.6.8-10.el7        RHEL7-server-rpms      7.0 M
 python3-pip             noarch      9.0.3-5.el7         RHEL7-server-rpms      1.8 M
 python3-setuptools      noarch      39.2.0-10.el7       RHEL7-server-rpms      629 k


`import redfish`를 위한 파이썬 모듈 목록을 작성 및 테스트 (아래 하위 항목(`1)~`)이 선행 설치되어야 함)

1. jsonpatch-1.32-py2.py3-none-any.whl

   1) six-1.16.0-py2.py3-none-any.whl
   
   2) urllib3-1.26.14-py2.py3-none-any.whl
   
   3) jsonpointer-2.3-py2.py3-none-any.whl
   
2. jsonpath-rw-1.4.0.tar.gz
  
   1) ply-3.11-py2.py3-none-any.whl
   
   2) decorator-5.1.1-py3-none-any.whl
   
3. python-ilorest-library-4.0.0.0.zip

   1) certifi-2022.12.7-py3-none-any.whl


.. code-block:: console

   # 1.
   pip3 install six-1.16.0-py2.py3-none-any.whl
   pip3 install urllib3-1.26.14-py2.py3-none-any.whl
   pip3 install jsonpointer-2.3-py2.py3-none-any.whl
   #
   pip3 install jsonpatch-1.32-py2.py3-none-any.whl

   # 2.
   pip3 install ply-3.11-py2.py3-none-any.whl
   pip3 install decorator-5.1.1-py3-none-any.whl
   #
   pip3 install jsonpath-rw-1.4.0.tar.gz

   # 3.
   pip3 install certifi-2022.12.7-py3-none-any.whl
   #
   pip3 install python-ilorest-library-4.0.0.0.zip


.. note::
   - 152K  certifi-2022.12.7-py3-none-any.whl    
   - 8.9K  decorator-5.1.1-py3-none-any.whl      
   - 13K   jsonpatch-1.32-py2.py3-none-any.whl   
   - 14K   jsonpath-rw-1.4.0.tar.gz              
   - 7.6K  jsonpointer-2.3-py2.py3-none-any.whl  
   - 49K   ply-3.11-py2.py3-none-any.whl         
   - 93K   python-ilorest-library-4.0.0.0.zip    
   - 2.2M  python-ilorest-library-master.zip     (github 전체 소스)
   - 11K   six-1.16.0-py2.py3-none-any.whl       
   - 138K  urllib3-1.26.14-py2.py3-none-any.whl  
   
Practice 4. 파이썬 기반 오픈소스 iLO RESTful API (테스트 일자: 2023-01-26)
----------------

`import redfish` 문제 없음 확인 후, 테스트용 파이썬 코드 수행


   

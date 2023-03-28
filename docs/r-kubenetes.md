# 쿠버네티스 이해

## 노드

각 노드는 컨트롤 플레인에 의해 관리되며, 
파드를 실행하는 데 필요한 서비스를 포함한다.

## 컨트롤플레인

노드(파드,컨테이너) 관리
노드의 서비스 관리

## 워크로드

쿠버네티스에서 구동되는 어플리케이션

- 워크로드는 어플리케이션, 쿠버네티스에서 구동되는,
- 파드 집합 내에서 어플리케이션이 실행됨
- 하나 이상의 컨테이너가 포함된 집합이 파드 내에서 실행됨

### Workload (내용)

https://kubernetes.io/ko/docs/concepts/workloads/

#### Pod

- Pod lifecycle
- Init Container
- Disruption
- Ephemeral Conatiner
- Pod Quality of Service Classes
- User Namespace
- Downward API

파드란...

#### Workload Resources

- Deployment
- ReplicaSet
- StatefulSet
- DaemonSet
- Job
- Automatic Cleanup for Finished Jobs
- CronJob
- Replication Controller

클러스터는 노드에 파드를 실행

Deployment: 노드에 파드를 배치 및 실행하는 부분을 의미

ReplicaSet: 파드 레플리카 집합의 실행을 관리 (안정적 유지)

(+) Deployment / ReplicaSet => Stateless 어플리케이션 지원 가능

StatefulSet: 어플리케이션의 Stateful을 관리 (+ Persistent Volume)

DaemonSet: 파드 내용 구성 및 생성 (파드 복사본 관리 및 배포가 중요, 대체로 백그라운드에서 지속적으로 수행됨)

Job: 파드 내용 구성 및 생성 (파드의 시작과 끝이 존재, 상태 추적)

(+) 잡 관리 요소들: Job + Automatic Cleanup for Finished Jobs + CronJob

Replication Controller: 파드 리플리카 실행을 관리 (설정된 파드 종류 및 수를 보장)

### 워크로드를 구동한다는 의미

- 노드 설정 및 생성
- 노드에 컨테이너가 포함된 파드를 배치 -> 컨테이너 실행됨


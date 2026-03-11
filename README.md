# Kubernetes Fundamentals

Kubernetes 입문자를 위한 단계별 실습 저장소입니다.  
`kubectl` 기반 명령형(Imperative) 실습부터 YAML 기반 선언형(Declarative) 실습까지, 핵심 리소스를 직접 다루며 학습하도록 구성했습니다.

## 프로젝트 개요
- Kubernetes 핵심 리소스(Node, Namespace, Pod, Service) 개념 이해
- `kubectl` 명령형 방식으로 리소스 생성/조회/수정/삭제
- YAML 선언형 방식으로 매니페스트 작성 및 적용
- Deployment 업데이트, 롤백, 일시중지/재개 같은 운영 패턴 실습
- Frontend/Backend 2-Tier 서비스 연결 시나리오 실습

## 이 저장소에서 배우는 것
- Pod, ReplicaSet, Deployment, Service의 역할과 연결 관계
- Label/Selector 기반 서비스 라우팅 원리
- `kubectl run/create/expose/apply/delete` 사용 흐름
- 선언형(`kubectl apply -f`)과 명령형(`kubectl run`, `kubectl expose`)의 차이
- 실습 중심으로 Kubernetes 운영 기본기(조회, 점검, 변경 이력 확인)

## 기술 스택 요약
- 오케스트레이션: Kubernetes, kubectl, k3s(학습 환경)
- 컨테이너: Docker, containerd
- 웹 계층: Nginx (정적 서빙, reverse proxy)
- 백엔드: Java, Spring Boot, Maven
- 매니페스트: YAML
- 문서 형식: Markdown, 이미지(png/mmd), 발표자료(pptx)

## 핵심 개념 빠른 정리
### Node
클러스터를 구성하는 실제 실행 머신(물리/가상)입니다.
- Control Plane Node: API Server, Scheduler, Controller 등 클러스터 제어
- Worker Node: Pod 실제 실행
- 관련 명령:

```bash
kubectl get nodes -o wide
kubectl describe node <node-name>
```

### Namespace
리소스를 논리적으로 분리하는 가상 경계입니다.
- 팀/프로젝트/환경(dev/stage/prod) 분리
- 이름 충돌 방지
- RBAC/ResourceQuota 적용 단위
- 관련 명령:

```bash
kubectl get ns
kubectl get all -n <namespace>
```

### Pod
Kubernetes의 최소 배포/실행 단위입니다.
- 하나 이상의 컨테이너를 포함
- Pod 단위 IP 부여(내부 컨테이너 간 `localhost` 통신 가능)
- 운영에서는 보통 Deployment/StatefulSet/DaemonSet이 Pod를 관리
- 관련 명령:

```bash
kubectl get pod -n <namespace> -o wide
kubectl describe pod <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace>
kubectl exec -it <pod-name> -n <namespace> -- sh
```

### Service
변동 가능한 Pod 집합에 고정된 접근 지점(IP/DNS)을 제공합니다.
- Pod 교체/재스케줄링과 무관하게 동일한 접근 경로 유지
- Selector로 Pod를 선택해 Endpoint 구성
- 주요 타입: `ClusterIP`, `NodePort`, `LoadBalancer`, `Headless`
- 관련 명령:

```bash
kubectl get svc -n <namespace>
kubectl describe svc <service-name> -n <namespace>
kubectl get endpoints -n <namespace>
```

### 한 장으로 보는 연결 구조
1. Deployment가 Pod 여러 개를 생성
2. Scheduler가 Pod를 적절한 Node에 배치
3. Service가 Label Selector로 Pod를 묶어 고정 주소 제공
4. 클라이언트는 Service로 접근하고, 트래픽은 Pod로 분산

## 학습 트랙
### 명령형(Imperative)
- `kubectl run`, `kubectl expose`처럼 즉시 명령으로 리소스를 생성
- 빠른 실습/검증에 유리

### 선언형(Declarative)
- YAML에 desired state를 정의하고 `kubectl apply -f`로 반영
- 변경 이력 관리와 운영 자동화에 유리

실무에서는 선언형 접근을 기본으로 두고, 긴급 점검이나 빠른 검증에서 명령형을 보조적으로 사용합니다.

## 전체 커리큘럼
| 번호 | 학습 내용 |
| --- | --- |
| 1 | Kubernetes 아키텍처 |
| 2 | kubectl로 Pod 다루기 |
| 3 | kubectl로 ReplicaSet 다루기 |
| 4 | kubectl로 Deployment 다루기 |
| 5 | kubectl로 Service 다루기 |
| 6 | YAML 기본 문법 |
| 7 | YAML로 Pod 생성 |
| 8 | YAML로 ReplicaSet 생성 |
| 9 | YAML로 Deployment 생성 |
| 10 | YAML로 Service 생성 |

## 폴더 구조 및 문서 요약
- [`00-Docker-Images`](./00-Docker-Images): 실습용 이미지 빌드/푸시
  - `03-kube-frontend-nginx/README.md`: Frontend Nginx 이미지 빌드와 reverse proxy 설정
- [`01-Kubernetes-Architecture`](./01-Kubernetes-Architecture): Kubernetes 구성요소와 동작 구조
- [`02-PODs-with-kubectl`](./02-PODs-with-kubectl): Pod 생성/조회/삭제, 기본 트러블슈팅
- [`03-ReplicaSets-with-kubectl`](./03-ReplicaSets-with-kubectl): 복제본 유지, 스케일링, `apply` vs `replace` 비교
- [`04-Deployments-with-kubectl`](./04-Deployments-with-kubectl): Deployment 생성/업데이트/롤백/중단·재개
- [`05-Services-with-kubectl`](./05-Services-with-kubectl): Backend(ClusterIP) + Frontend(NodePort) 연동
- [`06-YAML-Basics`](./06-YAML-Basics): YAML 문법과 매니페스트 기초
- [`07-PODs-with-YAML`](./07-PODs-with-YAML): Pod와 Service 선언형 실습
- [`08-ReplicaSets-with-YAML`](./08-ReplicaSets-with-YAML): ReplicaSet 선언형 실습
- [`09-Deployments-with-YAML`](./09-Deployments-with-YAML): Deployment 선언형 배포
- [`10-Services-with-YAML`](./10-Services-with-YAML): 2-Tier 서비스 선언형 구성

## Deployment/Service 상세 실습 단계
### 04-Deployments-with-kubectl
- Step-01: Deployment 생성/스케일링/Service 노출
- Step-02: `set image`로 업데이트
- Step-03: rollout history 확인 및 롤백
- Step-04: pause/resume로 배포 제어

### 05-Services-with-kubectl
- Step-01: Service 개요
- Step-02: Service 생성/검증

### 10-Services-with-YAML
- Step-01: Backend Deployment + ClusterIP Service
- Step-02: Frontend Deployment + NodePort Service
- Step-03: Frontend/Backend 통합 테스트

## Docker 이미지 목록
| 애플리케이션 | Docker 이미지 |
| --- | --- |
| Simple Nginx V1 | `stacksimplify/kubenginx:1.0.0` |
| Spring Boot Hello World API | `stacksimplify/kube-helloworld:1.0.0` |
| Simple Nginx V2 | `stacksimplify/kubenginx:2.0.0` |
| Simple Nginx V3 | `stacksimplify/kubenginx:3.0.0` |
| Simple Nginx V4 | `stacksimplify/kubenginx:4.0.0` |
| Backend Application | `stacksimplify/kube-helloworld:1.0.0` |
| Frontend Application | `stacksimplify/kube-frontend-nginx:1.0.0` |

## 사전 요구사항
- AWS 계정(특히 EKS 기반 실습 시)
- `kubectl` 설치
- Docker 기본 사용 경험
- Git 설치

## 시작하기
```bash
git clone https://github.com/edumgt/kubernetes-fundamentals-edumgt.git
cd kubernetes-fundamentals-edumgt
```

Git 미설치 환경에서는 아래를 참고하세요.

```bash
sudo apt update
sudo apt install -y git
git --version
```

필요 시 사용자 정보를 설정합니다.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global --list
```

## 대상 학습자
- Kubernetes를 처음 접하는 입문자
- AWS EKS 기반 운영을 준비하는 아키텍트/시스템 관리자/개발자

## 저장소 한 줄 요약
이 저장소는 **Docker 이미지 준비 → kubectl 명령형 실습 → YAML 선언형 실습 → Deployment/Service 운영 시나리오(업데이트·롤백·중단/재개)**로 이어지는 Kubernetes 기초 학습 커리큘럼입니다.

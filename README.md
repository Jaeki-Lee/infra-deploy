# ☁️ 온프레미스 기반 인프라 자동화 프로젝트

이 프로젝트는 **클라우드 비용 절감**과 **개발자 생산성 향상**을 목표로, 온프레미스 환경에 자동으로 개발/서비스 인프라를 구성하는 자동화 시스템입니다.

> Jenkins + Ansible + Docker Compose를 활용해 App, DB, Web, Monitoring 환경을 자동으로 배포합니다.

---

## 📦 프로젝트 목적

- AWS 등 클라우드 비용 절감
- 내부 인프라 자원을 활용한 하이브리드 환경 구성
- DevOps 기반 자동화 환경 구축
- 개발자는 ‘코드’에만 집중하고, 인프라는 자동화

---

## 🖥️ 인프라 구성

```
[Control Server] ── Ansible + SSH
     │
     ├── [Web Server]         : Nginx Proxy + Web1 + Web2 컨테이너
     ├── [App Server]         : Flask App1 컨테이너 + 로그
     ├── [DB Server]          : PostgreSQL Patroni(HA) + etcd + HAProxy
     ├── [Storage Server]     : GlusterFS + 앱/모니터링 로그
     └── [Monitoring Server]  : Prometheus + Grafana + Alertmanager
```

---

## 📁 디렉토리 구조

```bash
infra-deploy/
├── ansible/
│   ├── inventory.yml             # 서버 목록
│   ├── playbooks/                # 배포 플레이북
│   ├── roles/                    # 공통 역할 (common, docker)
│   └── secrets/                  # 보안 설정 (vault 등)
├── infra-components/
│   ├── app/                      # Flask App 소스
│   ├── db/                       # Patroni + HA 구성
│   ├── web/                      # Web1, Web2, Proxy + HTML
│   ├── storage/                  # GlusterFS 설정
│   └── monitoring/              # Prometheus, Grafana 설정
├── jenkins/
│   └── Jenkinsfile               # CI/CD 자동화 정의
└── team1.drawio                 # 인프라 구조 다이어그램
```

---

## 🚀 자동화 방식

### 1. Jenkins Pipeline 구성
- Git Clone → Rsync → Ansible Playbook 실행
- 서버별 SSH 키 기반 인증

### 2. Ansible Playbooks
- `deploy_control.yml`: SSH 구성 및 key 배포
- `deploy_db.yml`: DB 서버에 Patroni+etcd+HAProxy 자동 구성
- `deploy_app.yml`: Flask App 자동 배포
- `deploy_web.yml`: Proxy + Web1/2 Nginx 컨테이너 자동 구성
- `deploy_storage.yml`: GlusterFS Mount 및 마운트 경로 생성
- `deploy_monitoring.yml`: Prometheus + Grafana 자동 배포

---

## 🌐 간단한 웹 페이지 흐름

1. 사용자가 `http://web-server-ip` 접속
2. `proxy` 컨테이너가 `web1` 또는 `web2`로 분산 처리
3. `index.html`은 `/api/weather` 호출
4. `proxy → app-server → PostgreSQL`로 연결
5. 날씨 정보 조회 및 응답 반환

---

## 🧪 주요 명령어

```bash
# 컨트롤 서버에서 ansible로 배포
ansible-playbook -i inventory.yml playbooks/deploy_app.yml --ask-become-pass

# 특정 서버에서 수동 확인
curl http://localhost
docker ps
docker logs flask-app1
```

---

## ✅ 기대 효과

- 개발자는 “코드”에만 집중할 수 있는 환경 제공
- 클라우드 대비 유지비용 최소화
- 서비스 초기 단계에서 안정적인 내부 인프라 확보
- 장애 대응력 향상 (HA 구성, 모니터링 포함)

---

## ✍️ 참고

- Python Flask
- PostgreSQL + Patroni + etcd
- Docker Compose
- Nginx Proxy
- GlusterFS (로그 저장용)
- Jenkins Pipeline
- Prometheus / Grafana

---

## 🤝 팀원

- 인프라 자동화 / DevOps: `user1`
- 앱 개발: `user2`
- 모니터링 및 백업 설계: `user3`

---

> 본 리포지토리는 교육 및 사내 프로젝트 용도로 사용됩니다.

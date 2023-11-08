# movie-helm-chart

**movie-helm-chart**: Kubernetes에서 영화 프로젝트를 배포하기 위한 Helm Chart입니다.

<br>

## 영화 프로젝트 구조

- **<a href="https://github.com/howtis/movie-frontend" target="_blank">movie-frontend</a>**:
Vue 3 웹 프론트엔드 애플리케이션입니다. 영화 정보를 검색하고 표시하는 사용자 인터페이스를 제공합니다.

- **<a href="https://github.com/howtis/movie-backend" target="_blank">movie-backend</a>**:
Spring Boot 백엔드 애플리케이션입니다. 프론트엔드와 TMDb API, DB 간의 통신을 담당하여 데이터를 저장 및 프론트엔드로 제공합니다.

- **<a href="https://github.com/howtis/tmdb-java-api" target="_blank">tmdb-java-api</a>**: 
자바로 개발된 라이브러리로, 영화 프로젝트를 위해 The Movie Database (TMDb) API와 통신하여 영화 정보를 검색하고 제공하는 라이브러리입니다.

<br>

## 프로젝트 사이트
[p01076322603.site](https://p01076322603.site)

<br>

## Github Action Workflows
[deploy-to-cluster.yml](https://github.com/howtis/movie-helm-chart/blob/master/.github/workflows/deploy-to-cluster.yml)
다음과 같은 작업을 수행합니다:
```
Amazon RDS에 인증 후, 데이터베이스 상태를 확인하여 중지되어 있을 경우 다시 시작
Google Cloud Platform 인증 후, 클러스터 상태를 확인하여 존재하지 않을 경우 새로 생성
환경변수를 포함한 SecretKey가 존재하는지 확인하여 없을 경우 Github Secrets를 사용하여 새로 생성
배포된 Helm Chart가 있는지 확인 후 없다면 새로 설치, 있다면 업그레이드
```

<br>

## 테스트
개발 환경 실행을 위해 다음 도구가 필요합니다.
```
개발 환경 kubernetes 환경 테스트를 위해 skaffold를 사용하였습니다. https://skaffold.dev/docs/quickstart/

선행 설치: docker, kubectl, minikube (또는 다른 컨테이너 오케스트레이션 시스템)
```

<br>

개발 환경 실행을 위해 다음 환경변수가 설정되어야 합니다.
```
values-dev.yaml

environment: dev
datasource:
  url: [DATABASE_URL]
  port: [DATABASE_PORT]
  username: [DATABASE_USERNAME]
  password: [DATABASE_PASSWORD]
tmdb:
  apiKey: [TMDB_API_KEY]
  apiAccessToken: [TMDB_API_ACCESS_TOKEN]
recaptcha:
  sitekey: [RECAPTCHA_SITEKEY]
  secretkey: [RECAPTCHA_SECRETKEY]

# Spring Boot Backend Application
# https://github.com/howtis/movie-backend
backend:
  image:
    repository: ghcr.io/howtis/movie-backend-dev
    tag: release
    pullPolicy: Always
  service:
    port: 8080
    targetPort: 8080

# Vue.js Frontend Application
# https://github.com/howtis/movie-frontend
frontend:
  image:
    repository: ghcr.io/howtis/movie-frontend-dev
    tag: release
    pullPolicy: Always
  service:
    port: 80
    targetPort: 80
    nodePort: 31021

[DATABASE_URL], [DATABASE_PORT], [DATABASE_USERNAME], [DATABASE_PASSWORD]:
데이터베이스 정보입니다.

[TMDB_API_KEY], [TMDB_API_ACCESS_TOKEN]: 
사용하기 위해 TMDb 가입 후 API key, token 발급이 필요합니다.

[RECAPTCHA_SITEKEY], [RECAPTCHA_SECRETKEY]: 
reCAPTCHA V3 비밀 키 발급이 필요합니다.
```
[[TMDb 링크]](https://www.themoviedb.org)  
[[reCAPTCHA v3 링크]](https://www.google.com/recaptcha/admin/create?hl=ko)

- - -

### 실행

**<a href="https://github.com/howtis/movie-frontend" target="_blank">movie-frontend</a>**,
**<a href="https://github.com/howtis/movie-backend" target="_blank">movie-backend</a>** 와 같은 경로에 **movie-helm-chart** repository를 clone 한 다음,
```
minikube start (또는 다른 컨테이너 오케스트레이션 시스템을 시작합니다)
skaffold dev
```

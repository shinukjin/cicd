🚀 Spring Boot + Docker + Jenkins CI/CD 실습 프로젝트
이 프로젝트는 Windows 환경에서 WSL2(Ubuntu)를 활용하여 스프링 부트 애플리케이션의 빌드 및 배포 과정을 자동화하는 CI/CD 파이프라인 실습 내용을 담고 있습니다.

📋 시스템 환경
OS: Windows 11 (WSL2 - Ubuntu 24.04 LTS)

IDE: Cursor (VS Code 기반)

Container: Docker Desktop for Windows

CI/CD: Jenkins (Docker Container)

Language: Java 17 (OpenJDK)

🛠️ 주요 구축 단계
1. 인프라 환경 설정
WSL2 설치: Windows 터미널에서 wsl --install을 통해 Ubuntu 환경 구축.

Docker Integration: Docker Desktop 설정에서 WSL2 Ubuntu와 연동.

패키지 설치: Ubuntu 터미널에서 openjdk-17-jdk, docker.io 설치 및 사용자 권한(usermod) 설정.

2. 프로젝트 준비
코드 관리: 윈도우 경로(/mnt/c/...)에 있는 프로젝트를 WSL 환경에서 Cursor 에디터로 연동.

빌드 테스트: ./gradlew clean build를 통한 JAR 파일 생성 확인.

Dockerfile 작성: 애플리케이션을 이미지화하기 위한 명세 작성.

Dockerfile

FROM openjdk:17-jdk-slim
COPY build/libs/*-SNAPSHOT.jar app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
3. Jenkins 구축
젠킨스 구동: Docker를 사용하여 젠킨스 컨테이너 실행.

호스트의 docker.sock을 마운트하여 젠킨스 내부에서 Docker 명령어 사용 가능하도록 설정 (DooD: Docker out of Docker).

초기 설정: docker logs jenkins를 통해 초기 비밀번호 확인 후 필수 플러그인 설치 및 관리자 계정 생성.

4. CI/CD 파이프라인 설정
Jenkins Pipeline: Groovy 스크립트를 이용한 단계별 자동화 구현.

Checkout: 소스 코드 준비.

Build: ./gradlew를 이용한 프로젝트 빌드.

Docker Build: 빌드된 JAR를 포함한 도커 이미지 생성.

Deploy: 기존 컨테이너 중지 및 새 이미지로 배포 (localhost:8081).

🚀 실행 및 접속 정보
Jenkins: http://localhost:8080

Application: http://localhost:8081

⚠️ 트러블슈팅 경험
Permission Denied (Docker): 젠킨스 내부에서 도커 명령어 실행 시 권한 에러 발생 → sudo chmod 666 /var/run/docker.sock으로 해결.

Apt Fetch Error: 패키지 설치 시 404 에러 발생 → sudo apt update를 통해 패키지 리스트 최신화 후 해결.

Command Not Found (code/cursor): WSL에서 에디터 명령어를 못 찾는 문제 → WSL 확장 프로그램 설치 및 터미널 재시작으로 해결.
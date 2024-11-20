## 프론트엔드 배포 파이프라인
### 개요

<img width="308" alt="화면 캡처 2024-11-19 225337" src="https://github.com/user-attachments/assets/e92c3bd8-120d-42c5-a342-fb0f3e6e4499">

### GitHub Actions 워크플로우 배포 과정
0. 기본 설정
- main 브랜치에 커밋이 푸시될 때 실행됩니다.
- 사용자가 GitHub Actions 화면에서 수동으로 워크플로우를 실행할 수 있습니다.
- ubuntu-latest 환경에서 실행됩니다.
1. 저장소 체크아웃 (Checkout repository)
- GitHub Repository의 코드를 가져옵니다.
- 최신 버전인 v4로 업그레이드를 했습니다.
2. Node.js 설치 (Setup Node.js 20)
- 노드 버전 20으로 설치했습니다.
3. 프로젝트 의존성 설치
- package-lock.json 파일에 따라 프로젝트의 의존성을 설치합니다.
4. Next.js 프로젝트 빌드
- Next.js 프로젝트를 빌드하여 out 디렉터리에 정적 파일이 생성됩니다.
5. AWS 자격 증명 구성
- GitHub Secrets를 사용해 AWS에 접근할 수 있도록 자격 증명을 설정합니다.
- 최신 버전인 v4로 업그레이드를 했습니다.
6. 빌드 파일 S3 버킷 동기화
- out 디렉터리의 모든 정적 파일들을 S3 버킷에 동기화합니다.
- --delete 옵션으로 더 이상 존재하지 않는 파일을 삭제하여 동기화를 유지합니다.
7. CloudFront 캐시 무효화
- 변경된 파일이 즉시 사용자에게 반영되도록 CloudFront에 캐싱된 모든 파일을 무효화합니다.

## 관련 링크
- S3 버킷 웹사이트 엔드포인트: http://hanghae-fe3-16-sh.s3-website.ap-northeast-2.amazonaws.com/
- CloudFrount 배포 도메인 이름: https://d7r50d538zjrn.cloudfront.net/

## 주요 개념
### GitHub Actions과 CI/CD 도구
- CI/CD (Continuous Integration/Continuous Deployment) : 코드 변경사항을 자동으로 통합, 테스트, 배포하는 자동화된 워크플로우입니다.
- GitHub Actions : GitHub에서 제공하는 CI/CD 도구입니다. .yml 파일을 통해 워크플로우를 정의합니다.

### S3와 스토리지
- 스토리지: 데이터를 저장하고 관리하기 위한 공간이나 시스템입니다. 로컬 스토리지, 네트워크 스토리지를 포함하지만 일반적으로 요즘 스토리지라고 하면 대부분 클라우드 스토리지가 사용됩니다.
- S3 (Simple Storage Service): S3는 AWS(Amazon Web Services)에서 제공하는 클라우드 기반 대규모 객체 스토리지 서비스입니다. 정적 웹사이트 호스팅 기능을 제공합니다.

### CludFront와 CDN
- CDN (Content Delivery Network): CDN은 전 세계에 분산된 서버 네트워크를 통해 웹 콘텐츠를 사용자에게 가장 가까운 서버에서 빠르게 전송하는 시스템입니다. 빠른 로딩 속도로 사용자 경험(UX)을 증가시키고 서버 부하를 완화합니다.
- CloudFront: AWS에서 제공하는 CDN 서비스입니다. HTTPS 지원과 캐싱 기능을 지원합니다.

### 캐시 무효화(Cache Invalidation)
CDN의 엣지 서버에 캐시된 콘텐츠를 삭제하거나 갱신합니다. 새 콘텐츠가 배포될 때 사용자에게 즉시 반영합니다.

### Repository Secret과 환경변수
- Repository Secret: AWS 자격 증명, S3 버킷 이름, CloudFront 배포 ID 등 민감한 정보를 안전하게 저장하고 Actions 워크플로우에서 사용할 수 있는 기능입니다.
- 환경변수: 워크플로우 실행 중 특정 값을 참조할 수 있게 합니다.

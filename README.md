# 주요 링크

- S3 버킷 웹사이트 엔드포인트: test-cicd-action.s3-website-us-east-1.amazonaws.com
- CloudFrount 배포 도메인 이름: d22aod4f4pt6sq.cloudfront.net
- Route5s를 통해 연동된 도메인 이름 : https://www.mintchoco.store/

# 주요 개념

## GitHub Actions과 CI/CD 도구

### Github Actions
Github에서 제공하는 통합 CI/CD 서비스입니다. 레포지토리에 있는 코드를 워크플로우를 설정하여 빌드, 테스트, 린트 그리고 다양한 옵션 등을 설정할 수 있습니다.
`.github/workflows`디렉토리에 yaml파일로 정의됩니다.

### S3(스토리지)

<img src="https://github.com/user-attachments/assets/c141c7e9-0dbf-401b-b77c-754dcf68c6f5" height='200' width='200'/>

Amazon Simple Storage Service (Amazon S3)는 Amazon Web Services (AWS)가 제공하는 확장성, 보안, 성능을 갖춘 객체 스토리지 서비스입니다. S3는 사용자들이 대규모 데이터를 저장, 보호, 검색할 수 있도록 지원하며, 다양한 사용 사례에 적합합니다.

객체 스토리지: S3는 객체 스토리지 서비스로, 데이터를 "객체"라는 단위로 저장합니다. 각 객체는 데이터(실제 데이터)와 메타데이터(파일의 정보를 담을 데이터)로 구성되며, 고유한 키로 식별됩니다.

버킷: 객체를 저장하는 컨테이너 역할을 하는 논리적 단위입니다. 각 버킷은 고유한 이름을 가지며, 버킷 안에 여러 객체를 저장할 수 있습니다.


### CloudFront와 CDN


<img src="https://github.com/user-attachments/assets/99b8dbb8-3ad5-4039-a026-7cae8c6850d3" height='200' width='200'/>

CDN
콘텐츠 전송 네트워크(CDN, Content Delivery Network)는 사용자에게 빠르고 신뢰성 있게 콘텐츠를 제공하기 위해 지리적으로 분산된 서버 네트워크를 사용하는 시스템입니다. CDN은 웹 페이지, 이미지, 비디오, 스타일시트, 자바스크립트 파일 등의 정적 콘텐츠뿐만 아니라 동적 콘텐츠도 효율적으로 전달할 수 있도록 설계되었습니다.

<img src="https://github.com/user-attachments/assets/e188748a-0ddb-4625-93ed-0ba496bdeabb" />





### 캐시 무효화(Cache Invalidation)

### Repository secret과 환경변수

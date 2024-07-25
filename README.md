# CI/CD 파이프라인 다이어그램

![cicd-pipeline drawio](https://github.com/user-attachments/assets/82e0cf6e-aaba-4a95-a67d-b0c9c089a361)


# 주요 링크

- S3 버킷 웹사이트 엔드포인트: test-cicd-action.s3-website-us-east-1.amazonaws.com
- CloudFrount 배포 도메인 이름: d22aod4f4pt6sq.cloudfront.net
- Route5s를 통해 연동된 도메인 이름 : https://www.mintchoco.store/

# 주요 개념

## GitHub Actions과 CI/CD 도구

### Github Actions
Github에서 제공하는 통합 CI/CD 서비스입니다. 레포지토리에 있는 코드를 워크플로우를 설정하여 빌드, 테스트, 린트 그리고 다양한 옵션 등을 설정할 수 있습니다.
`.github/workflows`디렉토리에 yaml파일로 정의됩니다.

## S3(스토리지)

<img src="https://github.com/user-attachments/assets/c141c7e9-0dbf-401b-b77c-754dcf68c6f5" height='200' width='200'/>

Amazon Simple Storage Service (Amazon S3)는 Amazon Web Services (AWS)가 제공하는 확장성, 보안, 성능을 갖춘 객체 스토리지 서비스입니다. S3는 사용자들이 대규모 데이터를 저장, 보호, 검색할 수 있도록 지원하며, 다양한 사용 사례에 적합합니다.

### 객체 스토리지
> S3는 객체 스토리지 서비스로, 데이터를 "객체"라는 단위로 저장합니다. 각 객체는 데이터(실제 데이터)와 메타데이터(파일의 정보를 담을 데이터)로 구성되며, 고유한 키로 식별됩니다.

### 버킷
> 객체를 저장하는 컨테이너 역할을 하는 논리적 단위입니다. 각 버킷은 고유한 이름을 가지며, 버킷 안에 여러 객체를 저장할 수 있습니다.


## CloudFront와 CDN

<img src="https://github.com/user-attachments/assets/99b8dbb8-3ad5-4039-a026-7cae8c6850d3" height='200' width='200'/>

### CDN
콘텐츠 전송 네트워크(CDN, Content Delivery Network)는 사용자에게 빠르고 신뢰성 있게 콘텐츠를 제공하기 위해 지리적으로 분산된 서버 네트워크를 사용하는 시스템입니다. CDN은 웹 페이지, 이미지, 비디오, 스타일시트, 자바스크립트 파일 등의 정적 콘텐츠뿐만 아니라 동적 콘텐츠도 효율적으로 전달할 수 있도록 설계되었습니다.

<img src="https://github.com/user-attachments/assets/e188748a-0ddb-4625-93ed-0ba496bdeabb" />

### 엣지 로케이션
cdn서비스의 핵심기능으로 캐싱 서버 역할을 합니다. 리전과 별개로 엣지 로케이션이 적용되어 있기 때문에 먼 나라나 지역에서 지연없이 콘텐츠를 전송받을 수 있습니다.
-> 엣지 로케이션은 CDN서비스와 클라이언트가 만나는 지점이라고 볼 수 있습니다. 엣지 로케이션에 캐싱된 데이터가 없다면 CDN으로부터 요청을 할 것 입니다!

![image](https://github.com/user-attachments/assets/e80015f8-b6c5-402f-aa4b-446f58080743)



## 캐시 무효화(Cache Invalidation)
Cloudfront에서는 기본적으로 캐싱데이터가 TTL이 24시간입니다. 그렇기 때문에 새로운 콘텐츠를 빌드하고 업로드하여 반영하기까지 캐싱된 데이터가 갱신되어야 즉각적으로 이뤄질 수 있습니다.
위에서 알아봤듯이 캐싱된 데이터는 기본적으로 엣지 로케이션에 있으며, 요청이 들어오면 캐싱 데이터를 클라이언트에게 응답해줍니다.
그렇기 때문에 캐시데이터를 제거하는 작업이 필요합니다.

캐시 무효화 과정
1. 먼저 S3에 파일들이 동기화됩니다. (aws s3 sync 명령 사용).
2. S3 동기화가 완료되면, 즉시 다음 단계로 넘어가 CloudFront 캐시 무효화 명령이 실행됩니다.
3. 실제 캐시 무효화 후 갱신까지 시간이 소요될 수 있습니다.


## Repository secret과 환경변수
### Repository secret
주로 Git 저장소 플랫폼)에서 제공하는 기능으로, 민감한 정보를 안전하게 저장하고 사용할 수 있게 해줍니다.
직접 레포지토리에서 secret을 추가하며, 코드에 직접적으로 노출되지 않습니다. 레포지토리 관리자만 관리할 수 있으며 보통 github actions에서 사용됩니다.
```bash
jobs:
  deploy:
    steps:
      - name: Deploy to production
        env:
          DB_PASSWORD: ${{ secrets.DATABASE_PASSWORD }}
        run: |
          ./deploy-script.sh
```

### 환경변수
환경 변수는 운영 체제 수준에서 정의되는 동적 값으로, 프로세스나 사용자 세션에서 접근할 수 있습니다.
다양한 환경(prod, dev, test, stage)에서 쉽게 설정 변경이 가능합니다. 그렇기 때문에 동적으로 설정해서 주입할 수 있습니다.
```bash
jobs:
  conditional-env:
    runs-on: ubuntu-latest
    env:
      IS_MAIN: ${{ github.ref == 'refs/heads/main' }}
    steps:
      - name: Set environment for main branch
        if: env.IS_MAIN == 'true'
        run: echo "DEPLOY_ENV=production" >> $GITHUB_ENV
      - name: Set environment for other branches
        if: env.IS_MAIN != 'true'
        run: echo "DEPLOY_ENV=staging" >> $GITHUB_ENV
      - name: Use environment variable
        run: echo "Deploying to $DEPLOY_ENV"
```



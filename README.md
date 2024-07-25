## 프론트엔드 CI/CD 파이프라인

## 개요
1. 깃헙 액션은 main 브랜치에서 푸시가 발생할때 일어난다.
2. 저장소 체크아웃
3. 의존성 패키지 인스톨.
4. 빌드진행
5. 저장소에있는 secrets에 저장된 aws-key-id, access-key, region을 가지고 aws 자격증명을 구성한다.
- `${{ secrets.AWS_ACCESS_KEY_ID }}, ${{ secrets.AWS_SECRET_ACCESS_KEY }}, ${{ secrets.AWS_REGION }} 이 부분이 GitHub 시크릿을 참조`
- `aws cli,sdk가 사용할 수 있는 형태로 환경변수를 설정`
- `i am 자격 설정에서 특정 S3에 콘텐츠를 업로드하고, CloudFront를 통해 배포하며, 필요시 캐시를 무효화할 수 있도록 권한을 부여받은 키이기 때문에 가능!`
6. 빌드의 결과물을 저장하고 있는 out파일과 s3버킷과 동기화.
- `이때 sync 명령어는 차이를 비교하여 필요한 파일만 업로드 함.`
- `delete는 s3에는 있지만 로컬에 없는 파일을 삭제해줌`
7. CloudFront의 캐시를 무효화 시킨다
- `이는 클라우드 프론트자체에서 성능 향상을 위해 콘텐츠를 캐싱하여 사용하는데 이럴 경우 이전에 해당 사이트에 접근했던 사용자의 경우 새콘텐츠를 업로드 해도 캐싱된 이전 버전이 계속 제공 될 수 있기 때문에 캐시 무효화를 통해 사용자에게 이전버전이 계속 제공되는것을 방지!`
- `이러한 이유들 때문에 캐시무효화는 새로운 파일 동기화 이후에 무효화 된다.`

<img width="944" alt="스크린샷 2024-07-25 오전 12 12 14" src="https://github.com/user-attachments/assets/616b9bcc-4e7e-4768-bbf7-854d9aee8166">

## 주요 링크
- S3 버킷 웹사이트 엔드포인트: [http://hanghae-ci-cd-test.s3-website.ap-northeast-2.amazonaws.com](http://hanghae-ci-cd-test.s3-website.ap-northeast-2.amazonaws.com)
- CloudFrount 배포 도메인 이름: [https://dxr0c45o9qatc.cloudfront.net](https://dxr0c45o9qatc.cloudfront.net)

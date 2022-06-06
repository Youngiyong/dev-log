## AWS CloudFront의 Lambda@Edge로 실시간 이미지 리사이즈 구현하기

<br>

### :book: AWS CloudFront란?

* 빠른 전송 속도로 데이터, 동영상, 애플리케이션 및 API를 전 세계 고객에게 안전하게 전송하는 고속 콘텐츠 전송 네트워크(CDN, Contents Delivery Network) 서비스입니다.

<br>

### :book: Lambda@Edge란?

* AWS CloudFront의 기능 중 하나로서 애플리케이션의 사용자에게 더 가까운 위치에서 코드를 실행하여 성능을 개선하고 지연 시간을 단축할 수 있게 해 줍니다.

* 전 세계 여러 위치에 있는 인프라를 [프로비저닝](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EB%B9%84%EC%A0%80%EB%8B%9D)하거나 관리하지 않아도 됩니다.

* AWS CloudFront에 의해 생성된 이벤트에 대한 응답으로 코드를 실행합니다.

<br>

### :book: Image-Resizing 전체 구성도

|![image-resize-architecture](https://github.com/Youngiyong/dev-log/tree/main/lambda-edge-image-resize/image/Image-resize.png)|
| :----------------------------------: |
| AWS CloudFront, Lambda@Edge, S3를 사용한 Image-Resizing |

<br>


### :book: Serverless Lambda 생성
- install node 12 

- git clone 

- npm -g install serverless

- serverless config credentials --provider aws --key [aws access key] --secret [aws secret key]

- npm install --platform=linux --arch=x64 sharp --save

- npm install

- npm run build

- npm run deploy

### :book: S3 Bucket 생성하기

|![s3-bucket-policy](/image/bucket.png)|

### :book: Policy && Role 생성

```policy json 
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "iam:CreateServiceLinkedRole",
                "lambda:GetFunction",
                "lambda:EnableReplication",
                "cloudfront:UpdateDistribution",
                "s3:GetObject",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```
|![policy](https://github.com/Youngiyong/dev-log/tree/main/lambda-edge-image-resize/image/policy.png)|

|![role](https://github.com/Youngiyong/dev-log/tree/main/lambda-edge-image-resize/image/role.png)|

### :book: AWS CloudFront 배포 만들기

* Cloudfront origin identity를 생성합니다.

|![cloudfront-origin-identity](https://github.com/Youngiyong/dev-log/tree/main/lambda-edge-image-resize/image/origin-access-Identity.png)|


* Cloudfront 생성

|![cloudfront-origin](https://github.com/Youngiyong/dev-log/tree/main/lambda-edge-image-resize/image/origin.png)|

|![cloudfront-behavior](https://github.com/Youngiyong/dev-log/tree/main/lambda-edge-image-resize/image/behavior.png)|

|![cloudfront-behavior2](https://github.com/Youngiyong/dev-log/tree/main/lambda-edge-image-resize/image/behavior2.png)|

|![cloudfront-association](https://github.com/Youngiyong/dev-log/tree/main/lambda-edge-image-resize/image/association.png)|



### :bookmark: 참고

* [Cloudcraft](https://cloudcraft.co/)

* [AWS CloudFront](https://aws.amazon.com/ko/cloudfront/)

* [AWS Lambda@Edge](https://aws.amazon.com/ko/lambda/edge/)

* [AWS Lambda@edge로 실시간 이미지 리사이징](https://heropy.blog/2019/07/21/resizing-images-cloudfrount-lambda)

* [AWS Lambda@Edge에서 실시간 이미지 리사이즈 & WebP 형식으로 변환](https://medium.com/daangn/lambda-edge로-구현하는-on-the-fly-이미지-리사이징-f4e5052d49f3)

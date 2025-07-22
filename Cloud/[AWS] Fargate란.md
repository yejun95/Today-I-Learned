# AWS Fargate란
- Amazon EC2 인스턴스의 서버나 클러스터를 관리할 필요 없이 컨테이너를 실행하기위해 Amazon ECS에 사용할 수 있는 기술
  - ECS : Elastic Container Service
  - AWS에서 제공하는 완전관리형 컨테이너 오케스트레이션 서비스
  - 컨테이너의 배포,관리,확장,네트워킹을 자동화 해주는 유형
  - [박만자 : AWS ECS란](https://yoo11052.tistory.com/141)

<img width="719" height="355" alt="image" src="https://github.com/user-attachments/assets/efa103d5-35bf-4354-842c-25f0c4015cce" />
<br>
<br>

- Fargate를 사용하면 더 이상 컨테이너를 실행하기 위해 가상 머신의 클러스터를 프로비저닝, 구성 또는 조정할 필요가 없다.

- 즉, Fargate를 사용하면 컨테이너와 컨테이너 실행 환경, 이 두가지를 관리할 필요가 없는 컨테이너용 서버리스 컴퓨팅이다.
  - [Amazon ECS용 AWS Fargate](https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/AWS_Fargate.html)
<br>

<img width="841" height="333" alt="image" src="https://github.com/user-attachments/assets/207a8a22-7c35-4cd1-89d7-6a0783bfe926" />
<br>
<hr>
<br>

## ✔️ ECS-Fargate 생성하기
- 실제 생성하기에는 과금이 되므로 아래 링크 참조

- [44BITS: AWS 파게이트(AWS Fargate) 시작하기](https://www.44bits.io/ko/post/getting-started-with-ecs-fargate)
<br>
<hr>
<br>

**Reference**<br>

[클래스메소드코리아 : '초심자'무작정 해보는 AWS Fargate 환경 구축](https://m.blog.naver.com/classmethodkr/222782686010)<br>
[diligentdev: AWS Fargate란 무엇인가?](https://ddevgrit.tistory.com/16)<br>
[박만자 : AWS Fargate for ECS 시작하기](https://yoo11052.tistory.com/142)

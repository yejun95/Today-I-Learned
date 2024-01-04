- 기존에 내장 톰캣으로 실행할 때는 `application-{profile}.properties`에서 직접 포트를 설정하고 `spring.profiles.active={profile}`

  옵션을 통해 실행되는 포트를 다르게 하였다.
<br>

- 이 때문에 frontend에서 proxy 연결을 dev, prod 모드에 따라 다른 포트 번호를 주었고, backend가 dev가 되면 frontend 에서도

  mode를 development로 수동으로 계속 바꾸어 주었다.
<br>

- 하지만 외부 톰캣으로 전환하니 `server.xml`에 의해 WAS의 포트가 정해져 있는 상태이므로

  `application.properties`에 각각 설정한 포트가 의미가 없어졌다.   ->  WAS를 통해 서비스가 실행이 되니 당연히

  포트가 고정되어 있다. (해당 고정 포트는 물론 변경 가능)
<br>

- 💡 결국 properties에서 포트를 설정하던, WAS를 사용하던 서버는 1개인 것이다.

  각각 포트를 설정했던 방법도 서버가 2개가 아니라 실행될 때 마다, 서버의 포트가 바뀐 것
<br>

- 그렇다면 WAS에서의 profile 설정은 무엇을 의미하는가?

- 미리 생성해놓은 `application-{profile}.properties` 에 따라 환경 설정을 다르게 해주는 것은 동일 하지만

  포트를 다르게 하여 프로그램이 실행되는 점이 발생되지 않는 것이다. 
  
  `server.xml`에서 profile설정하는 context태그는 port를 다루는 Service 하위에 속해있다.

  ![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d50d59a9-9302-4b66-a310-5c5bbcb2bbfa)

<br>

- 그렇다면 편한 내장 톰캣을 두고 왜 외장 톰캣을 사용하는 것일까?

- 이는 하나의 WAS로 여러 서비스를 실행하기 편리하기 때문이다.

- virtual host 기능을 통해 도메인 이름에 따라 다른 루트 컨텍스트를 갖게하여 하나의 웹애플리케이션 배포만으로

  마치 여러 애플리케이션을 운영하는 것처럼 할 수 있는 것이다.

```javascript
<Host name="a.thxwelchs.com"  appBase="/webapps/weba" unpackWARs="true" autoDeploy="true" xmlValidation="false" xmlNamespaceAware="false">
</Host>
​
<Host name="b.thxwelchs.com"  appBase="/webapps/webb" unpackWARs="true" autoDeploy="true" xmlValidation="false" xmlNamespaceAware="false">
</Host>
```

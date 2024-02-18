## 2. Apache James를 이용한 메일서버 구축 - 설정
- 앞서서 James의 conf 폴더에 있는 여러 설정 파일 중에서 domainlist.xml, mailetcontainer.xml에서 도메인을(test.com) 수정해서 실행하였다.

- 여기에서는 몇 가지(알고 있는 or 중요한) 설정에 대해서 정리한다.
<br>
<hr>
<br>

### ✔ 설정 파일
- 메일을 보낼 때 사용하는 프로토콜인 SMTP (smtpserver.xml)

- 수신한 메일을 확인하기 위해 사용하는 프로토콜인 IMAP (imapserver.xml)
 
- POP3 (pop3server.xml) 프로토콜도 있지만 과거에 많이 사용하던 것으로, IMAP 사용이 권장되고 설정 방법도 비슷해서 여기에서는 정리하지 않는다.
<br>

**SMTP와 관련해서 설정하는 `smtpserver.xml`의 내용은 다음과 같다.**
- 주석제거

```javascript
<smtpservers>
   <smtpserver enabled="true">
     <jmxName>smtpserver</jmxName>
     <bind>0.0.0.0:25</bind>
     <connectionBacklog>200</connectionBacklog>
     
     <tls socketTLS="false" startTLS="false">
       <keystore>file://conf/keystore</keystore>
       <secret>yoursecret</secret>
       <provider>org.bouncycastle.jce.provider.BouncyCastleProvider</provider>
       <algorithm>SunX509</algorithm>
     </tls>
        <connectiontimeout>360</connectiontimeout>
        <connectionLimit>0</connectionLimit>
        <connectionLimitPerIP>0</connectionLimitPerIP>
        <authorizedAddresses>127.0.0.0/8</authorizedAddresses>
        <maxmessagesize>0</maxmessagesize>
        <addressBracketsEnforcement>true</addressBracketsEnforcement>
        <handlerchain>
            <handler class="org.apache.james.smtpserver.fastfail.ValidRcptHandler"/>
            <handler class="org.apache.james.smtpserver.CoreCmdHandlerLoader"/>       
        </handlerchain>           
   </smtpserver>
</smtpservers>
```

- <smtpserver>속성 enabled값이 true면 SMTP를 사용하는 것이고, false이면 사용하지 않는다 (메일을 보내고 받을  수 없다).

- <bind>의 25는 사용할 포트를 지정하는 것으로, SMTP는 25번 외에 465, 587번 포트를 사용할 수 있다.
  - 이 포트는 <tls>에서 어떤 인증 방식을 사용하는지에 따라 결정한다.
<br>

- 25번은 일반적인 통신에(socketTLS="false" startTLS="false") 많이 사용한다.

- 465, 587번은 SSL(Secure Sockets Layer)이나 TLS(Transport Layer Security)를 사용할 때 지정한다.

- 위의 포트 번호를 바꿔도 돼고, smtpserver 속성(smtpservers내에 있는) 전체를 복사해서 하나 더 만들어도 된다.

- 2개이상을 사용할 경우에는 <jmxName>의 이름을 다르게 부여해야 한다.

- 25번 포트의 <jmxName>는 smtpserver이니, 다른 포트는 smtpserver_ssl 등의 이름으로 지정한다.

- 이외에 <keystore> 속성에 인증서 파일이나 JKS 파일을 지정하고, <secret> 속성에 비밀번호를 지정해서 인증서(SSL, TLS)를 사용한다.
 
- 마지막으로 <authorizedAddresses> 속성은 인증 주소를 의미한다.
  - SMTP, IMAP등은 원격에서 메일 서버에 접속하는 것으로 아이디와 비번을 이용하여 로그인을 해야한다.
  - <authorizedAddresses>는 이러한 로그인을 하지 않아도 되는 IP주소를 의미한다.
  - 주로, 그룹웨어나 ERP등의 시스템에서 알림 메일등을 발송할때 해당 시스템 서버의 IP를 지정해서 로그인없이 메일을 발송할때 유용하다.
 
 - 이외에도 다양한 설정이 있고, 주석으로 작성되어 있지만 스팸 차단 프로그램과 연동하는 SpamAssassinHandler나 스팸 발송 업체들(IP)를 차단하는 URIRBLHandler등의 기능도 있다.
<br>
<br>

**IMAP을 설정하는 `imapserver.xml`에서는 다음과 같이 사용할 포트를 지정하고, 인증서(SSL, TLS)를 사용할지 결정한다.**
- 주석제거

```javascript
<imapservers>
    <imapserver enabled="true">
       <jmxName>imapserver</jmxName>
       <bind>0.0.0.0:143</bind>
       <connectionBacklog>200</connectionBacklog>
       <tls socketTLS="false" startTLS="false">
         <keystore>file://conf/keystore</keystore>
         <secret>yoursecret</secret>
         <provider>org.bouncycastle.jce.provider.BouncyCastleProvider</provider>
       </tls>
       <connectionLimit>0</connectionLimit>
       <connectionLimitPerIP>0</connectionLimitPerIP>
    </imapserver>
</imapservers>
```

- SMTP와 사용법은 동일하고 사용하는 포트만 143으로 차이가 있다.

- 인증서(SSL, TLS)를 사용할 경우에는 993 포트를 사용하고,  SMTP와 같이 2개의 포트를 지정할 수 있다.
<br>
<hr>
<br>

### ✔ SMTP, IMAP과 SSL(TLS)에 대한 정리
- 아웃룩, 썬더버드등의 메일 클라이언트 프로그램을 MUA(Mail User Agent)이라고 한다.

- 이런 프로그램을 이용하여 메일 작성하면 SMTP를 이용하여 메일 서버로 보내고, 해당 메일서버는 다른 메일 서버로 SMTP를 이용하여 메일을 발송한다(Mail Transfer Agent - MTA). 

- 메일 전송은 모두 SMTP 프로토콜이다.
  - 메일 서버에서 수신한 메일을 가지고 오는 것은 IMAP/POP3 프로토콜이다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/3e1bb29e-18fa-45de-9032-874310bd908c)
<br>
<br>

- 여기에 별도로 정리하는 것은 인증서 사용에 대해서 혼동하는 경우가 많기 때문이다.

- 당연이 IMAP/POP3 에서 인증서를 지정하면, 서버에서 메일을 가지고 올때 지정된 인증서로 암호화 해서 가지고 온다.

- 그리고, SMTP에 인증서를 지정하면, 지정된 인증서로 메일을 암호화해서 메일 서버로 전송한다.

- 하지만, 이 메일을 다른 메일 서버로 보낼 때에는 상대 서버에 지정된 인증서를 사용한다.
  - 내 인증서로 암호화를 하면 상대방이 복호화를 할 수 없다.
  - 공개키, 비밀키와 비슷한 개념이라고 보면 된다.
<br>

- 자신의 메일 서버에 지정된 인증서로 암호화해서 보내는 것이 아니고, 상대 서버의 인증서로 암호화해서 상대 서버로 보내게 된다.

- 자신의 서버에 인증서를 지정하지 않아도 보안 메일을 보낼 수 있는데, 자신의 서버에 인증서를 설치하지 않아서 보안 메일을 사용하지 않는 경우가 있다
  - 이 내용은 다음의 mailetcontainer.xml 정리에서 gmail을 대상으로한 예제에서 확인할 수 있다.
<br>

**💡💡💡 mailetcontainer.xml**
- 제일 중요하기 때문에 세번 강조를 해도 모자라다.

- James가 메일을 처리하는 과정을 통제하는 파일로써 플러그인을 개발하여 부족한 기능을 구현할 수도 있다.

- 여러가지 조건들은 [Apach James 문서를](https://james.apache.org/server/3/dev-provided-matchers.html) 참고하면 된다.

- 여기서는 중요사항 몇가지만 정리한다.
<br>
<br>

**먼저, 다음 match를 제거해야 외부로 메일을 보낼 수 있다.**
- 주석 처리해도 무방

```
<mailet match="RemoteAddrNotInNetwork=127.0.0.1" class="ToProcessor">
<processor>relay-denied</processor>  
<notice>550 - Requested action not taken: relaying denied</notice>
</mailet>
```

- 내부(LocalDelivery)는 현재 James 메일 서버에 등록된 도메인의 계정끼리 메일을 주고 받는 것을 의미한다.

- 외부(RemoteDelivery)는 Gmail등의 다른 메일서버로 메일을 보내는 것을 의미한다.

- 외부주소가 127.0.0.1이 아니면 relay-denied라는 processor로 보내서 메일을 발송하지 않고 종료한다.

- 위에서 match를 주석처리하였다면 이제 실제 메일서비스를 진행해보자.

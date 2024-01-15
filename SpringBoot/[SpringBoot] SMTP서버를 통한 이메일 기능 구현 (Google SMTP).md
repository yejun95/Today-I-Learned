## Google SMTP 서버를 이용한 이메일 기능 구현
- [SMTP란?](https://github.com/yejun95/Today-I-Learned/blob/master/Network/SMTP%EB%9E%80%3F.md) 
<br>

### ✔ JavaMailSender
- 스프링 프레임워크에서 제공하는 **인터페이스** 중 하나로, 이를 사용하여 Java에서 이메일을 보내는 기능을 구현할 수 있다.

- `org.springframework.mail.javamail` 패키지에 속해 있으며, 해당 패키지내에 클래스를 사용하여 구현한다.
<br>
<hr>
<br>

### ✔ pom.xml
- dependency 추가

- `org.springframework.mail.javamail.JavaMailSender` 해당 패키지를 사용하기 위함

```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```
<br>
<br>

- 패키지를 import하고  `JavaMailSender` 클래스를 생성하여 내부를 보면 `MailSender`를 상속받고 있다.
  - `JavaMailSender javaMailSender = new JavaMailSender();`
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/942e3e37-f746-4ccb-9ac0-7cdfac13d7a6)
<br>
<hr>
<br>

### ✔ JavaMailSender와  MailSender 차이
**MailSender**
- 이메일을 전송하는 간단한 메서드만을 정의

- 구체적인 이메일 전송 라이브러리에 의존하지 않고, 스프링이 제공하는 간단한 인터페이스를 통해 이메일을 보내고자 할 때 사용

- 텍스트 이메일만을 전송하는 것에 목적이 있다.
  - 이 때문에 내부 클래스를 보면 `SimpleMailMessage` 클래스를 사용하고 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/36459a54-8eea-491b-865d-33938105110e)
<br>
<br>

**JavaMailSender**
- MailSender를 상속받기 때문에 텍스트 전송이 가능하다.

- MIME 타입의 메시지, 첨부 파일, HTML 이메일 등을 지원한다.
  - MIME : Multipurpose Internet Mail Extensions
  - 이메일과 함께 동봉할 파일을 텍스트 문자로 전환해서 이메일 시스템을 통해 전달
<br>
<hr>
<br>

### ✔ application.properties SMTP 셋팅
- application.properties 파일에 메일 설정 정보를 추가해야한다.

- Gmail SMTP Server를 사용하려면 요구사항에 맞는 설정이 필요하며, 그 내용은 다음과 같다.<br>
  - [참고](https://www.siteground.com/kb/gmail-smtp-server/)<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/70356510-7173-4d8d-b755-4c159197327f)
<br>
<br>

- 1~6번은 구글에서 명시한 SMTP 설정을 적은 것이다.

```
# 이메일 발송 정보 세팅
spring.mail.host=smtp.gmail.com 
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.username=발신자의 구글 이메일주소
spring.mail.password=발급받은 앱 비밀번호 12자리
spring.mail.port=587
spring.mail.transport.protocol=smtp
spring.mail.debug=true
spring.mail.default.encoding=UTF-8
spring.mail.mime.charset=UTF-8
```

- spring.mail.host : 이메일을 보낼 때 사용할 SMTP 서버의 호스트를 지정

- spring.mail.properties.mail.smtp.auth : SMTP 서버에 인증을 사용할 것인지를 지정
  - Gmail을 사용하는 경우 반드시 인증이 필요하므로 true로 설정
<br>

- spring.mail.properties.mail.smtp.starttls.enable : STARTTLS 암호화를 사용할 것인지를 지정
  - . Gmail은 보안 연결을 위해 STARTTLS를 사용하므로 true로 설정
<br>

- spring.mail.username : SMTP 서버에 인증을 할 때 사용할 이메일 주소를 지정
 
- spring.mail.password : 서버에 인증을 할 때 사용할 이메일 계정의 비밀번호를 지정 (발급받은 앱 비밀번호 12자리)

- spring.mail.port : SMTP 서버의 포트를 지정
  - Gmail의 경우 일반적으로 587 포트를 사용
<br>

- spring.mail.transport.protocol : 이메일 전송에 사용될 프로토콜을 지정
  - SMTP를 사용하므로 smtp로 설정
<br>

- spring.mail.debug : 디버그 모드를 활성화할지 여부를 지정
  - 디버그 모드를 활성화하면 이메일 전송과 관련된 디버그 정보가 로그에 기록
<br>

- spring.mail.default.encoding : 기본 인코딩 설정

- spring.mail.mime.charset : MIME 인코딩에 사용될 문자 집합 설정
<br>
<hr>
<br>

### ✔ 구글 보안 설정
- 구글 설정에서 '나와 Google의 관계' -> 'Google 계정 관리' 탭 클릭

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/585f71af-7b50-4019-98b3-522ed1ef97ed)
<br>
<br>

- Google 계정관리 창이 새로 켜졌다면 검색창에 '앱 비밀번호' 입력 후 클릭

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/da218cf4-e6ab-43a5-9399-fe7ed00639ed)
<br>
<br>

- 앱 비밀번호의 이름을 임의로 입력 후 만들기 클릭

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/5c589e78-af41-4eb8-babc-3ae0a9bafae4)
<br>
<br>

- 만들기 클릭 시 앱 비밀번호가 발급되며, 이를 잘 저장해놓아야 한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/8f705d28-0673-4d49-b954-b40292328be2)
<br>
<hr>
<br>

### ✔ DTO 생성
```
// EmailMessage.java

public class EmailMessage {
	
	private String to;
	
	private String subject;
	
	private String message;

	public String getTo() {
		return to;
	}

	public void setTo(String to) {
		this.to = to;
	}

	public String getSubject() {
		return subject;
	}

	public void setSubject(String subject) {
		this.subject = subject;
	}

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}		
}
```
<br>
<hr>
<br>

### ✔ Controller 생성
```
// EmailController

@RestController
@RequestMapping(value = "/api")
public class EmailController {
	
	@Autowired
	EmailService emailService;
	
	@PostMapping("/sendMail")
	public ResponseEntity sendMail() {
		EmailMessage emailMessage = new EmailMessage("xodlf100@gmail.com", "테스트 메일 제목", "테스트 메일 본문");
        emailService.sendMail(emailMessage);
        return new ResponseEntity(HttpStatus.OK);
	}
	
}
```

- `sendMail` Post 요청 때, 고정된 데이터를 보내게 된다.
  - EmailMessage 생성자의 역할은 수신인, 제목, 내용 순서로 저장되어 보낸다.
<br>

- HTTP 상태 코드를 관리하기 위하여 ResponseEntity 사용
  - HttpStatus의 OK 함수는 200을 리턴
<br>
<hr>
<br>

### ✔ Service 생성
```
// EmailService.java

@Service
public interface EmailService {
	
	public void sendMail(EmailMessage emailMassage);
}
```
<br>
<hr>
<br>

### ✔ ServiceImpl 생성
```
@Service
public class EmailServiceImpl implements EmailService {
	
	@Autowired
	JavaMailSender javaMailSender;
	
    public void sendMail(EmailMessage emailMessage) {
        MimeMessage mimeMessage = javaMailSender.createMimeMessage();
        try {
            MimeMessageHelper mimeMessageHelper = new MimeMessageHelper(mimeMessage, false, "UTF-8");
            mimeMessageHelper.setTo(emailMessage.getTo()); // 메일 수신자
            mimeMessageHelper.setSubject(emailMessage.getSubject()); // 메일 제목
            mimeMessageHelper.setText(emailMessage.getMessage(), false); // 메일 본문 내용, HTML 여부
            javaMailSender.send(mimeMessage);
            System.out.println("메시지 전송 성공");
        } catch (MessagingException e) {
            System.out.println("메시지 전송 실패!!!!!");
            throw new RuntimeException(e);
        }
    }
}
```
<br>
<hr>
<br>

### ✔ api 테스트
- postman을 이용해 api를 테스트한다.

- 기본으로 설정했던 제목과 본문이 들어간다.

- 발신자와 수신자가 동일하기 때문에 내가 보냈다고 확인이 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/3bebd255-db69-4bcf-9af7-bfeb6f6cef80)
<br>
<hr>
<br>

**Reference**<br>

[june.log : Spring MailSender (SMTP)](https://velog.io/@injoon2019/Spring-MailSender-STMP)<br>
[Gwangyong Jeong : [Spring] SMTP 서버를 이용한 이메일 발송](https://dev-aiden.com/spring/Spring-%EC%9D%B4%EB%A9%94%EC%9D%BC-%EB%B0%9C%EC%86%A1/)<br>


# SMTP (메일 서비스 적용)

## POP vs IMAP

### POP
- 메일 서버에서 메일을 받아오는 프로토콜 중 하나로, 메일이 PC 로컬 저장소에 저장됩니다.
- 로컬 PC 저장소에 저장된 후에 메일 서버에선 사라집니다.
- 주로 메일 서버의 용량이 적을 때 사용됩니다.

### IMAP
- 동일하게 메일을 받아오는 프로토콜이지만, 메일 서버와 동기화가 이루어집니다.
- 모든 장치에서 동일한 이메일을 확인할 수 있습니다.
- 주로 메일 서버의 용량이 많을 때 사용됩니다.

# code 

## 00. gradle

```

// [Mail]
implementation 'org.springframework.boot:spring-boot-starter-mail:3.0.0'  // Spring Boot 3.0.x 버전
```



## 01. Config

```
@Configuration
public class MailConfig {

    // 01. yml 에서 읽어드림
    @Value("${spring.mail.username}")
    private String mailUsername;
    @Value("${spring.mail.password}")
    private String mailPassword;

    // 02. 빈으로 등록
    @Bean
    public JavaMailSender NaverMailService(){
        JavaMailSenderImpl javaMailSender = new JavaMailSenderImpl();

        javaMailSender.setHost("smtp.naver.com");
        javaMailSender.setUsername(mailUsername);
        javaMailSender.setPassword(mailPassword);
        javaMailSender.setPort(465); // SMTP 포트
        javaMailSender.setJavaMailProperties(getMailProperties()); // 메일 인증서버 가져오기

        return javaMailSender;
    }

    // 메일 인증서버 정보 가져오기
    private Properties getMailProperties(){
        Properties properties = new Properties();
        properties.setProperty("mail.transport.protocol", "smtp"); // 프로토콜 설정
        properties.setProperty("mail.smtp.auth", "true"); // smtp 인증
        properties.setProperty("mail.smtp.starttls.enable", "true"); // smtp strattles 사용
        properties.setProperty("mail.debug", "true"); // 디버그 사용
        properties.setProperty("mail.smtp.ssl.trust", "smtp.naver.com"); // ssl 인증 서버 (smtp 서버명)
        properties.setProperty("mail.smtp.ssl.enable", "true"); // ssl 사용

        return properties;
    }
}
```
- 프로퍼티스 또는 야물 파일에서 읽어드린다. 
- getMailProperties()의 경우, 저렇게 두는게 위험하지는 않을까 싶음

## 02. interface 
```
@Service
public interface MailServiceInter {

    // 메일 내용 작성
    MimeMessage creatMessage(String to) throws MessagingException, UnsupportedEncodingException;

    // 랜덤 인증코드 생성
    String createKey();

    // 메일 발송
    String sendSimpleMessage(String to) throws Exception;

}
```

## 03. 구현 (서비스)
```
@Service
public class MailService implements MailServiceInter {

    @Autowired
    private JavaMailSender emailsender;  // Bean 등록해둔 MailConfig 를 emailsender 라는 이름으로 autowired


    private String ePw; // 인증번호


    // 메일 내용 작성
    @Override
    public MimeMessage creatMessage(String to) throws MessagingException, UnsupportedEncodingException {

        MimeMessage message = emailsender.createMimeMessage(); // 깊은 코드
        MimeMessageHelper messageHelper = new MimeMessageHelper(message, "UTF-8"); // 얇은 코드

        messageHelper.setTo(to);
        messageHelper.setSubject("화석연료 커뮤니티 이메일 인증");

        // 이메일 HTML 템플릿
        String msgg = "<!DOCTYPE html>";
        msgg += "<html lang='ko'>";
        msgg += "<head>";
        msgg += "<meta charset='UTF-8'>";
        msgg += "<meta name='viewport' content='width=device-width, initial-scale=1.0'>";
        msgg += "<title>이메일 인증</title>";
        msgg += "<style>";
        msgg += "body { font-family: Arial, sans-serif; background-color: #f9f9f9; margin: 0; padding: 0; }";
        msgg += ".container { max-width: 600px; margin: 50px auto; background: white; border-radius: 10px; overflow: hidden; box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1); }";
        msgg += ".header { background: #FEE500; padding: 20px; font-size: 24px; font-weight: bold; color: #3c1e1e; }";
        msgg += ".content { padding: 30px; }";
        msgg += ".title { font-size: 18px; font-weight: bold; margin-bottom: 10px; }";
        msgg += ".description { font-size: 14px; color: #555; margin-bottom: 20px; }";
        msgg += ".info-box { background: #f5f5f5; padding: 15px; border-radius: 5px; font-size: 16px; }";
        msgg += ".info-box p { margin: 5px 0; }";
        msgg += ".footer { font-size: 12px; color: #777; text-align: center; padding: 15px; border-top: 1px solid #eee; }";
        msgg += ".code { font-size: 22px; font-weight: bold; color: #333; }";
        msgg += "</style>";
        msgg += "</head>";
        msgg += "<body>";
        msgg += "<div class='container'>";
        msgg += "<div class='header'>fossilfuel 계정</div>";
        msgg += "<div class='content'>";
        msgg += "<p class='title'>화석연료 커뮤니티 등록을 위한 인증번호입니다.</p>";
        msgg += "<p class='description'>아래 인증번호를 확인하여 이메일 인증을 완료해 주세요.</p>";
        msgg += "<div class='info-box'>";
        msgg += "<p><strong>연락처 이메일:</strong> " + to + "</p>";
        msgg += "<p><strong>인증번호:</strong> <span class='code'>" + ePw + "</span></p>";
        msgg += "</div>";
        msgg += "</div>";
        msgg += "<div class='footer'>";
        msgg += "본 메일은 발신전용입니다.<br>";
        msgg += "화석연료 관련하여 궁금한 점이 있으면 <a href='https://github.com/adorahelen'>개발자</a>를 확인해 보세요.<br>";
        msgg += "Copyright © Kangmin Kim. All rights reserved.";
        msgg += "</div>";
        msgg += "</div>";
        msgg += "</body>";
        msgg += "</html>";

        messageHelper.setText(msgg, true); // HTML 설정
        messageHelper.setFrom(new InternetAddress("adorahelenmin@naver.com", "화석연료"));

        return message;
    }

    // 랜덤 인증 코드 전송
    @Override
    public String createKey() {
        StringBuffer key = new StringBuffer();
        Random rnd = new Random();

        for (int i = 0; i < 8; i++) { // 인증코드 8자리
            int index = rnd.nextInt(3); // 0~2 까지 랜덤, rnd 값에 따라서 아래 switch 문이 실행됨

            switch (index) {
                case 0:
                    key.append((char) ((int) (rnd.nextInt(26)) + 97));
                    // a~z (ex. 1+97=98 => (char)98 = 'b')
                    break;
                case 1:
                    key.append((char) ((int) (rnd.nextInt(26)) + 65));
                    // A~Z
                    break;
                case 2:
                    key.append((rnd.nextInt(10)));
                    // 0~9
                    break;
            }
        }

        return key.toString();
    }


    // 메일 발송
    @Override
    public String sendSimpleMessage(String to) throws Exception {
         ePw = createKey(); // 선언해놓은 변수, 랜덤 인증번호 할당

        MimeMessage message = creatMessage(to); // 메일 발송준비
        try {
            emailsender.send(message); // 메일 발송
        } catch (Exception es) {
            es.printStackTrace();
            throw new IllegalArgumentException("Failed to send email");
        }

        return ePw; // 메일에 동봉한 인증코드 서버에 반환
    }

}
```

- 이렇게까지 길 필요는 없다. 
- 01 메일 내용 작성에서 길어졌다. (html)
- 02 랜덤 인증 코드 작성하는 부분 
- 03 메일 발송하는 부분 

## 04. 컨트롤러 
```
@RestController
@RequestMapping(value = "/api/mail")
public class MailRestController {

    @Autowired
    MailService registerMail;

    @PostMapping(value = "/confirm.json")
    public String mailConfirm(@RequestParam(name = "email") String email) throws Exception{

        String code = registerMail.sendSimpleMessage(email);

        System.out.println("사용자에게 발송한 인증코드 ==> " + code);

        return code;
    }
}
```
- 프론트측에서 하단 url 고려해서, 메일 인증 버튼 구현
  * @RequestMapping(value = "/api/mail")
  * @PostMapping(value = "/confirm.json")

- 메일 발송



## DB 암복화 
✅ 해당 부분은 이 [사이트](https://prohannah.tistory.com/80?category=870127)를 참조하여서 작성을 하였습니다. 
✅ 암호화에는 SHA나 SEED등 여러가지가 있는데, 이 페이지에서는 CBC 운영모드의 SEED 기반 암 복호화 프로세스에 대해서 설명을 하겠다.
  ```
  SEED의 선택 이유 
    - 블록암호 대칭키 알고리즘이다. 
    - KISA에서 권고하고 국가정보원 검증 알고리즘이기에 가장 큰 이유이다. 
  ```    
  - 처음 사용할 때에 kisa 사이트에서 seed관련된 알고리즘을 다운받은 후에 프로젝트에 파일을 만들어서 추가를 해주자.
  - 적용후에 테스트코드를 아래와 같이 작성해준다. 
  ```java 
  
  
public class Seed {
    private static final byte[] pbszUserKey = "testCrypt2020!@#".getBytes();
    private static final byte[] pbszIV = "1234567890123456".getBytes();
 
    public static byte[] encrypt(String rawMessage) {
        byte[] message = rawMessage.getBytes(StandardCharsets.UTF_8);
        byte[] encryptedMessage = KISA_SEED_CBC.SEED_CBC_Encrypt(pbszUserKey, pbszIV, message, 0, message.length);
        return encryptedMessage;
    }
 
    public static String decrypt(byte[] encryptedMessage) {
        byte[] decryptedMessage = KISA_SEED_CBC.SEED_CBC_Decrypt(pbszUserKey, pbszIV, encryptedMessage, 0, encryptedMessage.length);
        return new String(decryptedMessage, StandardCharsets.UTF_8);
    }
}


@Slf4j
public class SeedTest {
 
    private final byte[] pbszUserKey = "testCrypt2020!@#".getBytes();
    private final byte[] pbszIV = "1234567890123456".getBytes();
 
    @Test
    public void 암복호화_테스트() {
        // given
        String rawMessage = "테스트 데이터";
        log.info("원본 데이터 =>" + rawMessage);
 
        // when
        byte[] encryptedMessage = KISA_SEED_CBC.SEED_CBC_Encrypt(pbszUserKey, pbszIV, rawMessage.getBytes(), 0, rawMessage.getBytes().length);
        log.info("암호화된 데이터1 => " + new String(encryptedMessage));
        log.info("암호화된 데이터2 => " + encryptedMessage.toString());
 
        byte[] decryptedMessage = KISA_SEED_CBC.SEED_CBC_Decrypt(pbszUserKey, pbszIV, encryptedMessage, 0, encryptedMessage.length);
        log.info("복호화된 데이터 => " + new String(decryptedMessage));
 
        // then
        assertThat(rawMessage).isEqualTo(new String(decryptedMessage));
        assertThat(rawMessage).isNotEqualTo(encryptedMessage);
    }

❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗     

- 절대 pbszUserKey와 pbszIV는 유출되면 안된다. 

- 보통 암복화한 Byte 결과값은 String으로 변환하여서, DB에 저장을 해주어야한다.      
         이럴 때에는 바로 string으로 변환하는것이 아닌 Base64의 encoder/decoder를 호출해서 암/복호화 하고나서     
         String 변환 형식을 사용해야만 원하는 string으로 변환이 가능하다. 이 방식을 사용하지 않으면 문자가 다 깨져서 줄력이된다. 
         
- 암복화하는 부분을 함수형 클래스화를 시키면 아래와 같이 조금 더 깔끔한 구조로 튜닝이 가능하다. 

- 아래와 같이 하면, 호출 시에 조금 더 가독성 좋게 호출할 수 있다. 

❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗❗ 

@Slf4j
public class Seed {
 
    private static final Charset UTF_8 = StandardCharsets.UTF_8;
    private static final byte[] pbszUserKey = "testCrypt2020!@#".getBytes();
    private static final byte[] pbszIV = "1234567890123456".getBytes();
 
    public static String encrypt(String rawMessage) {
        Encoder encoder = Base64.getEncoder();
 
        byte[] message = rawMessage.getBytes(UTF_8);
        byte[] encryptedMessage = KISA_SEED_CBC.SEED_CBC_Encrypt(pbszUserKey, pbszIV, message, 0, message.length);
 
        return new String(encoder.encode(encryptedMessage), UTF_8);
    }
 
    public static String decrypt(String encryptedMessage) {
        Decoder decoder = Base64.getDecoder();
 
        byte[] message = decoder.decode(encryptedMessage);
        byte[] decryptedMessage = KISA_SEED_CBC.SEED_CBC_Decrypt(pbszUserKey, pbszIV, message, 0, message.length);
 
        return new String(decryptedMessage, UTF_8);
    }
}

@Slf4j
class SeedTest {
 
    private final byte[] pbszUserKey = "testCrypt2020!@#".getBytes();
    private final byte[] pbszIV = "1234567890123456".getBytes();
 
    @Test
    public void SEED_Custom_암복호화_테스트() {
        // given
        String rawMessage = "테스트 데이터";
        log.info("원본 데이터 =>" + rawMessage);
 
        // when
        String encryptedMessage = Seed.encrypt(rawMessage);
        log.info("암호화 데이터 =>" + encryptedMessage);
 
        String decryptedMessage = Seed.decrypt(encryptedMessage);
        log.info("복호화 데이터 =>" + decryptedMessage);
 
        // then
        assertThat(rawMessage).isEqualTo(decryptedMessage);
        assertThat(rawMessage).isNotEqualTo(encryptedMessage);
    }
}

  ```    
  - 적용하는 과정은 사내 프로젝트에서 SEED 검색해보면 로직을 따라갈 수 있다. (오늘해봄 11.25) 



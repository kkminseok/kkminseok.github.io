## 개요

이번에 회사에서 Kafka구축 및 Spring Boot와 통합을 진행하면서 의문이 든게 있다.

Kafka Consumer에서 역직렬화를 수행할 때 객체를 자동으로 변환해줄 수 없는가?에 대한 의문이였다.

무슨 의미인지 먼저 살펴보겠다.

## 문제 살황

Spring-Boot-Kafka 공식 샘플 예제를 받아오면 다음과 같이 코드가 구성되어있다. 

```java
public record Foo2(String foo) {
}

public record Bar2(String bar) {
}
```

데이터를 주고받을 Record(공식 문서에서는 Class)를 정의해준다.

그리고 예제용으로 데이터를 주고받는걸 손쉽게 하기위해 컨트롤러를 작성해준다.

```java
@RestController
@RequiredArgsConstructor
public class ProducerController {

    private final KafkaProducerService kafkaProducerService;

    @PostMapping(path = "/send/foo/{what}")
    public void sendFoo(@PathVariable String what) {
        kafkaProducerService.sendMessage("foos", new Foo2(what));
    }

    @PostMapping(path = "/send/bar/{what}")
    public void sendBar(@PathVariable String what) {
        kafkaProducerService.sendMessage("bars", new Bar2(what));
    }

}
```

각 요청이 들어오면 객체로 바꿔서 Kafka 메시지를 쏴준다.

실제 쏴주는 Service는 이렇게 구성되어있다.

```java
@Slf4j
@Service
@RequiredArgsConstructor
public class KafkaProducerServiceImpl implements KafkaProducerService {
    private final KafkaTemplate<Object, Object> kafkaTemplate;

    public void sendMessage(String topic, Object message) {
        log.info("topic:{},message:{}", topic, message);
        kafkaTemplate.send(topic, message);
    }

}
```

딱히 볼 거 없이 도픽과 메시지를 받아서 데이터를 쏴주고 있다.

리스너는 다음과 같이 구성되어있다.

```java
@Slf4j
@Component
@KafkaListener(id = "multiGroup", topics = {"foos", "bars"})
public class MultiMethods {

    private final ExecutorService exec = Executors.newVirtualThreadPerTaskExecutor();

    @KafkaHandler
    public void foo(Foo2 foo) {
        log.info("Received Foo2: {}", foo);
        terminateMessage();
    }

    @KafkaHandler
    public void bar(Bar2 bar) {
        log.info("Received Bar2: {}", bar);
        terminateMessage();
    }

    @KafkaHandler(isDefault = true)
    public void unknown(Object unknown, @Headers Map<String, Object> headers) {
        log.info("Received unknown: {}", unknown);
        log.info(headers.toString());
        terminateMessage();
    }


    private void terminateMessage() {
        this.exec.execute(() -> log.info("Hit Enter to terminate..."));
    }
}
```

각 객체를 가지하여 로그를 뿌려주고 있는데 각 객체를 인지할 수 있도록 샘플 예제는 다음과 같은 컨버터를 구성한다.

```java

@Bean
public RecordMessageConverter converter() {
    JsonMessageConverter converter = new JsonMessageConverter();
    DefaultJackson2JavaTypeMapper typeMapper = new DefaultJackson2JavaTypeMapper();
    typeMapper.setTypePrecedence(Jackson2JavaTypeMapper.TypePrecedence.TYPE_ID);
    typeMapper.addTrustedPackages("com.common");
    Map<String, Class<?>> mappings = new HashMap<>();
    mappings.put("foo", Foo2.class);
    mappings.put("bar", Bar2.class);
    typeMapper.setIdClassMapping(mappings);
    converter.setTypeMapper(typeMapper);
    return converter;
}

//토픽 없을시 생성
@Bean
public NewTopic foos() {
    return new NewTopic("foos", 1, (short) 1);
}

@Bean
public NewTopic bars() {
    return new NewTopic("bars", 1, (short) 1);
}

```


Convert부분을 보면 객체로 역직렬화하기위해 신뢰가능한 패키지를 적어주고 `Map`자료형을 써서 클래스 정보를 넣어주고 있다.

## 의문

만약 Producer와 Consumer에서 Core에 존재하는 객체를 Kafka를 통해서 주고받을때, 오브젝트 정의가 생성될때마다 kafka Consumer 역직렬화 정보를 바꿔줘야하나?

그런 클래스가 100개가 생긴다면? 수동으로 바꾸기엔 무리가 있을거라 생각했다.
동적으로 구성하고 싶었다.

즉,

```java
		Map<String, Class<?>> mappings = new HashMap<>();
		mappings.put("foo", Foo2.class);
		mappings.put("bar", Bar2.class);
		typeMapper.setIdClassMapping(mappings);
```

이부분을 동적으로 구성하고 싶었다.

## 변경 전

먼저 위의 코드를 삭제한채로 요청을 보내보겠다.
즉, 

```java

@Bean
public RecordMessageConverter converter() {
    JsonMessageConverter converter = new JsonMessageConverter();
    DefaultJackson2JavaTypeMapper typeMapper = new DefaultJackson2JavaTypeMapper();
    typeMapper.setTypePrecedence(Jackson2JavaTypeMapper.TypePrecedence.TYPE_ID);
    typeMapper.addTrustedPackages("com.common");
    converter.setTypeMapper(typeMapper);
    return converter;
}
```

Map에 데이터를 넣어주지 않은 채로 foo 요청(/send/foo/test)을 보내보았다.


```bash
2025-04-11T15:07:44.549+09:00  INFO 6326 --- [Spring-Boot-Kafka-Producer] [ucer-producer-1] org.apache.kafka.clients.Metadata        : [Producer clientId=Spring-Boot-Kafka-Producer-producer-1] Cluster ID: pL2OQtu3SwWRWstobZqEfA
2025-04-11T15:07:44.551+09:00  INFO 6326 --- [Spring-Boot-Kafka-Producer] [ucer-producer-1] o.a.k.c.p.internals.TransactionManager   : [Producer clientId=Spring-Boot-Kafka-Producer-producer-1] ProducerId set to 22 with epoch 0
2025-04-11T15:07:45.632+09:00  INFO 6326 --- [Spring-Boot-Kafka-Producer] [ultiGroup-0-C-1] o.a.k.c.c.internals.LegacyKafkaConsumer  : [Consumer clientId=consumer-multiGroup-3, groupId=multiGroup] Seeking to offset 20 for partition foos-0
2025-04-11T15:07:45.637+09:00  INFO 6326 --- [Spring-Boot-Kafka-Producer] [ultiGroup-0-C-1] o.s.k.l.KafkaMessageListenerContainer    : Record in retry and not yet recovered
2025-04-11T15:07:46.681+09:00  INFO 6326 --- [Spring-Boot-Kafka-Producer] [ultiGroup-0-C-1] o.a.k.c.c.internals.LegacyKafkaConsumer  : [Consumer clientId=consumer-multiGroup-3, groupId=multiGroup] Seeking to offset 20 for partition foos-0
2025-04-11T15:07:46.681+09:00  INFO 6326 --- [Spring-Boot-Kafka-Producer] [ultiGroup-0-C-1] o.s.k.l.KafkaMessageListenerContainer    : Record in retry and not yet recovered
```

형 변환에 실패해서 내가 구성한 fallback전략에 따라 요청이 실패함을 알 수 있다.




## 고민

Java Reflection를 사용해야하나? 클래스정보를 동적으로 받아오기위해서는 어떻게 해야할까?

여러 서치를 해봤을때 명확한 해답을 주지 않았지만 힌트는 얻을 수 있었다.

여기서 사용하고 있는 `DefaultJackson2JavaTypeMapper`는 `toJavaType()` 함수를 통해 형변환을 진행하고 있다.

Header라는 값을 통해 클래스 정보를 얻고, 해당 클래스의 정보가 일치하는지 Java Reflection를 통해서 해당 클래스로 변환해준다. 
즉, 이 함수를 잘 오버라이딩하면 문제를 해결할 수 있을거라 생각했다.


## 생성자 수정

Header값을 보고 형변환을 진행하므로 생산자쪽에서 헤더에 클래스정보를 넣어줘야한다.

```java
    public void sendMessage(String topic, Object message) {
        log.info("topic:{},message:{}", topic, message);
        ProducerRecord<Object, Object> record = new ProducerRecord<>(topic,message);
        //com.my.springbootkafkaproducer.model.Foo2
        log.info("class name: {}", message.getClass().getName());
        //99, 111, 109, 46, 109, 121, 46, 115, 112, 114, 105, 110, 103, 98, ....
        log.info("class name2: {}", message.getClass().getName().getBytes(StandardCharsets.UTF_8));
        record.headers().add("__TypeId__", message.getClass().getName().getBytes(StandardCharsets.UTF_8));
        kafkaTemplate.send(record);
    }
```

코드를 보면 클래스경로를 헤더에 __TypeID__를 키값을 가지게 하고 넣는다. 
이제 이를 수신하는 소비자쪽에서 DefaultJackson2JavaTypeMapper 클래스를 상속받는 클래스를 만들어서 오버라이딩해본다.

```java

@Slf4j
public class DynamicMapper extends DefaultJackson2JavaTypeMapper {

    @Override
    public JavaType toJavaType(Headers headers){

        String typeIdHeader = this.retrieveHeaderAsString(headers, this.getClassIdFieldName());
        if (typeIdHeader != null) {
            try {
                String className = typeIdHeader;
                Class<?> clazz = Class.forName(className);
                log.info("clazz: {]", clazz.toString());
                return TypeFactory.defaultInstance().constructType(clazz);
            } catch (ClassNotFoundException e) {
                throw new IllegalArgumentException("Cannot find class for type id: " + typeIdHeader, e);
            }
        }
        return super.toJavaType(headers);
    }
}
```

헤더에 있는 `__TypeId__`키 값을 파싱하여 해당 값의 경로에 있는 클래스로 형변환한다.

이제 이 클래스를 사용하도록 convert쪽도 수정해준다.

```java
@Bean
public RecordMessageConverter converter() {
    JsonMessageConverter converter = new JsonMessageConverter();
    DynamicMapper typeMapper = new DynamicMapper();
    typeMapper.setTypePrecedence(Jackson2JavaTypeMapper.TypePrecedence.TYPE_ID);
    typeMapper.addTrustedPackages("com.common");
    converter.setTypeMapper(typeMapper);
    return converter;
}
```

이러면 작업은 끝난다. 한 번 실행해보고 아까와 같은 요청을 보내보자.

만약 성공한다면 아까와 같은 Retry구문은 뜨지 않을 것이다.

```bash
2025-04-11T15:15:20.131+09:00  INFO 6736 --- [Spring-Boot-Kafka-Producer] [ultiGroup-0-C-1] c.m.s.mapper.DynamicMapper               : init : com.my.springbootkafkaproducer.model.Foo2
2025-04-11T15:15:20.131+09:00  INFO 6736 --- [Spring-Boot-Kafka-Producer] [ultiGroup-0-C-1] c.m.s.mapper.DynamicMapper               : headers: RecordHeaders(headers = [RecordHeader(key = __TypeId__, value = [99, 111, 109, 46, 109, 121, 46, 115, 112, 114, 105, 110, 103, 98, 111, 111, 116, 107, 97, 102, 107, 97, 112, 114, 111, 100, 117, 99, 101, 114, 46, 109, 111, 100, 101, 108, 46, 70, 111, 111, 50])], isReadOnly = false)
2025-04-11T15:15:20.131+09:00  INFO 6736 --- [Spring-Boot-Kafka-Producer] [ultiGroup-0-C-1] c.m.s.mapper.DynamicMapper               : foo class name: com.my.springbootkafkaproducer.model.Foo2
2025-04-11T15:15:20.131+09:00  INFO 6736 --- [Spring-Boot-Kafka-Producer] [ultiGroup-0-C-1] c.m.s.mapper.DynamicMapper               : clazz: {]
2025-04-11T15:15:20.154+09:00  INFO 6736 --- [Spring-Boot-Kafka-Producer] [ultiGroup-0-C-1] c.m.s.service.MultiMethods               : Received Foo2: Foo2[foo=test]
```

정말 고맙게도 Retry 명령어는 뜨지 않는다. 나는 리스너를 `MultiMethods`라는 클래스안에 두었기에 해당 클래스에서 로그가 잘 출력됨을 볼 수 있다.

이로써 문제 자체는 해결되었다.

하지만 더 나아가서 해야할 일이 있다.

## 나아가서

현재 생산자의 코드가 더럽다. 메시지를 생성할떄 저 sendMessage함수 하나만 쓰면 발전시킬 필요는 없지만 좀 더 앞단에서 공통된 소스는 진행되었으면한다.

이또한 전처리를 진행하는 유틸 클래스를 하나 더 두거나 KafkaTemplate 클래스를 직접 상속받아서 `send()`함수를 오버라이딩하는 방식 등이 있지만 이는 다음에 기회가 되면 진행해보겠다.


[예제코드](/https://github.com/kkminseok/baeldung-test/tree/main/Spring-messaging/Spring-Boot-Kafka-Producer)
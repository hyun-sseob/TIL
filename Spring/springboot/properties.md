# ๐  Spring boot properties


## ๐ง๐ปโ๐ป ํ๋กํผํฐ ์ ์ฉ ์ฐ์ ์์.
- ์ฐ์ ์์๋ ์๋ ๋งํฌ ์ฐธ์กฐ.
- https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config

## ๐ง๐ปโ๐ป Type safety ํ๋กํผํฐ ๋ง๋ค๊ธฐ !

```java
@Component
@ConfigurationProperties("sseob")
@Validated
public class SeobProperties {
	private String name;
	private int age;
	private String fullName;

    // @DurationUnit(ChronoUnit.SECONDS) ํ๋กํผํฐ ํ์ผ์ 30s๋ผ๊ณ  ์ ์ด์ฃผ๋ฉด ์ด ์ด๋ธํ์ด์์ ์๋ต ๊ฐ๋ฅํ๋ค.
	private Duration sessionTimeout = Duration.ofSeconds(30); // ๊ธฐ๋ณธ๊ฐ 30์ด
    ... (์๋ต)getter, setter
}
```

- `@ConfigurationProperties` ์ด๋ธํ์ด์์ ํตํด ๋ง๋ค ์ ์๋ค.
    - ํ๋กํผํฐ๋ฅผ ์ค์ ํ๋ค๋ `@EnableConfigurationProperties` ์ด๋ธํ์ด์์ ์ค์ ๊ณผ ๊ฐ์ด ์ฌ์ฉ ๋์ด์ผ ํ์ง๋ง, springboot application์์๋ ์๋ต ๊ฐ๋ฅํ๋ค.
    - prefix๊ฐ `sseob`์ธ property๊ฐ์ ์ฝ์ด์ ๋งคํํ๋ค.
    - ํ๋กํผํฐ ๋งคํ Class์์ ์ค์ ๋ prefix๊ฐ์ properties ํ์ผ์์ ์ฌ์ฉํ  ๋์ IDE์ ์ ๊ทน์ ์ธ ์ง์์ ๋ฐ์ ํธ๋ฆฌํ๊ฒ ์ด์ฉ ๊ฐ๋ฅํ๋ค.
    - ์นด๋ฉ์ผ์ด์ค, ์ค๋ค์ดํฌ ์ผ์ด์ค, ์ผ๋ฐฅ ์ผ์ด์ค ๋ฑ ๋ค์ํ text ๋ฌธ์ ํ์์ผ๋ก properties ํ์ผ์์ ์์ฑํด๋ ์ ์ ํ ๋งคํ๋๋ค.
        > ex) fullName, full-name, full_name, FULLNAME -> ๋ชจ๋ fullName ํ๋๋ก ๋งคํ๋๋ค.

- @Component ์ด๋ธํ์ด์์ ํตํด `Bean`์ผ๋ก ๋ฑ๋ก์ํฌ ์ ์๋ค.
    - ์ด๋ ๊ฒ `Bean`์ผ๋ก ๋ง๋ ๋ค๋ฉด ์ด๋์๋  ์ฃผ์๋ฐ์ ์ฌ์ฉ ๊ฐ๋ฅํ๋ค.
    - ๋งคํ๋ ํ๋กํผํฐ Class๊ฐ ์๋ ๊ฒฝ์ฐ์๋ @Value ์ด๋ธํ์ด์์ผ๋ก ํ๋กํผํฐ ๊ฐ์ ์ฌ์ฉ ํ๋๋ฐ, ์ด๋ ๊ฒ ๋งคํ Class๋ฅผ ๋ง๋ค์ด ๋์ผ๋ฉด @Value ์ด๋ธํ์ด์์ ๋์ฒดํ์ฌ ์ฌ์ฉ ๊ฐ๋ฅํ๋ค. ์ด๋ฏธ ๋ชจ๋  ๊ฐ๋ค์ด ํด๋น prefix์ ๋งคํํ์ฌ Bean์ผ๋ก ๋ง๋ค์ด ๋จ์ผ๋ ์ฃผ์๋ฐ์ ์ฌ์ฉํ๋ฉด ๋๋ค.

- ํ๋กํผํฐ ๊ฐ์ ๋งคํํ  ๋์ @Validated ์ด๋ธํ์ด์์ ํตํด validation check๋ ๊ฐ๋ฅํ๋ค !
- ํ๋กํผํฐ ๊ฐ์ ๋งคํํ  ๋์ Type conversion์ด ์ ์ฉ๋๋ค.
    > ex) `sseob.age = 10` -> 10์ด๋ผ๋ ์ซ์๋ properties file์์ ๋จ์ํ text์ด์ง๋ง `int`๋ก ํ์์ด ๋ฐ๋์ด ๋งคํ๋๋ค.

    > ex) `sseob.sessionTimeout = 20s` -> properties file์์ s๋ผ๋ seconds๋ฅผ ์์งํ๋ ๋ฌธ์๋ฅผ ๋ถ์ฌ์ฃผ๋ฉด Duration ํ์์ผ๋ก ๋งคํ๋๋ค.

    > ex) `sseob.sessionTimeout = 20` -> ์ด ๊ฒฝ์ฐ์๋ ssesionTimeout field์ `@DurationUnit(ChronoUnit.SECONDS)` ๋ผ๊ณ  ๋ช์ํด์ผ๋๋ค.


## ๐ Profile๊ณผ ํจ๊ป

> ์ฌ๋ฌ๊ฐ์ง ํ๋กํ์ผ์ ๋ง๋ค์ด ๋๊ณ , program argument ๋๋ application.properties์ ์ค์ ํ์ฌ ์ ํ์ ์ธ ํ๋กํผํฐ ๊ฐ์ ์ฌ์ฉํ  ์ ์๋ค.

> profile์ด ์ ์ฉ๋ application properties๊ฐ ์ฐ์ ์์๊ฐ ๋ ๋๋ค.

> ์ ํ์ ์ธ Bean์ค์ ์ ์ฌ์ฉํ  ์ ์๋ค.

```java
@Configuration
@Profile("hyun")
public class HyunProfileConfig {
	
	@Bean
	public String hello() {
		return "hello hyun !";
	}
}

@Configuration
@Profile("sseob")
public class SeobProfileConfig {
	
	@Bean
	public String hello() {
		return "hello sseob !";
	}
}
```

- `application properties` hyun profile ํ์ฑํ ์ค์ .
```xml
sseob.fullName= hyunSseob
spring.profiles.active=hyun
```

- `application-sseob.properties` sseob.fullName๊ฐ ์ค์ .
```xml
sseob.full-name=sseob profile was changed sseob full name
```

- `application-hyun.properties` sseob.fullName๊ฐ ์ค์ .
```xml
sseob.full-name=hyun profile was changed sseob full name
```

```java
/**
 * ํ๋กํ์ผ ์ค์ ์ ๋ฐ๋ผ ์ด๋ป๊ฒ ๋์ํ๋์ง ํ์ธ
 */
@Component
public class ProfileRunner implements ApplicationRunner {

	@Autowired
	private SeobProperties seobProperties;

	// profile config๋ฅผ ํตํด ์์ฑํ bean ์ฃผ์๋ฐ์
	@Autowired
	private String hello;

	@Override
	public void run(ApplicationArguments args) throws Exception {
		System.out.println("============ProfileRunner============");
		System.out.println(hello);
		System.out.println(seobProperties.getFullName());
		System.out.println("============ProfileRunner============");
	}
}
```

### ์ถ๋ ฅ
1. profile ํ์ฑํ ์ค์ ์ ๋ฐ๋ผ `HyunProfileConfig` Class์ Bean์ด **์ฃผ์** ๋์์์ ์ ์ ์๋ค.
2. ํ๋กํผํฐ ์ ์ฉ ์ฐ์ ์์์ ๋ฐ๋ผ sseob.fullName ๊ฐ์ด application-hyun.properties ํ์ผ์ ๊ฐ์ผ๋ก ์ ์ฉ๋๊ฒ์ ํ์ธํ  ์ ์๋ค. 
```console
=============ProfileRunner=============
hello hyun !
hyun profile was changed sseob full name
=============ProfileRunner=============
```

### ํ๋กํ์ผ ๋ณ๊ฒฝํ๊ธฐ
- application.properties ํ์ผ์ ์ค์ ์ ํตํด ๋ณ๊ฒฝํ  ์ ์์ง๋ง ๋ค๋ฅธ ๋ฐฉ๋ฒ๋ ์๋ค.
```xml
spring.profiles.active=hyun
```

- program argument ์ฌ์ฉํ๊ธฐ.
    - ํจํค์ง ํ jar ํ์ผ์ ์คํํ  ๋์ java -jar sseob.jar --spring.profiles.active=hyun
    - program argument ๋ํ ํ๋กํผํฐ ์ ์ฉ ์ฐ์ ์์์ ๋ฐ๋ผ ์ ์ฉ๋๋ค.

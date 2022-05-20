---
title: unquoted character ((ctrl-char code 13))
date: 2022-05-20
tags:
- spring
---

외부 api를 조회하여 데이터를 끌어오는 스케쥴러가 있었는데, 일부 데이터가 누락되는 이슈가 발생했다.

api를 호출하는 코드는 대충 다음과 같다.

```Java
.
.
.
ResponseEntity<List<CustomDTO>> response = restTemplate.exchange(
                                            url,
                                            HttpMethod.GET,
                                            new HttpEntity<>("", httpHeaders),
                                            new ParameterizedTypeReference<List<CustomDTO>>() {});
.
.
.
return response.getBody();                             
```
특정한 파라미터도 없었고, 브라우저에서 domain url로 붙어도 json data가 보이는 지극히 단순한 코드였는데 

unquoted character ((ctrl-char code 13)) 오류가 간헐적으로 발생하고 있었다.
   
json valid 사이트에서 확인해보니, 다음과 같이 속성 값에 줄바꿈 때문에 다음 줄부터 오류가 발생했다.

```text
{ "data" : "abc
def"
}
```

나는 다음과 같이 [ObjectMapper](https://www.baeldung.com/spring-boot-customize-jackson-objectmapper)를 사용해 해결했다.

```Java
.
.
ObjectMapper objectMapperForAllowUnquotedControlChars = new ObjectMapper()
            .configure(JsonParser.Feature.ALLOW_UNQUOTED_CONTROL_CHARS, true);
.
.
.
.
ResponseEntity<String> response = restTemplate.exchange(
                                            url,
                                            HttpMethod.GET,
                                            new HttpEntity<>("", httpHeaders),
                                            String.class);

return objectMapperForAllowUnquotedControlChars.readValue(result.getBody()
                                            , new TypeReference<List<CustomDTO>>(){});
```

# Rest request to query an endpoint

## advices

- use `exchange`
- use a `URIBuilder`
- set headers in a base class
- inject a `restTemplate`

## Base Class

```java
protected abstract class AbstractBaseController {
    
    protected final HttpHeaders headers = new HttpHeaders() {{
        this.set("Accept", MediaType.APPLICATION_JSON_VALUE);
        this.setContentType(MediaType.APPLICATION_JSON);
    }};
}
```

## Get request

```java
class MyController extends AbstractBaseController {

    ResponseEntity<ExpectedResponse> requestSomething() {
        var result = restTemplate.exchange(
        uriBuilder.toUriString(),
        HttpMethod.GET,
        request,
        ExpectedResponse.class);
    }

}
```
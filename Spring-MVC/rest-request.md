# Rest request to query an endpoint

## advices

- use `exchange`
- use a `URIBuilder`
- set headers in a base class
- inject a `restTemplate`

## Base Class

```java
protected abstract class AbstractBaseController {

    private OAuth2RestTemplate restTemplate;
    private final Logger logger = LogManager.getLogger(this.getClass());

    public AbstractController(OAuth2RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    protected final HttpHeaders headers = new HttpHeaders() {{
        this.set("Accept", MediaType.APPLICATION_JSON_VALUE);
        this.setContentType(MediaType.APPLICATION_JSON);
    }};

    protected <T> Optional<T> exchange(UriComponentsBuilder uriBuilder, HttpMethod method, @Nullable HttpEntity<?> requestEntity, ParameterizedTypeReference<T> responseType) {
        try {
            var result = restTemplate.exchange(uriBuilder.toUriString(), method, requestEntity, responseType);
            return result.getStatusCode().isError() && !result.hasBody() ? Optional.empty() : Optional.of(result.getBody());
        } catch (RestClientException ex) {
            logger.error(ex.getMessage(), ex);
            return Optional.empty();
        }
    }
}
```

## Get request

```java
class MyController extends AbstractBaseController {

    Optional<ExpectedResponse> requestSomething() {
        try {
            var result = restTemplate.exchange(
            uriBuilder.toUriString(),
            HttpMethod.GET,
            request,
            ExpectedResponse.class);
            return result.getStatusCode().isError() || !result.hasBody() ? Optional.empty() : Optional.of(result.getBody());
        } catch (RestClientException ex) {
            logger.error(ex.getMessage(), ex);
            return Optional.empty();
        }
    }
}
```
# API & Web Service Security Checklist
Checklist of things to consider and implement to protect your exposed web services and applications

## Transport
- [ ] Use HTTPS using Certificate from Trusted CA for Client access
- [ ] Rate Limit requests based on client finger printing, API key, or IP
- [ ] Use `HSTS` header with SSL to avoid SSL Strip attack.
- [ ] Use encryption on all transport between client and other backend services

## Authentication
- [ ] Don't use Basic Auth for public/exposed enpoints. Use oAuth/OpenID.
- [ ] Use account lock out

### JSON Web Token (JWT)
- [ ] Use a randomly generated long key also refered to as the `JWT Secret`
- [ ] Don't extract the algorithm from the header. Algorithm should be known, forced and strong such as `HS256` or `RS256`)
- [ ] Every request should contain the token and token must be validated everytime
- [ ] Token validation must be synchronous and fail by default
- [ ] Token validation must check expiry and signature
- [ ] Access token should have short TTL and Refresh should be implemented
- [ ] JWT Payload should be light and not contain sensitive information
- [ ] JWT access/role information must be validated

### OAuth Services/Servers
- [ ] Use a already tested and developed solution such as Keycloak
- [ ] Validate `redirect_uri` to allow whitelisted URLs.
- [ ] Validate Web-Origins to allow whitelisted URLs.
- [ ] Consider Authorisation code instead of exposing tokens.
- [ ] Use `state` with a random hash to prevent CSRF during authentication process.
- [ ] Define the default scope, and validate scope parameters for each application.

## Input
- [ ] Do not allow payloads bigger than expect
- [ ] Sanitise all input validating type, length and content to avoid common vulnerabilities; `XSS`, `SQL-Injection`, etc.)
- [ ] Use the proper HTTP method according to the operation: `GET`, `POST`, `PUT/PATCH`, `DELETE`
- [ ] Validate `content-type` on request Accept header (Content Negotiation) to allow only your supported format; `application/json`, etc.)
- [ ] Validate `content-type` of posted data as you accept; `application/x-www-form-urlencoded`, `application/json`, etc.).
- [ ] Sensitive data must not be contained in URL. 
- [ ] Consider obfuscation of URL strings.
- [ ] Consider API Gateway services to enable caching, Rate Limit policies

## Processing
- [ ] Server to communcation must validate sessions and/or tokens
- [ ] Use UUID instead of incremental IDs
- [ ] If you are parsing XML files, make sure entity parsing is not enabled to avoid `XXE` (XML external entity attack).
- [ ] If you are parsing XML files, make sure entity expansion is not enabled to avoid `Billion Laughs/XML bomb` via exponential entity expansion attack.
- [ ] Use a CDN/Object Storage for file uploads, and do not not allow upload to the API service/server

## Output
- [ ] Send `X-Content-Type-Options: nosniff` header.
- [ ] Send `X-Frame-Options: deny` header.
- [ ] Send `Content-Security-Policy: default-src 'none'` header.
- [ ] Remove server identifty headers - `X-Powered-By`, `Server`, `X-AspNet-Version`, etc.
- [ ] Force `content-type` for your response. E.g set `content-type` to `application/json` if thats what the content is.
- [ ] Return the proper status code according to the operation completed; `200 OK`, `401 Unauthorized`, `405 Method Not Allowed`, etc.

## Logging
- [ ] Log Success and Failures, the damage is in the successful execution!
- [ ] Halt processing if Audit/Logging fails
- [ ] User high perfomring logging solution such as logstash & elasticserach

## Infrastructure
- [ ] Avoid HTTP Blocking by using workers, queues and a scale out infrastructure.
- [ ] User network base IPS system
- [ ] Load test to understand your bottle necks and capabilities
- [ ] Expose only what is require to public networks, consider Gateway services to secure insecure backend services
- [ ] User Anti Virus and Anti malware

## Development Process
- [ ] When including packages/libraries make sure they are actively maintained and security tested
- [ ] Code review, no self-approval
- [ ] Another Code review; external and community
- [ ] Deploy mutliple times and test positve and negtive scenarios
- [ ] Automate continuous  testing


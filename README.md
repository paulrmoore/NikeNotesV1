# NikeNotesV1
Poking around the Nike website 

## Email Verification

### 3 API Calls For Username Login page

1. (Name = some value not understood ) [accounts.nike.com]/(some random value)
* Rest method = POST 
* redirect_uri=https://www.nike.com/auth/login
2. (Name = v1) [accounts.nike.com]/credential_lookup/v1
  * Rest method = POST
  * Content-type: application/json
  * Request data: {"credential":"[email]","client_id":"[client_id]"}
  * Response data: {
    "challenge": "password",
    "legacySwooshChallenge": "none",
    "swooshEntitlement": false,
    "legacySwooshEntitlement": false
}
    Tells UI that the next step in the flow is for the password to be accepted 
3. (Name = v1) [accounts.nike.com]/events/v1
  * Rest method = Post
  * Content-type: application/json
  * Request data:
     1. {"name":"Authentication View Rendered","clientId":"[client_data]","country":"US","language":"en","path":"lookup","view":"lookup","timestamp":,"anonymousId":"","emperorId":"","browserName":"Safari","browserVersion":"17.6","deviceModel":"Macintosh","deviceType":"desktop","deviceVendor":"Apple","osName":"Mac OS","osVersion":"10.15.7","userType":"member","regSiteId":""}
     2. {"name":"Authentication Started","clientId":"","country":"US","language":"en","path":"lookup","view":"lookup","timestamp":1726004539004,"anonymousId":"","emperorId":"","browserName":"Safari","browserVersion":"17.6","deviceModel":"Macintosh","deviceType":"desktop","deviceVendor":"Apple","osName":"Mac OS","osVersion":"10.15.7","userType":"member","regSiteId":""}
     3. {"name":"Authentication View Rendered","clientId":"","country":"US","language":"en","path":"challenge","view":"challenge","timestamp","anonymousId":"","emperorId":"","browserName":"Safari","browserVersion":"17.6","deviceModel":"Macintosh","deviceType":"desktop","deviceVendor":"Apple","osName":"Mac OS","osVersion":"10.15.7","userType":"member","regSiteId":""}

4. (Name = v1) [accounts.nike.com]/challenge/password/v1
   * Rest method = Post
   * Content-type: application/json
   * Request data:   {"credential":"[email]","password":"[password]","client_id":"","swoosh_login":false}
   * Response data:  {"deactivated": false, "deviceVerificationRequired": true}
5. (Name = v1) [accounts.nike.com]/verification_code/send/v1
   * Rest method = Post
   * Content-type: application/json
   * Referer: [https://accounts.nike.com/verify-unrecognized-device?client_id=4fd2d5e7db76e0f85a6bb56721bd51df&redirect_uri=https://www.nike.com/auth/login&response_type=code&scope=openid%20nike.digital%20profile%20email%20phone%20flow%20country&state=0935cf7ea79b4d7eb921933e0978f4db&code_challenge=86U-i87EJpak4bCPzfqr8ATYmOGYyBs7BmGg9d2tFgg&code_challenge_method=S256]
   * Request data: {"destination":"[email]","country":"US","type":"CHALLENGE","client_id":"","language":"en","swoosh_login":false}
   * Response data: 204 (no data)
6. (Name = v1) [accounts.nike.com]/events/v1
   * Request data:
   * {"name":"Http Request Error","clientId":"4fd2d5e7db76e0f85a6bb56721bd51df","country":"US","language":"en","path":"challenge","view":"challenge","timestamp":1726017209084,"anonymousId":"4C6FA3D0236B14DCDF2417F0912C16D7","emperorId":"bbe7bdcd-1cc7-4eb3-8bc8-28e59165d7e8","browserName":"Safari","browserVersion":"17.6","deviceModel":"Macintosh","deviceType":"desktop","deviceVendor":"Apple","osName":"Mac OS","osVersion":"10.15.7","userType":"member","regSiteId":"","status":401,"message":"Invalid Credentials","url":"/challenge/password/v1"}
   * {"name":"Authentication View Rendered","clientId":"4fd2d5e7db76e0f85a6bb56721bd51df","country":"US","language":"en","path":"verify-unrecognized-device","view":"verify_unrecognized_device","timestamp":1726017227889,"anonymousId":"4C6FA3D0236B14DCDF2417F0912C16D7","emperorId":"bbe7bdcd-1cc7-4eb3-8bc8-28e59165d7e8","browserName":"Safari","browserVersion":"17.6","deviceModel":"Macintosh","deviceType":"desktop","deviceVendor":"Apple","osName":"Mac OS","osVersion":"10.15.7","userType":"member","regSiteId":""}
   * {"name":"Send Code","clientId":"4fd2d5e7db76e0f85a6bb56721bd51df","country":"US","language":"en","path":"verify-unrecognized-device","view":"verify_unrecognized_device","timestamp":1726017229657,"anonymousId":"4C6FA3D0236B14DCDF2417F0912C16D7","emperorId":"bbe7bdcd-1cc7-4eb3-8bc8-28e59165d7e8","browserName":"Safari","browserVersion":"17.6","deviceModel":"Macintosh","deviceType":"desktop","deviceVendor":"Apple","osName":"Mac OS","osVersion":"10.15.7","userType":"member","regSiteId":"","codeType":"CHALLENGE","destinationType":"EMAIL"}
   * Response data: 204 (no data)
7. (Name = v1) [accounts.nike.com]/challenge/code/v1
   * Request method:  POST
   * Request data: {"credential":"[email]","verification_code":"","client_id":"","verification_code_type":"CHALLENGE"}
   * Response data: {
    "error_id": "924962b3-c5bd-4c18-8bde-85d673cae40e",
    "errors": [{"code": "invalid_verification_code","message": "Invalid Request"}]}

### After Login

1. Token exchange using Bearer token (Name = token_exchange) [api.nike.com]/cssfproxy
   * validates Bearer token
   * Response is identityToken 

2. (Name = events) [insights-collector.newrelic.com]/v1/accounts
   * Request data: {"eventType":"AdtechMarketingClientEvent","event":"marketingClientEvent","eventName":"onLandingPageViewed","country":"us","eventId":"1f2b0654-b865-4d51-85de-9dfd42941789","isEVO":"N","privacyType":"nike-privacy-core","scriptSource":"","filter":"","windowHREF":"https://www.nike.com/","referer":"https://www.nike.com/auth/login","version":"","view":"homepage"}
   * Response data: {
    "success": true,
    "uuid": "ca735001-0001-b533-39d5-0192019e417b"
}

3. (Name = address) [api.nike.com]/identity/user/v1/(value)/address
   * Request Method: GET
   * uses Bearer token
   * Response data: {"id":"","code":"(zip)","country":"US","line1":"","locality":"","name":{"primary":{"family":"","given":""}},"phone":{"primary":""},"preferred":true,"province":""}
       * contains personal data 

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

# GET STARTED
## Authentication & Authorization
The Reloadly APIs are HTTP-based RESTful APIs that use OAuth 2.0 protocol for authorization. API request and response bodies are formatted in JSON.

OAuth is an open standard that many companies use to provide secure access to protected resources.

When you create a developer account. Reloadly generates a set of OAuth client ID and secret credentials for your app for both sandbox and live environments. You pass these credentials in the `Authorization` header in the get access token request.

In exchange for these credentials, the Reloadly authorization server issues access tokens called bearer tokens that you use for authorization when you make REST API requests. A bearer token enables you to complete actions on behalf of, and with the approval of the resource owner.

The `access_token` field in the get access token response contains a bearer token, indicated by the `token_type` of `Bearer` :
<aside class="success">
Response 200 (application/json)
</aside>
<pre style="float:none;margin-bottom: 1.5em;">
{
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa",
      "scope": "<scopes>",
      "expires_in": "5184000",
      "token_type": "Bearer"
}
</pre>
Include this bearer token in API requests in the `Authorization` header with the `Bearer` authentication scheme.

This sample request uses a bearer token to list topup transactions for an user account :
<pre style="float:none;margin-bottom: 1.5em;">
curl -v -X GET https://topups-sandbox.reloadly.com/transactions?page=1&size=3 \
    -H "Accept: application/com.reloadly.topups-v1+json" \
    -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa" \
</pre>

Access tokens have a finite lifetime. The `expires_in` field in the get access token response indicates the lifetime, in seconds, of the access token. For example, an expiry value of 3600 indicates that the access token expires in one hour from the time the response was generated.

To detect when an access token expires, write code to either :

* Keep track of the `expires_in` value in the token response. The value is expressed in seconds.
* Handle the HTTP `401 Unauthorized` status code and the `TOKEN_EXPIRED` error code in the error response message. The API endpoint issues this status code when it detects an expired token.

Before you create another token, re-use the access token until it expires. See the rate limiting guidelines.

## API Requests

To construct a REST API request, combine these components :

Component | Description
---------- | -------
The HTTP method | <ul><li> `GET`. Requests data from a resource.</li><li>`POST`. Submits data to a resource to process.</li><li>`PUT`. Updates a resource</li><li>`PATCH`. Partially updates a resource.</li><li>`DELETE`. Deletes a resource.</li></ul>
The URL to the API service | <ul><li>Sandbox. `https://topups-sandbox.reloadly.com`</li><li>Live. `https://topups.reloadly.com`</li></ul>
The URI to the resource | The resource to query, submit data to, update, or delete. For example, `/reports/transactions`.
Query parameters | Optional. Controls which data appears in the response. Use to filter, limit the size of, and sort the data in an API response.
HTTP request headers | Includes the `Authorization` header with the access token.
A JSON request body | Required for most `GET`, `POST`, `PUT`, and `PATCH` calls.
### Query Parameters
For most REST `GET` calls, you can specify one or more optional query parameters on the request URI to filter, limit the size of, and sort the data in an API response. For filter parameters, see the individual `GET` calls.

To limit, or page, and sort the data that is returned in some API responses, use these, or similar, query parameters :

<blockquote style="float:none;margin-bottom:1.5rem;">
<p>**Note** : Not all pagination parameters are available for all APIs.</p>
</blockquote>

Parameter | Type | Description
---------- | ------- | -------
`count` | integer | The number of items to list in the response.
`startTime` | integer | The start date and time for the range to show in the response, in Internet date and time format. (ISO 8601) For example, `startTime=2018-03-06 00:00:00`
`endTime` | integer | The end date and time for the range to show in the response, in Internet date and time format. (ISO 8601) For example, `endTime=2016-03-06 23:59:59`.
`page` | integer | The zero-relative start index of the entire list of items that are returned in the response. So, the combination of `page=0` and `page_size=20` returns the first 20 items. The combination of `page=20` and `page_size=20` returns the next 20 items.
`size` | integer | The number of items to return in the response.
`sortBy` | string | Sorts the transactions in the response by a specified value, such as the create time or update time.
`sortOrder` | string | Sorts the items in the response in ascending or descending order.

For example, the Invoicing API returns details for four invoices beginning with the third invoice and includes the total count of invoices in the response:

<pre style="float:none;margin-bottom: 1.5em;">
curl -v -X GET https://sandbox.topups.reloadly.com/transactions?page=1&size=10&startTime=2018-03-01 00:00:00&endTime=2018-03-31 23:59:59 \
    -H "Accept: application/com.reloadly.topups-v1+json" \
    -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa" \
</pre>

### Request Headers
The commonly used HTTP request headers are :
 
Header | Description 
---------- | -------
**Accept** | Required for operations with a response body. Specifies the response format. The syntax is: <br><br>Accept: `application/format` <br><br>Where `format` is `com.reloadly.topups-v1+json` .
**Authorization** | Required to get an access token or make API calls.<br><br> To get an access token, set this header to your `client_id` and `client_secret` credentials.<br><br>**Note**: If you use cURL, specify `-u "client_id:secret"`.<br><br>To make REST API calls, include the bearer token in the `Authorization` header with the `Bearer` authentication scheme:<br><br>Authorization: `Bearer Access-Token`
**Content-Type** | Required for operations with a request body.<br><br>Specifies the request format. The syntax is:<br><br>Content-Type: `application/format`<br><br>Where `format` is `json`.
## API Responses
Reloadly API calls return HTTP status codes. Some API calls also return JSON response bodies that include information about the resource including one or more contextual HATEOAS links. Use these links to request more information about and construct an API flow that is relative to a specific request.
### HTTP status codes
Each REST API request returns a **success** or **error** status code.
####Success
Status code | Description
---------- | -------
`200 OK` | The request succeeded.
`201 Created` | A `POST` method successfully created a resource. If the resource was already created by a previous execution of the same method, for example, the server returns the HTTP `200 OK` status code.
`202 Accepted` | The server accepted the request and will execute it later.
`204 No Content` | The server successfully executed the method but returns no response body.

####Error
In the responses for failed requests, Reloadly returns HTTP `4XX` or `5XX` status codes.

For all errors except Identity errors, Reloadly returns an error response body that includes additional error details in this format:

<pre style="float:none;margin-bottom: 1.5em;">
{
  "timeStamp": ERROR_TIME_STAMP (numerical),
  "message": "Account verification failed, invalid or expired token",
  "infoLink": "ERROR_DOCUMENTATION_LINK",
  "path": "/accounts/verify",
  "errorCode": "ERROR_CODE",
  
  // Some types of errors also include a details array:
  "details": [
    {
        "field": "FIELD_NAME",
        "issue": "PROBLEM_WITH_FIELD"
    }
  ]
}
</pre>
In the responses, Reloadly returns these HTTP status codes for failed requests:

HTTP status code | Typical error code and error message | Cause
---------- | ------- | -------
`400 Bad Request` | `INVALID_REQUEST`. Request is not well-formed, syntactically incorrect, or violates schema. | See Validation errors. The server could not understand the request. Indicates one of these conditions: <ul><li>The API cannot convert the payload data to the underlying data type.</li><li>he data is not in the expected data format.</li><li>A required field is not available.</li><li>A simple data validation error occurred.</li></ul>
`401 Unauthorized` | `AUTHENTICATION_FAILURE`. Authentication failed due to invalid authentication credentials. | See Authentication errors. The request requires authentication and the caller did not provide valid credentials.
`403 Forbidden` | `NOT_AUTHORIZED`. Authorization failed due to insufficient permissions. | The client is not authorized to access this resource although it might have valid credentials. For example, the client does not have the correct OAuth 2 scope. Additionally, a business-level authorization error might have occurred. For example, the account holder does not have sufficient funds.
`404 Not Found` | `RESOURCE_NOT_FOUND`. The specified resource does not exist. | The server did not find anything that matches the request URI. Either the URI is incorrect or the resource is not available. For example, no data exists in the database at that key.
`405 Method Not Allowed` | `METHOD_NOT_SUPPORTED`. The server does not implement the requested HTTP method. | The service does not support the requested HTTP method. For example, `PATCH`.
`406 Not Acceptable` | `MEDIA_TYPE_NOT_ACCEPTABLE`. The server does not implement the media type that would be acceptable to the client. | The server cannot use the client-request media type to return the response payload. For example, this error occurs if the client sends an `Accept: application/xml` request header but the API can generate only an `application/json` response.
`415 Unsupported Media Type` | `UNSUPPORTED_MEDIA_TYPE`. The server does not support the request payload’s media type. | The API cannot process the media type of the request payload. For example, this error occurs if the client sends a `Content-Type: application/xml` request header but the API can only accept `application/json` request payloads.
`422 Unprocessable Entity` | `UNPROCCESSABLE_ENTITY`. The API cannot complete the requested action, or the request action is semantically incorrect or fails business validation. | The API cannot complete the requested action and might require interaction with APIs or processes outside of the current request. No systemic problems limit the API from completing the request. For example, this error occurs for any business validation errors, including errors that are not usually of the `400` type.
`429 Unprocessable Entity` | `RATE_LIMIT_REACHED`. Too many requests. Blocked due to rate limiting. | The rate limit for the user, application, or token exceeds a predefined value. See RFC 6585.
`500 Internal Server Error` | `INTERNAL_SERVER_ERROR`. An internal server error has occurred. | A system or application error occurred. Although the client appears to provide a correct request, something unexpected occurred on the server.
`503 Service Unavailable` | `SERVICE_UNAVAILABLE`. Service Unavailable. | The server cannot handle the request for a service due to temporary maintenance.

####Validation errors
For validation errors, Reloadly returns the HTTP `400 Bad Request` status code.

To prevent validation errors, ensure that parameters are of the right type and conform to these constraints:

Parameter type | Description
---------- | ------- 
Character | Names, addresses, phone numbers, and so on have maximum character limits.
Numeric | Amounts, ids, and so on must use non-negative numeric values and have required formats. For example, a CVV must be three or four numbers while a credit card number must contain only numbers.
Required | Must be included in the request. For example, when you provide credit card information, you must include a postal code for most countries.
Monetary | Must use the right currency.

####Authentication errors
For authentication errors, Reloadly returns the HTTP `401 Unauthorized` status code. See authentication and authorization.

Access token-related issues often cause authentication errors.

Ensure that the access token is valid and present and not expired.
### HATEOAS links
Hypermedia as the Engine of Application State (HATEOAS) is a constraint of the REST application architecture that distinguishes it from other network application architectures.

This excerpt from a sample response shows an array of HATEOAS links:
<pre style="float:none;margin-bottom: 1.5em;">
{  
  "links": [{
    "href": "https://api.reloadly.com/payments/4",
    "rel": "self",
    "method": "GET"
  }, {
    "href": "https://api.reloadly.com/payments/4/refund",
    "rel": "refund",
    "method": "POST"
  }]
}
</pre>
You can use the links in this example, as follow :
* Use the `self` link to get more information about the request. Combine the method and the target URL to make the call :
`GET https://api.reloadly.com/payments/4`
*Use the `refund` link to request a refund :
`POST https://api.reloadly.com/payments/4/refund`

The elements in the `link` object are :

Element | Required | Description
---------- | ------- | -------
 `href` | Required | The target URL.
 `rel` | Required | The link relationship type.
 `method` | Optional | The HTTP method. Default is `GET`.
 
 ####The href element
 The `href` element contains the complete target URL, or link, to use in combination with the HTTP `method` to make the related call. `href` is the key HATEOAS component that links a completed call with a subsequent call. 

####The rel element
The `rel` element contains the link relationship type, or how the href link relates to the previous call.

For a complete list of the link relationship types, see Link Relationship Types.
####The method element
Optional. The `method` element contains an HTTP method. If present, use this method to make a request to the target URL. If absent, the default method is `GET`.

The HTTP methods are :

Method |  Description
---------- | ------- 
`DELETE` | Deletes a resource.
`GET` | Shows details for a resource or lists resources.
`PATCH` | Partially updates a resource.
`POST` | Creates or manages a resource.
`PUT` | Updates a resource.

## Error Codes
A Reloadly API operation may return multiple error and warning codes :

Code | Description
---------- | -------
TOKEN_EXPIRED | Access tokens have a finite lifetime. The expires_in field in the get access token response indicates the lifetime, in seconds, of the access token. For example, an expiry value of 3600 indicates that the access token expires in one hour from the time the response was generated. <br><br>To detect when an access token expires, write code to either :<br><br><ul><li>Keep track of the `expires_in` value in the token response. The value is expressed in seconds.</li><li>Handle the HTTP `401 Unauthorized` status code and the `TOKEN_EXPIRED` error code in the error response message. The API endpoint issues this status code when it detects an expired token.</li></ul>
COUNTRY_NOT_SUPPORTED | The specified country (ISO-2 country code) in the request is currently not supported
INVALID_RECIPIENT_PHONE | The specified `recipientPhone` in the request is not valid for the specified country (ISO-2 country code) country
INVALID_SENDER_PHONE | The specified `senderPhone` in the request is not valid for the specified country (ISO-2 country code) country
INVALID_PHONE_NUMBER | The specified phone number is not valid
PHONE_RECENTLY_RECHARGED | After a given phone number has been topuped-up, a delay of 2 minutes has to take place before the same phone number can be topuped-up again.
COUNTRY_NOT_SUPPORTED | The specified country in the request is currently disabled or not supported
OPERATOR_UNAVAILABLE_OR_CURRENTLY_INACTIVE | The specified operator is currently disabled or inactive on the platform.
INACTIVE_ACCOUNT | Reloadly reserves the right to de-activate a developer account for various reasons. The developer must contact support for more info.
INVALID_AMOUNT_FOR_OPERATOR | The specified topup amounts is not valid for the given operator in the request.
TOPUP_TRANSACTION_FAILED | Topup transactions may failed various reasons. Developer must contact support if they need more details.
COULD_NOT_AUTO_DETECT_OPERATOR | Reloadly platform has the ability to auto-detect mobile carriers/operators for a given phone number, see Operator auto-detect. In cases where that's not possible, this error code is returned.
ACCOUNT_NOT_FOUND | User account not found or does not exist.
MAX_DAILY_TRANSACTION_COUNT_REACHED | Some developer account may have a daily transaction count limit. This error code is returned when that occurs
MAX_DAILY_TRANSACTION_AMOUNT_REACHED | Some developer account may have a daily total transaction amount limit. This error code is returned when that occurs
TRANSACTION_CANNOT_BE_PROCESSED_AT_THE_MOMENT | Topup transactions may failed various reasons. Developer must contact support if they need more details.
INVALID_AMOUNT | Reloadly expects amounts to be numerical.
INSUFFICIENT_BALANCE | This error code is returned when a user attempts an API operation that requires adequate account balance to be available in order to be performed. This error code is returned when the user account lack enought balance. 
OPERATOR_NOT_IN_SERVICE | The specified operator in the request is currently disabled, inactive or not in service on the platform.

## Make Your First Call
To make REST API calls, create a Reloadly developer account and get an access token :

1. **[Create a developer account](https://www.reloadly.com/login)** <= (click to go there). When you create a developer account, Reloadly generates a set of OAuth credentials.
2. **Get an access token**. Pass the OAuth credentials in a get access token call.In response, the Reloadly authorization server issues an access token.
3. **Make REST API calls**. Use the access token for authentication when you make REST API calls.

###Get an access token
<blockquote style="float:none;margin-bottom:1.5rem;">
<p> URLs:<br> <ul><li>https://auth.reloadly.com/oauth/token</li></ul> </p>
</blockquote>
To get an access token, you pass your OAuth credentials in a get access token call. To make this call, you can use either cURL on the command line or the Postman app.

In response, the Reloadly authorization server issues an access token.

Re-use the access token until it expires. See our rate limiting guidelines. When it expires, you can get a new token.
###cURL example
<blockquote style="float:none;margin-bottom:1.5rem;">
<p>Tips:<br>
<ul>
<li> If you use Windows, use a Bash shell to make cURL calls.</li>
<li>If you use a command-line tool other than cURL, set content-type to application/json.</li>
</ul>
</p>
</blockquote>

1. Download [cURL](https://curl.haxx.se/download.html) for your environment.
2. From the command line, run this command :

<pre style="float:none;margin-bottom: 1.5em;">
curl -d '{
            "client_id":"YOUR_CLIENT_ID",
            "client_secret":"YOUR_CLIENT_SECRET",
            "grant_type":"client_credentials",
            "audience":"https://topups.reloadly.com"
        }' \
    -H "Content-Type: application/json" \
    -X POST https://auth.reloadly.com/oauth/token \
</pre>

Where :
 
&nbsp; | &nbsp;
---------- | -------
The get access token endpoint. | `https://auth.reloadly.com/oauth/token`
`client_id` | Your client ID.
`client_secret` | Your client secret.
`audience` | The API you're requesting the access token for.<br>Live : `https://topups.reloadly.com`<br>Sandbox : `https://topups-sandbox.reloadly.com`
`grant_type` | The grant type. Set to `client_credentials`.

3. View the sample response.

###Postman example

1. Download the latest version of [Postman](https://www.getpostman.com/) for your environment, and open Postman.
2. Select the `POST` method.
3. Enter the `https://auth.reloadly.com/oauth/token` request URL.
4. On the **Header** tab, enter **Content-Type** under key and **application/json** as value :

&nbsp; | &nbsp;
---------- | -------
**key** | Content-Type
**value** | application/json

5. On the **Body** tab, select **raw** and enter this information :

<pre style="float:none;margin-bottom: 1.5em;">
{

   "client_id": "YOUR_CLIENT_ID",
   
   "client_secret": "YOUR_CLIENT_SECRET",
   
   "grant_type": "client_credentials",
   
   "audience": "https://topups-sandbox.reloadly.com"
  }
</pre>

6. Click Send
7. View the sample response :

<pre style="float:none;margin-bottom: 1.5em;">
{

  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSU",

  "scope": "send-topups read-topups-history read-operators read-promotions",

  "expires_in": 86400,

  "token_type": "Bearer"
 }
</pre>

Where : 

&nbsp; | &nbsp;
---------- | -------
`access_token` | Your access token.
`expires_in` | The number of seconds after which the token expires. Request another token when the current one expires.

###Make REST API Calls
With a valid access token, you can make REST API calls.

This sample call sends a airtime (topup) transaction and uses only the required input parameters. The access token in the call is an OAuth bearer token.

**Note**: Topups API calls are always made by an `actor`. The actor specifies a bearer token in the Authorization: Bearer request header. A bearer token is an access token that is issued to the actor by an authorization server with the approval of the resource owner. In this case, the actor uses the bearer token to make a topup request.

<pre style="float:none;margin-bottom: 1.5em;">
curl -d '{
          "recipientPhone": {
            "countryCode": "HT",
            "number": "50936377111" //(Note the "+509" country dialing code for Haiti)
          },
          "senderPhone": {
            "countryCode": "US",
            "number": "13059547862" //(Note the "+1" country dialing code for USA)
          },
          "operatorId": 173,
          "amount": 15,
          "customIdentifier": "transaction by john@example.com"
        }' \
    -H "Accept: application/com.reloadly.topups-v1+json" \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSU" \
    -X POST https://topups.reloadly.com/topups
</pre>

**Note**: Phone numbers (`recipientPhone`, `senderPhone`) can be in 2 formats. In the example above, the "+" sign is not present in front of the country dialing codes (see countries dialing codes). Our API also accept phone number prefixed with "+". The following is equivalent to the example above :

<pre style="float:none;margin-bottom: 1.5em;">
curl -d '{
          "recipientPhone": {
            "countryCode": "HT",
            "number": "+50936377111" //(Note the "+509" country dialing code for Haiti)
          },
          "senderPhone": {
            "countryCode": "US",
            "number": "+13059547862" //(Note the "+1" country dialing code for USA)
          },
          "operatorId": 173,
          "amount": 15,
          "customIdentifier": "transaction by john@example.com"
        }' \
    -H "Accept: application/com.reloadly.topups-v1+json" \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSU" \
    -X POST https://topups.reloadly.com/topups
</pre> 
# Account Balance
## Retrieve account balance

```shell
curl --include \
     --header "Accept: application/com.reloadly.topups-v1+json" \
     --header "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa" \
  'https://topups.reloadly.com/accounts/balance
```
```java
// Maven : Add these dependecies to your pom.xml (java6+)
// <dependency>
//     <groupId>org.glassfish.jersey.core</groupId>
//     <artifactId>jersey-client</artifactId>
//     <version>2.8</version>
// </dependency>
// <dependency>
//     <groupId>org.glassfish.jersey.media</groupId>
//     <artifactId>jersey-media-json-jackson</artifactId>
//     <version>2.8</version>
// </dependency>

import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;
import javax.ws.rs.client.Entity;
import javax.ws.rs.core.Response;
import javax.ws.rs.core.MediaType;

Client client = ClientBuilder.newClient();
Response response = client.target("https://topups.reloadly.com/accounts/balance
")
  .request(MediaType.TEXT_PLAIN_TYPE)
  .header("Accept", "application/com.reloadly.topups-v1+json")
  .header("Authorization", "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa")
  .get();

System.out.println("status: " + response.getStatus());
System.out.println("headers: " + response.getHeaders());
System.out.println("body:" + response.readEntity(String.class));
```
```javascript
var request = new XMLHttpRequest();

request.open('GET', 'https://topups.reloadly.com/accounts/balance
');

request.setRequestHeader('Accept', 'application/com.reloadly.topups-v1+json');
request.setRequestHeader('Authorization', 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa');

request.onreadystatechange = function () {
  if (this.readyState === 4) {
    console.log('Status:', this.status);
    console.log('Headers:', this.getAllResponseHeaders());
    console.log('Body:', this.responseText);
  }
};

request.send();
```
```perl
$ENV{'PERL_LWP_SSL_VERIFY_HOSTNAME'} = 0;
use LWP::UserAgent;
use strict;
use warnings;
use 5.010;
use Cpanel::JSON::XS qw(encode_json decode_json);

my $ua   = LWP::UserAgent->new;

$ua->default_header("Accept" => "application/com.reloadly.topups-v1+json");
$ua->default_header("Authorization" => "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa");

my $response = $ua->get("https://topups.reloadly.com/accounts/balance
");

print $response->as_string;
```
```python
from urllib2 import Request, urlopen

headers = {
  'Accept': 'application/com.reloadly.topups-v1+json',
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa'
}
request = Request('https://topups.reloadly.com/accounts/balance
', headers=headers)

response_body = urlopen(request).read()
print response_body
```
```php
<?php
$ch = curl_init();

curl_setopt($ch, CURLOPT_URL, "https://topups.reloadly.com/accounts/balance
");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HEADER, FALSE);

curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  "Accept: application/com.reloadly.topups-v1+json",
  "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa"
));

$response = curl_exec($ch);
curl_close($ch);

var_dump($response);
```
```ruby
require 'rubygems' if RUBY_VERSION < '1.9'
require 'rest_client'

headers = {
  :accept => 'application/com.reloadly.topups-v1+json',
  :authorization => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa'
}

response = RestClient.get 'https://topups.reloadly.com/accounts/balance
', headers
puts response
```
```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func main() {
	client := &http.Client{}

	req, _ := http.NewRequest("GET", "https://topups.reloadly.com/accounts/balance
", nil)

	req.Header.Add("Accept", "application/com.reloadly.topups-v1+json")
	req.Header.Add("Authorization", "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa")

	resp, err := client.Do(req)

	if err != nil {
		fmt.Println("Errored when sending request to the server")
		return
	}

	defer resp.Body.Close()
	resp_body, _ := ioutil.ReadAll(resp.Body)

	fmt.Println(resp.Status)
	fmt.Println(string(resp_body))
}
```
```csharp
//Common testing requirement. If you are consuming an API in a sandbox/test region, uncomment this line of code ONLY for non production uses.
//System.Net.ServicePointManager.ServerCertificateValidationCallback = delegate { return true; };

//Be sure to run "Install-Package Microsoft.Net.Http" from your nuget command line.
using System;
using System.Net.Http;

var baseAddress = new Uri("https://topups.reloadly.com/");

using (var httpClient = new HttpClient{ BaseAddress = baseAddress })
{

  httpClient.DefaultRequestHeaders.TryAddWithoutValidation("accept", "application/com.reloadly.topups-v1+json");
  
  httpClient.DefaultRequestHeaders.TryAddWithoutValidation("authorization", "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa");
  
  using(var response = await httpClient.GetAsync("accounts/balance"))
  {
 
        string responseData = await response.Content.ReadAsStringAsync();
  }
}
```
```vb
Dim request = TryCast(System.Net.WebRequest.Create("https://topups.reloadly.com/accounts/balance
"), System.Net.HttpWebRequest)

request.Method = "GET"

request.Accept = "application/com.reloadly.topups-v1+json"

request.Headers.Add("authorization", "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa")

request.ContentLength = 0
Dim responseContent As String
Using response = TryCast(request.GetResponse(), System.Net.HttpWebResponse)
  Using reader = New System.IO.StreamReader(response.GetResponseStream())
    responseContent = reader.ReadToEnd()
  End Using
End Using
```
```groovy
import groovyx.net.http.RESTClient
import static groovyx.net.http.ContentType.JSON
import groovy.json.JsonSlurper
import groovy.json.JsonOutput

@Grab (group = 'org.codehaus.groovy.modules.http-builder', module = 'http-builder', version = '0.5.0')
def client = new RESTClient("https://topups.reloadly.com")

def emptyHeaders = [:]
emptyHeaders."Accept" = "application/com.reloadly.topups-v1+json"
emptyHeaders."Authorization" = "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa"

response = client.get( path : "/accounts/balance", headers: emptyHeaders )

println("Status:" + response.status)

if (response.data) {
  println("Content Type: " + response.contentType)
  println("Body:\n" + JsonOutput.prettyPrint(JsonOutput.toJson(response.data)))
}
```
```objective_c
NSURL *URL = [NSURL URLWithString:@"https://topups.reloadly.com/accounts/balance
"];

NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:URL];
[request setHTTPMethod:@"GET"];

[request setValue:@"application/com.reloadly.topups-v1+json" forHTTPHeaderField:@"Accept"];
[request setValue:@"Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa" forHTTPHeaderField:@"Authorization"];

NSURLSession *session = [NSURLSession sharedSession];
NSURLSessionDataTask *task = [session dataTaskWithRequest:request
                                        completionHandler:
                              ^(NSData *data, NSURLResponse *response, NSError *error) {

                                  if (error) {
                                      // Handle error...
                                      return;
                                  }

                                  if ([response isKindOfClass:[NSHTTPURLResponse class]]) {
                                      NSLog(@"Response HTTP Status code: %ld\n", (long)[(NSHTTPURLResponse *)response statusCode]);
                                      NSLog(@"Response HTTP Headers:\n%@\n", [(NSHTTPURLResponse *)response allHeaderFields]);
                                  }

                                  NSString* body = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
                                  NSLog(@"Response Body:\n%@\n", body);
                              }];
[task resume];
```
```swift
import Foundation

// NOTE: Uncommment following two lines for use in a Playground
// import PlaygroundSupport
// PlaygroundPage.current.needsIndefiniteExecution = true

let url = URL(string: "https://topups.reloadly.com/accounts/balance
")!
var request = URLRequest(url: url)
request.addValue("application/com.reloadly.topups-v1+json", forHTTPHeaderField: "Accept")
request.addValue("Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik0wWXpRa", forHTTPHeaderField: "Authorization")

let task = URLSession.shared.dataTask(with: request) { data, response, error in
  if let response = response {
    print(response)

    if let data = data, let body = String(data: data, encoding: .utf8) {
      print(body)
    }
  } else {
    print(error ?? "Unknown error")
  }
}

task.resume()
```
###Response
200
###Headers
**Content-Type**:application/json
###Body

<pre style="float:none;margin-bottom: 1.5em;">
{
  "balance": 550.75,
  "currencyCode": "USD"
}
</pre>


# Commissions Collection
## List all commissions
## Retrieve account operator commission by operator id

#Country Collection
##List All Supported Countries
##Retrieve a country by country ISO code(ISO Alpha-2)

#Operators Collection 
##List all operators
##Retrieve a operator by id
##Auto-detect mobile operator for given phone number
##Retrieve FX rate for a given operator id
##List available operators by (ISO Alpha-2) country code 

#Promotions Collection
##List all topup operators promotions
##Retrieve topup operator promotion by id
##Retrieve topup operator promotion by operator id

#Topup a phone 
##Send Topup

#Transaction History
##Retrieve topup transactions
##Retrieve topup transaction by transaction id
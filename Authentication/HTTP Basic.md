# HTTP Basic Authentication

HTTP Basic authentication is the oldest and simplest way to start working with the Harvest API. Requests include the username and password of the user authenticating. HTTPS is required for accessing the API.

## Making a request

To interact with Harvest in this example we're going to use XML. To make requests in XML, specify application/xml for your Content-Type and Accept headers. A simple example with [curl](http://en.wikipedia.org/wiki/CURL):

    curl -H 'Content-Type: application/xml' -H 'Accept: application/xml' -u "user@email.com:password" https://subdomain.harvestapp.com/account/who_am_i

Successful requests return HTTP response codes in the 2xx range (e.g. 200, 201, etc.). Other response codes indicate a failed request or missing resource, in which case an error message may be provided. When successful, the above request returns:

    <?xml version="1.0" encoding="UTF-8"?>
    <hash>
      <company>
        <base-uri>https://subdomain.harvestapp.com</base-uri>
      </company>
      <user>
        <timestamp-timers type="boolean">false</timestamp-timers>
        <email>name@example.com</email>
        <timezone>Eastern Time (US &amp; Canada)</timezone>
        <id type="integer">222222</id>
        <admin type="boolean">true</admin>
      </user>
    </hash>

*Chrome Users*: Check out the [Postman REST Client](https://chrome.google.com/webstore/detail/postman-rest-client/fdmmgilgnpjigdojojpjoooidkmcomcm?hl=en) to test requests.

*Firefox Users*: We recommend using the [RestClient plugin](https://addons.mozilla.org/en-US/firefox/addon/restclient/) to help you make requests to and see responses from the Harvest API. You need to set up your request headers with the following:

Accept: application/xml
Content-Type: application/xml
Authorization: Basic (insert your authentication string here)
Your authentication string is a base64 encoded version of your credentials. You can generate this in Ruby:

    Base64.encode64("my@email.com:password").delete("\r\n")
# Making Requests

## Content Negotiation

Clients using the api should specify that they accept responses using the
`application/vnd.api+json` format, for convenience we will also accept `application/json`
since it is the default for many popular client libraries.

The Server will respond with a `Content-Type` header that mirrors the format
requested by the Client.

{% method %}
## Shards {#shards}
{% common %}
To specify the regional shard, use this code:

{% sample lang="shell" %}
```shell
"...gamelockerapp.com/shards/<region>/..."
```

{% sample lang="java" %}
```java
"...gamelockerapp.com/shards/<region>/..."
```

{% sample lang="python" %}
```python
"...gamelockerapp.com/shards/<region>/..."
```

{% sample lang="ruby" %}
```ruby
"...gamelockerapp.com/shards/<region>/..."
```

{% sample lang="js" %}
```javascript
"...gamelockerapp.com/shards/<region>/..."
```

{% sample lang="go" %}
```go
"...gamelockerapp.com/shards/<region>/..."
```

{% endmethod %}

The Vainglory Game Data Service currently supports the following regions:

***General Region Shards***

To find data regarding live servers, where all data is found, please use the following shards.

* **China:** ```cn```
* **North America:** ```na```
* **Europe:** ```eu```
* **South America:** ```sa```
* **East Asia:** ```ea```
* **Southeast Asia (SEA):** ```sg```

***Tournament Region Shards***

To find data regarding professional eSport, which take place on the private client only, please use the following shards.

* **North America Tournaments:** ```tournament-na```
* **Europe Tournaments:** ```tournament-eu```
* **South America Tournaments:** ```tournament-sa```
* **East Asia Tournaments:** ```tournament-ea```
* **Southeast Asia Tournaments:** ```tournament-sg```

***Public Beta Environment***

For developers that have signed an agreement with SEMC to access the Public Beta Eniroment (PBE) your key has access to an
additional shard

* **Public Beta Environment (Coming Soon):** ```pbe```

**Choosing a specific region is required**
{% method %}
## GZIP

> To specify the header Accept-Encoding, use this code:
{% sample lang="shell" %}
```shell
-H "Accept-Encoding: gzip"
```
{% sample lang="java" %}
```java
conn.setRequestProperty("Accept-Encoding","gzip");
```
{% sample lang="python" %}
```python
header = {"Accept-Encoding":"gzip"}
```
{% sample lang="ruby" %}
```ruby
```
{% sample lang="js" %}
```javascript
```
{% sample lang="go" %}
```go
req.Header.Set("Accept-Encoding", "gzip")
```
{% endmethod %}
Clients can specify the header `Accept-Encoding: gzip` and the server will compress responses.  
Responses will be returns with `Content-Encoding: gzip`.

Given the size of matches, this can have significant performance benefits.


## Pagination


Where applicable, the server allows requests to limit the number of results
returned via pagination. To paginate the primary data, supply pagination information
to the query portion of the request using the limit and offset parameters.  
To fetch items 2 through 10 you would specify a limit of 8 and an offset of 2:

If not specified, the server will default for matches to`limit=5` and `offset=0`, and for players/samples to `limit=50` and `offset=0`

<aside class="warning">
Important - Currently the server will not allow responses with over 50 primary data objects
</aside>

## Time

* The max search time span between createdAt-start and createdAt-end is 28 days
* If you don't specify either createdAt-start, or createdAt-end, the default is: current time - 28 days
* If you specify only createdAt-start, createdAt-end is 28 days after or current time, whichever hits first
* If you specify only createdAt-end, createdAt-start is 28 days prior
* If you specify a future time, and are within the 28 day limit, the createdAt-end will default to current time

{% method %}
## Sorting

>The example below will return the oldest articles first:
{% sample lang="shell" %}
```shell
".../matches?sort=createdAt"
```
{% sample lang="java" %}
```java
".../matches?sort=createdAt"
```
{% sample lang="python" %}
```python
".../matches?sort=createdAt"
```
{% sample lang="ruby" %}
```ruby
".../matches?sort=createdAt"
```
{% sample lang="js" %}
```javascript
".../matches?sort=createdAt"
```
{% sample lang="go" %}
```go
".../matches?sort=createdAt"
```
{% common %}
>The example below will return the newest articles first.
{% sample lang="shell" %}

```shell
".../matches?sort=-createdAt"
```
{% sample lang="java" %}

```java
".../matches?sort=-createdAt"
```
{% sample lang="python" %}

```python
".../matches?sort=-createdAt"
```
{% sample lang="ruby" %}
```ruby
".../matches?sort=-createdAt"
```
{% sample lang="js" %}

```javascript
".../matches?sort=-createdAt"
```
{% sample lang="go" %}
```go
".../matches?sort=-createdAt"
```
{% endmethod %}
The default sort order is always ascending. Ascending corresponds to the
standard order of numbers and letters, i.e. A to Z, 0 to 9).  For dates and times, ascending means that earlier values precede later ones e.g. 1/1/2000 will sort ahead of 1/1/2001.

All resource collections have a default sort order.  In addition, some resources provide the ability to sort according to one or more criteria ("sort fields").

If sort fields are is prefixed with a minus, the order will be changed to descending.

{% method %}
## JSON-P Callbacks

{% sample lang="shell" %}

```shell
curl -g "https://api.dc01.gamelockerapp.com/status?callback=foo"
```
{% endmethod %}

You can send a ?callback parameter to any GET call to have the results wrapped in a JSON function. This is typically used when browsers want to embed content in web pages by getting around cross domain issues. The response includes the same data output as the regular API, plus the relevant HTTP Header information.


{% method %}

## Cross Origin Resource Sharing

The API supports Cross Origin Resource Sharing (CORS) for AJAX requests from any origin.
You can read the CORS W3C Recommendation, or this intro from the HTML 5 Security Guide.

Here's a sample request sent from a browser hitting http://example.com:


{% sample lang="shell" %}
```shell
curl -i https://api.dc01.gamelockerapp.com/status -H "Origin: http://example.com"
  HTTP/1.1 200 OK
  ...
  Access-Control-Allow-Origin: *
  Access-Control-Expose-Headers: Content-Length
```
{% common %}
This is what the CORS preflight request looks like:

{% sample lang="shell" %}
```shell
curl -i https://api.dc01.gamelockerapp.com/status -H "Origin: http://example.com" -X OPTIONS
  HTTP/1.1 200 OK
  ...
  Access-Control-Allow-Headers: Origin,X-Title-Id,Authorization
  Access-Control-Allow-Methods: GET,POST,OPTIONS
  Access-Control-Allow-Origin: *
  Access-Control-Max-Age: 86400
```
{% endmethod %}

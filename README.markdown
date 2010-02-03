# tastyTopia API for orderTopia (version 0.1)

## Introduction

The tastyTopia API represents a live, real-time view of the rich dataset that orderTopia holds. This document provides information to developers and other interested parties about our domain model and how to integrate with orderTopia.

## Making API requests

tastyTopia provides a [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)-ful resource-oriented API that returns responses in the [JSON](http://en.wikipedia.org/wiki/JSON) format. In its simplest incarnation, you describe the resource or resources you're looking for, and we respond with the results.

### <a name="auri"></a>Address

The API is available at `http://api.ordertopia.com:8080`, also called the __API URI__, or __AURI__. You can check the status of the API by making a `GET` request for this page; if successful, it is currently operational. You can make requests to and will receive responses from this location.

### Authentication

At present, the API is public access but in beta. No authentication is required, although we will rate-limit excessive usage. Use it judiciously.

### Request format

All requests must be prefixed by the [AURI](#auri). After that, the general format of a request is:

    /<resource>/[<action>|<id>].<format>[?[<filter>=<value>]&...[<filter>=<value>]]

Specifically, a request consists of:

* a __resource__, followed by
* exactly one of an __action__ or a __resource identifier__;
* concatenated by `.` to a __format__;
* followed by zero or more __filter-value pairs__,
    * the first such pair starting with `?` and subsequent ones starting with `&`,
    * and each such pair consisting of a __filter__ concatenated by `=` with its corresponding __value__.

<table>
  <tr>
    <th>field</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
  </tr>
  <tr>
    <td>resource</td>
    <td>One of the <a href="#resource">tastyTopia resources</a>.</td>
    <td>string</td>
    <td>"customers"</td>
  </tr>
  <tr>
    <td>action</td>
    <td>Action to perform. Must be one of the <a href="#supported-actions">supported actions</a>.</td>
    <td>string</td>
    <td>"list"</td>
  </tr>
  <tr>
    <td>id</td>
    <td>An identifier for the resource specified.</td>
    <td>number</td>
    <td>500</td>
  </tr>
  <tr>
    <td>format</td>
    <td>Format to respond in. Must be one of the <a href="#supported-formats">supported formats</a>.</td>
    <td>string</td>
    <td>"json"</td>
  </tr>
  <tr>
    <td>filter</td>
    <td>Filter to apply to results. Must be one of the <a href="#filters">supported filters</a>.</td>
    <td>string</td>
    <td>"limit"</td>
  </tr>
   <tr>
    <td>value</td>
    <td>Value to pass to this filter.</td>
    <td><em>(varies; see <a href="#filters">filters</a>)</em></td>
    <td>100</td>
  </tr>
</table>

A request is said to be __conforming__ if it meets the above specification. Nonconforming requests might still be accepted, but we don't make any promises. A request is said to be __valid__ if it has acceptable values, regardless of syntax. A request is called __correct__ if it's both valid and conforming. Note that a correct request might still result in a failure [response](#response) -- for example, if you ask for a resource that doesn't exist.

Here are some examples of different requests:

* `http://ordertopia.com/foo/5.json`: __Invalid__; requests must start with the AURI.
* `http://api.ordertopia.com:8080/foo/5.json`: __Invalid__; `foo` is not an orderTopia resource.
* `http://api.ordertopia.com:8080/merchants/json.5`: __Non-conforming__; `id` should come before `format`.
* `http://api.ordertopia.com:8080/merchants/5.json`: __Correct__. Retrieves `merchant` resource with `id` 5 in the `json` format.
* `http://api.ordertopia.com:8080/merchants/list.json?limit=10`: __Correct__. Lists the first 10 `merchant` resources.

#### <a name="supported-actions"></a>Supported actions

The following actions are supported:

* `list`: List all matching resources.

#### <a name="supported-formats"></a>Supported formats

The following formats are supported:

* `json`: Standards-compliant [JavaScript Object Notation](http://json.org).

### HTTP status codes

Part of the headers you receive back in any tastyTopia response will be a HTTP status code. There are several possibilities for this value:

<table>
  <tr>
    <th>status code</th>
    <th>name</th>
    <th>description</th>
  </tr>
  <tr>
    <td>200</td>
    <td>Success</td>
    <td>You asked for a resource or resources. Here you go!</td>
  </tr>
  <tr>
    <td>300</td>
    <td>Multiple Choices</td>
    <td>You asked me for something, but you weren't specific enough. Which one of these did you mean?</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Bad Request</td>
    <td>You have a syntax error or errors, or something else is wrong about what you asked me to do.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>Unauthorized</td>
    <td><em>(Not used yet.)</em> You didn't supply appropriate credentials.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>Forbidden</td>
    <td>You are not authorized to make that kind of request.</td>
  </tr>
  <tr>
    <td>404</td>
    <td>Not Found</td>
    <td>The resource you described doesn't match anything I know about.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Server Error</td>
    <td>Something went wrong, so I can't give you what you asked for right now. Sorry about that.</td>
  </tr>
  <tr>
    <td>501</td>
    <td>Not Implemented</td>
    <td>I understood your request, but I don't support that kind of request right now.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Service Unavailable</td>
    <td>I'm not able to respond to requests right now.</td>
  </tr>
</table>

A code of 200 indicates success. Anything else means that there was a problem with the request.

### <a name="#response"></a>Responses

When you receive a response, the body will contain a top-level __response object__, in JSON form. It will have the following general structure:

    {
      "count": ...,
      "resource": ...,
      "params": ...,
      "results": [
        ...
      ]
    }

These fields have the following meaning.

<table>
  <tr>
    <th>field</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
  </tr>
  <tr>
    <td>resource</td>
    <td>Type of results you will get; will be one of {"primitive", [resource], "error"}, where [resource] is the name of <a href="#resource">any tastyTopia resource.</a></td>
    <td>string</td>
    <td>"resource": "customers"</td>
  </tr>
  <tr>
    <td>count</td>
    <td>Number of results returned.</td>
    <td>number</td>
    <td>"count": 5</td>
  </tr>
  <tr>
    <td>params</td>
    <td>Parameters that were received from your request, in the form of a map of key-value string pairs.</td>
    <td>object</td>
    <td>"params": {"id": 6}</td>
  </tr>
</table>

#### <a name="success-responses"></a>Successful responses

If you receive a HTTP 200 header, your request was successful and tastyTopia was able to give you what you asked for. Your response will contain a resource array populated with the results. Here is an example of a successful request and response:

    http://api.ordertopia.com:8080/merchants/100.json

    HTTP/1.1 200 OK
    Date: Wed, 03 Feb 2010 19:01:14 GMT
    Server: Apache/2.2.3 (CentOS)
    Pragma: no-cache
    Cache-Control: no-cache
    Content-Length: 191
    Connection: close
    Content-Type: application/json; charset=utf-8

    {
      "count": 1,
      "resource": "merchants",
      "params": {"id": "100"},
      "results": [
        {
          "url": "http://ordertopia.com/merchants/tastytaters",
          "description": "Tasty Taters sells a variety of scrumptiously starchy tubers!",
          "id": 52,
          "title": "Tasty Taters"
        }
      ]
    }

#### Failed responses

If you receive any header other than HTTP 200, your request was not necessarily successful:

* If your [response object][response] has a resource of `error`, then your request failed.
* Otherwise, it was successful (see [Successful Responses](#success-responses) above).

If your request failed, the response will contain one or more [errors][error] which you may consult for additional information.

## <a name="resource"></a>Resources

__Resources__ form the vocabulary of things you can work with in tastyTopia.

### <a name="merchant"></a>Merchants

![merchants](http://yuml.me/689d321a)

__Merchants__ are businesses which operate [stores][store]. They earn income by selling their [products][product] to [customers][customer].

#### Fields

<table>
  <tr>
    <th>field</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
  </tr>
  <tr>
    <td>id</td>
    <td>Merchants's unique identifier.</td>
    <td>identifier</td>
    <td>10</td>
  </tr>
  <tr>
    <td>title</td>
    <td>Merchant's business name.</td>
    <td>string</td>
    <td>Tasty Taters, Inc.</td>
  </tr>
  <tr>
    <td>url</td>
    <td>Merchant's primary website.</td>
    <td>string</td>
    <td>http://ordertopia.com/merchants/tastytaters</td>
  </tr>
  <tr>
    <td>description</td>
    <td>Short textual description of merchant and its offerings.</td>
    <td>string</td>
    <td>Tasty Taters offers scrumptiously starchy tubers of all kinds!</td>
  </tr>
</table>

### <a name="store"></a>Categories

![categories](http://yuml.me/1412243a)

__Categories__ are arbitrary, [merchant][merchant]-defined groupings of [products][product]. Typically, merchants categorize products according to similarity in function or type. Categories may have a parent category.

#### Fields

<table>
  <tr>
    <th>field</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
  </tr>
  <tr>
    <td>id</td>
    <td>Category's unique identifier.</td>
    <td>identifier</td>
    <td>30</td>
  </tr>
  <tr>
    <td>merchant_id</td>
    <td>Merchant that this category belongs to.</td>
    <td>identifier</td>
    <td>60</td>
  </tr>
  <tr>
    <td>name</td>
    <td>Name of this category.</td>
    <td>string</td>
    <td>"Cold Beverages"</td>
  </tr>
  <tr>
    <td>parent</td>
    <td>The parent id of this category.</td>
    <td>identifier</td>
    <td>29</td>
  </tr>
</table>

### <a name="customer"></a>Customers

![customers](http://yuml.me/80faa1a)

A __Customer__ is an individual person who purchases from a [merchant's][merchant] [stores][store]. The same [user][user] may represent many customers, each with a different presence at a different merchant.

#### Fields

<table>
  <tr>
    <th>field</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
  </tr>
  <tr>
    <td>id</td>
    <td>The customer's unique identifier.</td>
    <td>identifier</td>
    <td>40</td>
  </tr>
  <tr>
    <td>first_name</td>
    <td>Customer's first name.</td>
    <td>string</td>
    <td>Alice</td>
  </tr>
  <tr>
    <td>last_name</td>
    <td>Customer's last name.</td>
    <td>string</td>
    <td>Waithersford-Humphreys</td>
  </tr>
  <tr>
    <td>street_address</td>
    <td>Customer's mailing address.</td>
    <td>string representing a mailing address</td>
    <td>1 OrderTopia Drive, Charlottesville, VA 22901</td>
  </tr>
  <tr>
    <td>phone_number</td>
    <td>Customer's primary phone number contact.</td>
    <td>string representing a phone number</td>
    <td>+1-567-890-0123</td>
  </tr>
  <tr>
    <td>email</td>
    <td>Customer's designated e-mail address.</td>
    <td>string representing an email address</td>
    <td>alicew@example.com</td>
  </tr>
</table>

### <a name="store"></a>Products

![products](http://yuml.me/2fd2023f)

__Products__ are the offerings of a [merchant][merchant] to their [customers][customer]. Complex products may require customers to make some choices about their contents, in which case they will have one or more [slots][slot].

#### Fields

<table>
  <tr>
    <th>field</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
  </tr>
  <tr>
    <td>id</td>
    <td>The product's unique identifier.</td>
    <td>identifier</td>
    <td>50</td>
  </tr>
  <tr>
    <td>name</td>
    <td>Short name for the product.</td>
    <td>string</td>
    <td>Tater Volcano</td>
  </tr>
  <tr>
    <td>description</td>
    <td>Description of the product.</td>
    <td>string</td>
    <td>A scrumptious but geologically unstable starchy treat, smothered with your choice of gravy or cheese.</td>
  </tr>
  <tr>
    <td>categories</td>
    <td>Categories that this product belongs to.</td>
    <td>array of identifiers</td>
    <td>[2, 8, 15]</td>
  </tr>
  <tr>
    <td>slots</td>
    <td>Slots present on this item.</td>
    <td>array of slots</td>
    <td><em>(see <a href="#slot">slot</a> section)</em></td>
  </tr>
</table>

### <a name="slot"></a>Slots

![](http://yuml.me/2b4f33bc)

__Slots__ represent decisions that can be made about a [product][product], like what kind of toppings to get on a sundae. A slot is filled with other products (which themselves can also have slots!). Which products may go in a slot depend on the [categories][category] specified by that slot. The number of products matching that specification that may go into a slot is determined by the slot's minimum and maximum size.

#### Fields

<table>
  <tr>
    <th>field</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
  </tr>
  <tr>
    <td>min</td>
    <td>Minimum number of items that may go in this slot.</td>
    <td>number</td>
    <td>0</td>
  </tr>
  <tr>
    <td>max</td>
    <td>Maximum number of items that may go in this slot.</td>
    <td>number</td>
    <td>5</td>
  </tr>
  <tr>
    <td>categories</td>
    <td>Product categories which may fill this slot.</td>
    <td>array of identifiers</td>
    <td>[83, 86, 199]</td>
  </tr>
</table>

### <a name="store"></a>Stores

__Stores__ are a [merchant's][merchant] physical or online presences, through which they sell [products][product]. Presently, each merchant has only one store.

## Special Resources

These resources aren't true domain resources in orderTopia, but they're nonetheless important.

### <a name="error"></a>Errors

__Errors__ are simply vehicles for delivering information about errors.

#### Fields

<table>
  <tr>
    <th>field</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
  </tr>
  <tr>
    <td>msg</td>
    <td>Message payload for this error.</td>
    <td>string</td>
    <td>"filter 'mechant_id' is not a valid filter; did you mean 'merchant_id'?"</td>
  </tr>
</table>

## <a name="filters"></a>Filters

The following filters with these values are supported:

<table>
  <tr>
    <th>filter</th>
    <th>description</th>
    <th>format</th>
    <th>filtered count?</th>
  </tr>
  <tr>
    <td><code>limit = n</code></td>
    <td>Return no more than <code>n</code> items.</td>
    <td>number</td>
    <td>no</td>
  </tr>
  <tr>
    <td><code>offset = n</code></td>
    <td>Return items beginning at the <code>n+1</code>th item.</td>
    <td>number</td>
    <td>no</td>
  </tr>
  <tr>
    <td><code>category_id = n</code></td>
    <td>Return anything tagged with this category.</td>
    <td>identifier</td>
    <td>yes</td>
  </tr>
  <tr>
    <td><code>merchant_id = n</code></td>
    <td>Return anything tagged with this merchant.</td>
    <td>identifier</td>
    <td>yes</td>
  </tr>
</table>

When you apply some filters, the `count` field in your response may be different from the number of items in the `results` collection. This is done so that you know how many additional results are available that matched your base request, which is helpful when paging through long requests. Filters with this property are identified above with a "no" entry in the "filtered count?" column, and are called __non-reducing filters__.

In such cases, the `count` field will be equal to the number of results that would have been returned had you not applied any non-reducing filters.

[category]: #category
[customer]: #customer
[merchant]: #merchant
[product]: #product
[slot]: #slot
[store]: #store
[user]: #user

[response]: #response

[error]: #error

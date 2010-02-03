# orderTopia API (version 0.1)

## Introduction

The tastyTopia represents a live, real-time view of the rich dataset that orderTopia holds. This document provides information to developers and other interested parties about our domain model and how you can integrate with orderTopia.

## Request/responses

tastyTopia provides a [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)-ful resource-oriented API that returns responses in the [JSON](http://en.wikipedia.org/wiki/JSON) format. In its simplest incarnation, you describe the resource or resources you're looking for, and we respond with the results.

### Address

The API is available at `http://api.ordertopia.com:8080`. You can check the status of the API by making a request for this page; if successful, it is currently operational. You can make requests to and will receive responses from this location.

### Authentication

At present, the API is public access but in beta. No authentication is required, although we will rate-limit excessive usage. Use it judiciously.

### HTTP status codes

Part of the headers you receive back in any tastyTopia response will be an HTTP status code. There are several possibilities for this value:

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
    <td>Type of results you will get; will be one of {"primitive", [entity], "error"}, where [entity] is the name of <a href="#entities">any tastyTopia entity.</a></td>
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

#### Successful responses

If you receive an HTTP 200 header, your response was successful and tastyTopia was able to give you what you asked for. Your response will contain a resource array populated with the results.

#### Failed responses

If you receive any header other than HTTP 200, your response was not necessarily successful:

* If your [response object][response] has a resource of `error`, then your response failed.
* If your
* Otherwise, it was successful.

and the and tastyTopia was able to give you what you asked for. Your response will contain a resource array populated with the results.

## <a name="entities"></a>Entities

### <a name="merchant"></a>Merchants

![merchants](http://yuml.me/689d321a)

__Merchants__ are businesses which operate [stores][store]. They earn income by selling their [products][product] to [customers][customer].

#### Attributes

<table>
  <tr>
    <th>attribute</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
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
    <td>Tasty Taters offers delicious starchy tubers of all kinds!</td>
  </tr>
</table>

### <a name="customer"></a>Customers

![customers](http://yuml.me/80faa1a)

A __Customer__ is an individual person who purchases from a [merchant's][merchant] [stores][store]. The same [user][user] may represent many customers, each with a different presence at a different merchant.

#### Attributes

<table>
  <tr>
    <th>attribute</th>
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

![customers](http://yuml.me/3298b9e5)

__Products__ are the offerings of a [merchant][merchant] to their [customers][customer]. Complex products may require customers to make some choices about their contents, in which case they will have one or more [slots][slot].

#### Attributes

<table>
  <tr>
    <th>attribute</th>
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

__Slots__ represent decisions that can be made about a product, like what kind of toppings to get on a sundae.

### <a name="store"></a>Stores

__Stores__ are a [merchant's][merchant] physical or online presences, through which they sell [products][product]. Presently, each merchant has only one store.

## Special Entities

These entities aren't true domain entities in orderTopia.

### <a name="error"></a>Errors

__Errors__ are simply vehicles for delivering information about errors. No other entity has any relationship with errors.

#### Attributes

<table>
  <tr>
    <th>attribute</th>
    <th>description</th>
    <th>format</th>
    <th>example</th>
  </tr>
  <tr>
    <td>msg</td>
    <td>Message payload for this error.</td>
    <td>string</td>
    <td>"filter 'mechant_id' is not a valid filter; did you mean 'merchant_id'?</td>
  </tr>
</table>

## Filters

[customer]: #customer
[merchant]: #merchant
[product]: #product
[slot]: #slot
[store]: #store
[user]: #user

[response]: #response

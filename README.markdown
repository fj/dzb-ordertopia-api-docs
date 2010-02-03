# orderTopia API (version 0.1)

## Introduction

The orderTopia API is a read-only, live, real-time view of the data in orderTopia's servers.

## Request/responses

### Address

The API is available at `http://api.ordertopia.com:8080`.

### General structure

### HTTP status codes

## Entities

### <a name="merchant"></a>Merchants

![merchants](http://yuml.me/689d321a)

__Merchants__ are businesses which operate [stores][store]. They earn income by selling their [products][product] to [customers][customer].

#### Properties

<table>
  <tr>
    <th>property</th>
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
    <td>URL</td>
    <td>Merchant's primary website.</td>
    <td>string</td>
    <td>http://ordertopia/merchants/tastytaters.com</td>
  </tr>
  <tr>
    <td>description</td>
    <td>Short textual description of merchant and its offerings.</td>
    <td>string</td>
    <td>Tasty Taters offers delicious starchy tubers of all kinds!</td>
  </tr>
</table>

### <a name="store"></a>Stores

__Stores__ are a [merchant's][merchant] physical or online presences, through which they sell [products][product]. Presently, each merchant has only one store.

### <a name="customer"></a>Customers

![customers](http://yuml.me/diagram/scruffy/class/%5BCustomer%7Bbg%3Aorange%7D%5D%2C%20%5BUser%5D++1-%3E*%5BCustomer%5D%2C%20%5BMerchant%5D++1-%3E*%5BCustomer%5D)

A __Customer__ is an individual person who purchases from a [Merchant's][merchant] [Stores][store]. The same [User][user] may represent many Customers, each with a different presence at a different merchant.

## Filters

[customer]: #customer
[merchant]: #merchant
[product]: #product
[store]: #store
[user]: #user

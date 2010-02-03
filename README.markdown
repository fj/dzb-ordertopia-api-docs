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
    <td>http://ordertopia/merchants/tastytaters.com</td>
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

## Filters

[customer]: #customer
[merchant]: #merchant
[product]: #product
[slot]: #slot
[store]: #store
[user]: #user

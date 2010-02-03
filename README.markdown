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

__Merchants__ are businesses which operate __Stores__. 

### <a name="customer"></a>Customers

<!--
[Customer{bg:orange}]
[User]++1->*[Customer]
[Merchant]++1->*[Customer]
-->
![customers](http://yuml.me/diagram/scruffy/class/[Customer{bg:orange}], [User]++1->*[Customer])

A __Customer__ is an individual person who purchases from a [Merchant's][merchant] stores. The same [User][user] may represent many Customers.


## Filters

[merchant]: #merchant
[customer]: #customer
[store]: #store
[user]: #user

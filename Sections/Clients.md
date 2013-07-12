# Clients

## GET ALL CLIENTS

    GET `/clients`

    HTTP Response: 200 Success

```xml
    <clients type="array">
      <client>
        <name>SuprCorp</name>
        <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
        <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
        <highrise-id type="integer">568902</highrise-id>
        <id type="integer">11072</id>
        <cache-version type="integer">0</cache-version>
        <currency>United States Dollars - USD</currency>
        <active type="boolean">true</active>
        <currency-symbol>$</currency-symbol>
        <details>123 Main St.
    Third Floor
    New York, NY 10011
    USA
    212-555-1212
    212-555-1213 (fax)
    http://www.company.com</details>
        <default-invoice-timeframe>20110101,20111231</default-invoice-timeframe>
        <last-invoice-kind>task</last-invoice-kind>
      </client>
      <client>
       ...
      </client>
    </clients>
```

You can filter by updated_since. To show only the clients that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

    GET `/clients?updated_since=2010-09-25+18%3A30`

    HTTP Response: 200 Success

## GET A CLIENT

    GET `/clients/#{client_id}`

    HTTP Response: 200 Success

```xml
    <client>
      <name>SuprCorp</name>
      <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
      <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
      <highrise-id type="integer">110011</highrise-id>
      <id type="integer">11072</id>
      <cache-version type="integer">0</cache-version>
      <currency>United States Dollars - USD</currency>
      <currency-symbol>$</currency-symbol>
      <active type="boolean">true</active>
      <details>123 Main St.
    Third Floor
    New York, NY 10011
    USA
    212-555-1212
    212-555-1213 (fax)
    http://www.company.com</details>
      <default-invoice-timeframe>20110101,20111231</default-invoice-timeframe>
      <last-invoice-kind>task</last-invoice-kind>
    </client>
```

## CREATE A NEW CLIENT

    POST `/clients`

    HTTP Response: 201 Created
    Location: /clients/#{new_client_id}

You need to post the following:

```xml
    <client>
      <name>Company LLC</name>
      <highrise-id type="integer">110011</highrise-id>
      <currency>United States Dollars - USD</currency>
      <currency-symbol>$</currency-symbol>
      <active type="boolean">true</active>
      <details>123 Main St.
    Third Floor
    New York, NY 10011
    USA
    212-555-1212
    212-555-1213 (fax)
    http://www.company.com</details>
    </client>
```

Note: Only NAME is required.

## UPDATE CLIENT

    PUT `/clients/#{client_id}`

    HTTP Response: 200 OK
    Location: /clients/#{client_id}

You may update selected attributes for a client.

```xml
    <client>
      <name>Company LLC</name>
      <details>123 Main St.
    Third Floor
    New York, NY 10011
    USA
    212-555-1212
    212-555-1213 (fax)
    http://www.company.com</details>
    </client>
```

## (DE)ACTIVATE AN EXISTING CLIENT

    POST `/clients/#{client_id}/toggle`

    HTTP Response: 200 Success
    Location: /clients/#{client_id}

Note that if the client has active projects, Harvest will return HTTP Response: 400 Bad Request, with a Hint header.

## DELETE A CLIENT

    DELETE `/clients/#{client_id}`

If client does not have associated projects or invoices Harvest deletes it and returns HTTP Response: 200 OK otherwise client is not deleted and you'll get a HTTP Response: 400 Bad Request .
# This documentation is out of date. Please see our new API docs [here!](http://help.getharvest.com/api)

## Invoice Item Categories

Important: this assumes invoice feature is enabled for your account.

Here you can change the categories appearing before each line item on your invoice. By default Harvest provides two non-removable categories, for the hours and for the expenses you bill. You can create many more categories and change the name of the default ones.

## SHOW ALL INVOICE ITEM CATEGORIES

GET `/invoice_item_categories`

HTTP Response: 200 Success

```xml
<invoice-item-categories type="array">
  <invoice-item-category>
    <id type="integer">39353</id>
    <name>Hosting</name>
    <updated-at type="datetime">2008-03-05T22:50:44Z</updated-at>
    <use-as-expense type="boolean">false</use-as-expense>
    <use-as-service type="boolean">false</use-as-service>
    <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
    <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
  </invoice-item-category>
  <invoice-item-category>
    <id type="integer">1</id>
    <name>Service</name>
    <updated-at type="datetime">2008-01-30T12:02:41Z</updated-at>
    <use-as-expense type="boolean">false</use-as-expense>
    <use-as-service type="boolean">true</use-as-service>
    <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
    <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
  </invoice-item-category>
  ...
</invoice-item-categories>
```

## CREATE NEW INVOICE ITEM CATEGORY

POST `/invoice_item_categories`

HTTP Response: 201 Created

Location: /invoice_categories/#{new_category_id}

Sample post:

```xml
<invoice-item-category>
  <name>Entertainment</name>
</invoice-item-category>
```

## UPDATE AN EXISTING CATEGORY

PUT `/invoice_item_categories/#{category_id}`

HTTP Response: 200 OK

Location: /invoice_item_categories/#{category_id}

Sample put:

```xml
<invoice-item-category>
  <name>Entertainment</name>
</invoice-item-category>
```

## DELETE EXISTING CATEGORY

DELETE `/invoice_item_categories/#{expense_category_id}`

HTTP Response: 200 OK.

HTTP Response: 400 Bad Request

Returned if category is not removable (default categories can only be edited).

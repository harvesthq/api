# This documentation is out of date. Please see our new API docs [here!](http://help.getharvest.com/api)

## Expense Categories

## SHOW ALL EXPENSE CATEGORIES

GET `/expense_categories`

HTTP Response: 200 Success

```xml
<expense-categories>
  <expense-category>
    <name>Mileage</name>
      <!-- unit values only populated for unit-based expenses -->
    <unit-name>Miles</unit-name>
    <unit-price type="decimal">0.485</unit-price>
      <!-- If true no expenses can be logged to this category -->
    <deactivated type="boolean">false</deactivated>
    <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
    <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
  </expense-category>
  <expense-category>
     ...
  </expense-category>
  ...
</expense-categories>
```

You can filter by updated_since. To show only the expense categories that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

GET `/expense_categories?updated_since=2010-09-25+18%3A30`

HTTP Response: 200 Success

## CREATE NEW EXPENSE CATEGORY

POST `/expense_categories`

HTTP Response: 201 Created

Location: /expense_categories/#{new_expense_category_id}

Sample post:

```xml
<expense-category>
  <name>Mileage</name>
    <!-- unit values only necessary for unit-based expenses -->
  <unit-name>Miles</unit-name>
  <unit-price type="decimal">0.485</unit-price>
</expense-category>
```

## UPDATE AN EXISTING EXPENSE CATEGORY

PUT `/expense_categories/#{expense_category_id}`

HTTP Response: 200 OK

Location: /expense_categories/#{expense_category_id}

Sample put:

```xml
<expense-category>
  <name>Mileage</name>
    <!-- unit values only necessary for unit-based expenses -->
  <unit-name>Miles</unit-name>
  <unit-price type="decimal">0.485</unit-price>
</expense-category>
```

## SHOW EXPENSE CATEGORY

GET `/expense_categories/#{expense_category_id}`

HTTP Response: 200 Success

```xml
<expense-category>
  <name>Mileage</name>
    <!-- unit values only populated for unit-based expenses -->
  <unit-name>Miles</unit-name>
  <unit-price type="decimal">0.485</unit-price>
    <!-- If true no expenses can be logged to this category -->
  <deactivated type="boolean">false</deactivated>
  <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
  <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
</expense-category>
```

## DELETE EXISTING EXPENSE CATEGORY

DELETE `/expense_categories/#{expense_category_id}`

HTTP Response: 200 OK

Returned if expense category does not have any reported expenses associated.

HTTP Response: 400 Bad Request

Returned if expense category is not removable.

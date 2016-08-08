# This documentation is out of date. Please see our new API docs [here!](http://help.getharvest.com/api)

## Invoices

Important: this assumes that the invoice feature is enabled for your account (via. "Choose Modules" on your Account Settings page).

## SHOW RECENTLY CREATED INVOICES

GET `/invoices`

HTTP Response: 200 Success

```xml
<invoices type="array">
  <invoice>
    <id type="integer">1234567</id>
    <amount type="decimal">1155.0</amount>
    <due-amount type="decimal">0.0</due-amount>
    <due-at type="date">2008-02-06</due-at>
      <!-- human representation for due at -->
    <due-at-human-format>due upon receipt</due-at-human-format>
      <!-- invoiced period, present for generated invoices -->
    <period-end type="date" nil="true"></period-end>
    <period-start type="date" nil="true"></period-start>
    <client-id type="integer">46066</client-id>
    <subject>Invoice for project X</subject>
      <!-- see  -->
    <currency>United States Dollar - USD</currency>
    <issued-at type="date">2008-02-06</issued-at>
    <created-by-id type="integer">123456</created-by-id>
    <notes></notes>
    <number>8008</number>
    <purchase-order></purchase-order>
    <client-key>f2a56b1232ad1asdf5926f040e8cff2befb5e8f1</client-key>
      <!-- See invoice messages and invoice payments for manipulating the state attribute.  Direct assigment will be ignored. Options are open, draft, partial, paid and closed.-->
    <state>paid</state>
      <!-- applied tax percentage, blank if not taxed -->
    <tax type="decimal" nil="true"></tax>
      <!-- applied tax 2 percentage, blank if not taxed -->
    <tax2 type="decimal" nil="true"></tax2>
      <!-- the first tax amount -->
    <tax-amount type="decimal" nil="true"></tax-amount>
      <!-- the second tax amount -->
    <tax-amount2 type="decimal" nil="true"></tax-amount2>
      <!-- discount -->
    <discount-amount type="decimal">0.0</discount-amount>
    <discount type="decimal" nil="true"></discount>
      <!-- is it recurring? -->
    <recurring-invoice-id type="integer" nil="true"></recurring-invoice-id>
      <!-- was this converted from an estimate? -->
    <estimate-id type="integer" nil="true"></estimate-id>
      <!-- a retainer_id will only be present if the invoice funds a retainer -->
    <retainer-id type="integer">12345</retainer-id>
    <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
    <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
  </invoice>
  ...
</invoices>
```

The state attribute may be open for unpaid, sent invoices (either current or past due), draft, partial for invoices with partial payments, paid for paid in full invoices, or closed for written off invoices.

For performance reasons line item data is not shown for this call, you'll have to use a more specific API call.

By default Harvest will display 50 records per page. You can get further pages by passing in a page parameter.

GET `/invoices?page=2`

HTTP Response: 200 Success

You can also query invoices by timeframe (date created) based on issue date.

GET `/invoices?from=YYYY-MM-DD&to=YYYY-MM-DD`

HTTP Response: 200 Success

You can filter by updated_since. To show only the invoices that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

GET `/invoices?updated_since=2010-09-25+18%3A30`

HTTP Response: 200 Success

Filtering by invoice state is also possible, for example this would return partially paid invoices:

GET `/invoices?status=partial`

HTTP Response: 200 Success

Other parameters for filtering by state are:

- `open` - sent to the client but no payment recieved
- `partial` - partial payment was recorded
- `draft` - Harvest did not sent this to a client, nor recorded any payments
- `paid` - invoice paid in full
- `unpaid` - unpaid invoices
- `pastdue` - past due invoices

You can also filter by client. For example, to show only the invoices belonging to a client with the id 23445

GET `/invoices?client=23445`

HTTP Response: 200 Success

All of the above filters can be combined, allowing for powerful queries into your records.

## SHOW A PARTICULAR INVOICE

GET `/invoices/#{invoice_id}`

HTTP Response: 200 Success

```xml
<invoice>
  <amount type="decimal">253.44</amount>
  <client-id type="integer">8</client-id>
  <subject>Invoice for Project X</subject>
  <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
  <currency>United States Dollar - USD</currency>
  <due-amount type="decimal">253.44</due-amount>
  <due-at type="date">2008-02-06</due-at>
  <due-at-human-format>due upon receipt</due-at-human-format>
  <id type="integer">1421</id>
  <issued-at type="date">2008-02-06</issued-at>
  <notes>Some notes go here</notes>
  <number>82208</number>
  <period-end type="date">2008-03-31</period-end>
  <period-start type="date">2007-06-26</period-start>
  <purchase-order nil="true"></purchase-order>
  <client-key>f2a56b1232ad1asdf5926f040e8cff2befb5e8f1</client-key>
    <!-- See invoice messages and invoice payments for manipulating the state attribute.  Direct assigment will be ignored. -->
  <state>draft</state>
  <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
  <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
    <!-- Line items in CSV format -->
  <csv-line-items>
  kind,description,quantity,unit_price,amount,taxed,taxed2,project_id
  Service,Security support / Apply upgrades,0.68,80.00,54.4,true,false,3
  Service,Security support / Intrusion detection,0.57,80.00,45.6,true,false,3
  Service,Backend Programming / Forum admin,1.20,80.00,96.0,true,false,3
  </csv-line-items>
    <!-- applied a 10% tax on all line items marked as taxed -->
  <tax type="decimal">10</tax>
    <!-- no second tax -->
  <tax2 type="decimal" nil="true"></tax2>
    <!-- tax amount -->
  <tax-amount type="decimal">23.04</tax-amount>
  <tax2-amount type="decimal" nil="true">0</tax2-amount>
</invoice>
```

You can build a URL to the public web invoice view using the client-key attribute:

`https://<SUBDOMAIN>.harvestapp.com/client/invoices/<CLIENT-KEY>`

## CREATE A NEW INVOICE

POST `/invoices`

HTTP Response: 201 Created

Location: /invoices/#{new_id}

Sample post:

```xml
<invoice>
    <!-- human representation for due at -->
  <due-at-human-format>NET 10</due-at-human-format>
  <client-id type="integer">8</client-id>
    <!-- see list bellow for accepted values -->
  <currency>United States Dollar - USD</currency>
  <issued-at type="date">2008-02-06</issued-at>
  <subject>Your invoice subject goes here</subject>
  <notes>Some notes go here</notes>
  <number>82208</number>
  <!-- allowed values:
    free_form:  creates a free form invoice (non-generated).
                Content is added via CSV.
    project:    gathers content from Harvest grouping by projects
    task:       gathers content from Harvest grouping by task
    people:     gathers content from Harvest grouping by people
    detailed:   includes detailed notes -->
  <kind>project</kind>
    <!-- comma separated project ids to gather data from, useless on free_form invoices -->
  <projects-to-invoice>3</projects-to-invoice>
    <!-- import hours useless on free_form invoices -->
  <import-hours>all</import-hours>
    <!-- import expenses useless on free_form invoices -->
  <import-expenses>all</import-expenses>
    <!-- invoiced period, present for generated invoices -->
  <period-end type="date">2008-03-31</period-end>
  <period-start type="date">2007-06-26</period-start>
    <!-- invoiced period for expenses, present for generated invoices -->
  <expense-period-end type="date">2008-03-31</expense-period-end>
  <expense-period-start type="date">2007-06-26</expense-period-start>
  <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
  <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
</invoice>
```

Note: If no number is passed, a number will be generated automatically.

Note: we accept only a limited set of values for the currency attribute.

Note: The csv-line-items should have their entries enclosed in quotes when they contain extra commas. This is especially important if you are using a number format which uses commas as the decimal separator.

You can also create a free form invoice with line items by passing in the csv-line-items attribute.

Sample post:

```xml
<invoice>
  <due-at-human-format>due upon receipt</due-at-human-format>
  <client-id type="integer">8</client-id>
  <currency>United States Dollar - USD</currency>
  <issued-at type="date">2008-02-06</issued-at>
  <subject>Your invoice subject goes here</subject>
  <notes>Some notes go here</notes>
  <number>82208</number>
  <kind>free_form</kind>
  <tax type="decimal">3</tax>
  <tax2 type="decimal">5</tax2>
    <!-- Line items in CSV format -->
  <csv-line-items>
  kind,description,quantity,unit_price,amount,taxed,taxed2,project_id
  Service,Security support / Apply upgrades,0.68,80.00,54.4,true,true,3
  Service,Security support / Intrusion detection,0.57,80.00,45.6,true,false,3
  Service,Backend Programming / Forum admin,1.20,80.00,96.0,true,false,3
  </csv-line-items>
</invoice>
```

## UPDATE AN EXISTING INVOICE

PUT `/invoices/#{invoice_id}`

HTTP Response: 200 OK

Location: /invoices/#{invoice_id}

Sample put:

```xml
<invoice>
    <!-- human representation for due at -->
  <due-at-human-format>NET 15</due-at-human-format>
  <client-id type="integer">8</client-id>
    <!-- see list bellow for accepted values -->
  <currency>United States Dollar - USD</currency>
  <issued-at type="date">2008-02-06</issued-at>
  <notes>Some notes go here</notes>
  <number>82208</number>
</invoice>
```

You can also update the line items for the invoice by passing in the csv-line-items attribute.

Sample post:

```xml
<invoice>
    <!-- if you don't know the project id, you can just leave it of from data rows ending the row with a comma. CSV header must always be intact though, or updates will be discarded. -->
  <csv-line-items>
  kind,description,quantity,unit_price,amount,taxed,taxed2,project_id
  Service,Branding for marketing pages,20,100.00,2000,false,false,3
  Service,What's in a name,8,100.00,800,false,false,3
  </csv-line-items>
</invoice>
```

Not all attributes need to be passed in, Harvest supports selective updates.

## DELETE EXISTING INVOICE

DELETE `/invoices/#{invoice_id}`

HTTP Response: 200 OK.

# Invoice Payments

Important: this assumes invoice feature is enabled for your account.

## SHOW ALL RECORDED PAYMENTS FOR AN INVOICE

GET `/invoices/#{invoice_id}/payments`

HTTP Response: 200 Success

```xml
<payments type="array">
  <payment>
    <amount type="decimal">1238.0</amount>
    <id type="integer">1334</id>
    <invoice-id type="integer">4393</invoice-id>
    <notes>check</notes>
    <paid-at type="datetime">2008-02-14T00:00:00Z</paid-at>
    <recorded-by>Happy Owner</recorded-by>
    <recorded-by-email>happy@owner.com</recorded-by-email>
    <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
    <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
  </payment>
</payments>
```

## SHOW A PARTICULAR PAYMENT

GET `/invoices/#{invoice_id}/payments/#{payment_id}`

HTTP Response: 200 Success

```xml
<payment>
  <amount type="decimal">52000.0</amount>
  <id type="integer">54664</id>
  <invoice-id type="integer">2393</invoice-id>
  <notes>Finally.</notes>
  <paid-at type="datetime">2008-02-14T00:00:00Z</paid-at>
  <recorded-by>Happy Owner</recorded-by>
  <recorded-by-email>happy@owner.com</recorded-by-email>
  <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
  <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
</payment>
```

## CREATE A NEW PAYMENT

POST `/invoices/#{invoice_id}/payments`

HTTP Response: 201 Created

Location: /invoice/#{new_id}/payments/#{newly_created_payment_id}

Sample post:

```xml
<payment>
    <!-- required -->
  <paid-at type="datetime">2008-02-14T00:00:00Z</paid-at>
    <!-- required -->
  <amount type="decimal">52000.0</amount>
  <notes>Some optional notes go here</notes>
</payment>
```

Payment related invoice attributes like due-amount will be updated. Invoice status will be set to paid if no amount is left due.

## DELETE EXISTING PAYMENT

DELETE `/invoices/#{invoice_id}/payments/#{payment_id}`

HTTP Response: 200 OK.

Payment related invoice attributes like due-amount will be updated. Invoice status will be set to paid if no amount is left due.
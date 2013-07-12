# Invoice Messages

Important: this assumes invoice feature is enabled for your account.

Invoice messages are notes displayed in the activity log section of an invoice.

## SHOW ALL MESSAGES SENT FOR AN INVOICE

    GET `/invoices/#{invoice_id}/messages`

    HTTP Response: 200 Success

```xml
    <invoice-message type="array">
      <invoice-message>
        <body>The message body goes here</body>
        <created-at type="datetime">2008-04-09T20:44:54Z</created-at>
        <id type="integer">409</id>
        <invoice-id type="integer">1420</invoice-id>
        <send-me-a-copy type="boolean">true</send-me-a-copy>
        <sent-by>My Name</sent-by>
        <sent-by-email>my@email.com</sent-by-email>
          <!-- comma separated list of recipient email addresses using the "Name <emai@domain.com>" format -->
        <full-recipient-list>Jane Doe &lt;jane@doe.org&gt;,My Name &lt;my@email.com&gt;</full-recipient-list>
        <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
        <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
      </invoice-message>
       ...
    </invoice-message>
```

## SHOW A PARTICULAR MESSAGE

    GET `/invoices/#{invoice_id}/messages/#{message_id}`

    HTTP Response: 200 Success

```xml
    <invoice-message>
      <body>Anther message body goes here</body>
      <created-at type="datetime">2008-04-09T20:46:34Z</created-at>
      <id type="integer">403</id>
      <invoice-id type="integer">1420</invoice-id>
      <send-me-a-copy type="boolean">true</send-me-a-copy>
      <sent-by>My Name</sent-by>
      <sent-by-email>my@email.com</sent-by-email>
      <full-recipient-list>Jane Doe &lt;jane@doe.org&gt;,My Name &lt;my@email.com&gt;</full-recipient-list>
      <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
      <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
    </invoice-message>
```

## SEND AN INVOICE

    POST `/invoices/#{invoice_id}/messages`

    HTTP Response: 201 Created
    Location: /invoices/#{invoice_id}/messages/#{new_id}

```xml
    <invoice-message>
      <body>Message body</body>
      <recipients>support@harvestapp.com, help@getharvest.com</recipients>
      <attach-pdf type="boolean">true</attach-pdf>
      <send-me-a-copy type="boolean">true</send-me-a-copy>
    </invoice-message>
```

## DELETE EXISTING MESSAGE

    DELETE `/invoices/#{invoice_id}/messages/#{message_id}`

    HTTP Response: 200 OK.

## CREATE A MESSAGE FOR MARKING AN INVOICE AS SENT

    POST `/invoices/#{invoice_id}/messages/mark_as_sent`

    HTTP Response: 200 OK.

```xml
    <invoice-message>
      <body>Optional message body goes here</body>
    </invoice-message>
```

## CREATE A MESSAGE AND MARK AN OPEN INVOICE AS CLOSED (WRITING AN INVOICE OFF)

    POST `/invoices/#{invoice_id}/messages/mark_as_closed`

    HTTP Response: 200 OK.

```xml
    <invoice-message>
      <body>Optional message body goes here</body>
    </invoice-message>
```

## CREATE A MESSAGE AND MARK A CLOSED (WRITTEN-OFF) INVOICE AS OPEN

    POST `/invoices/#{invoice_id}/messages/re_open`

    HTTP Response: 200 OK.

```xml
    <invoice-message>
      <body>Optional message body goes here</body>
    </invoice-message>
```

## CREATE A MESSAGE FOR MARKING AN OPEN INVOICE AS DRAFT

    POST `/invoices/#{invoice_id}/messages/mark_as_draft`

    HTTP Response: 200 OK.
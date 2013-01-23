# Client Contacts

## GET ALL CONTACTS FOR AN ACCOUNT

    GET /contacts

    HTTP Response: 200 Success

    <contacts type="array">
      <contact>
        <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
        <title>President</title>
        <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
        <phone-office>555.555.5555</phone-office>
        <id type="integer">9</id>
        <client-id type="integer">11072</client-id>
        <fax>555.555.9999</fax>
        <last-name>Doe</last-name>
        <email>Jane@Doe.com</email>
        <first-name>Jane</first-name>
        <phone-mobile>555.555.7777</phone-mobile>
      </contact>
    </contacts>

You can filter by updated_since. To show only the contacts that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

    GET /contacts?updated_since=2010-09-25+18%3A30

    HTTP Response: 200 Success

## GET ALL CONTACTS FOR A CLIENT

    GET /clients/#{client_id}/contacts

    HTTP Response: 200 Success

    <contacts type="array">
      <contact>
        <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
        <title>President</title>
        <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
        <phone-office>555.555.5555</phone-office>
        <id type="integer">9</id>
        <client-id type="integer">11072</client-id>
        <fax>555.555.9999</fax>
        <last-name>Doe</last-name>
        <email>Jane@Doe.com</email>
        <first-name>Jane</first-name>
        <phone-mobile>555.555.7777</phone-mobile>
      </contact>
    </contacts>

You can also filter by updated_since. To show only the contacts that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

    GET /clients/#{client_id}/contacts?updated_since=2010-09-25+18%3A30

    HTTP Response: 200 Success

## GET A CLIENT CONTACT

    GET /contacts/#{contact_id}

    HTTP Response: 200 Success

    <contact>
      <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
      <title>President</title>
      <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
      <phone-office>555.555.5555</phone-office>
      <id type="integer">9</id>
      <client-id type="integer">11072</client-id>
      <fax>555.555.9999</fax>
      <last-name>Doe</last-name>
      <email>Jane@Doe.com</email>
      <first-name>Jane</first-name>
      <phone-mobile>555.555.7777</phone-mobile>
    </contact>

## CREATE A NEW CLIENT CONTACT

    POST /contacts

    HTTP Response: 201 Created 
    Location: /contacts/#{new_contact_id}

You need to post the following:

    <contact>
      <client-id type="integer">9</client-id>
      <email>Jane@Doe.com</email>
      <first-name>Jane</first-name>
      <last-name>Doe</last-name>
      <phone-office>555.555.5555</phone-office>
      <phone-mobile>555.555.7777</phone-mobile>
      <title>President</title>
      <fax>555.555.9999</fax>
    </contact>

Note: Only client-id, first-name and last-name are required.

## UPDATE CLIENT CONTACT

    PUT /contacts/#{contact_id}

    HTTP Response: 200 OK 
    Location: /contacts/#{contact_id}

You can update selected attributes for a client contact.

    <contact>
      <client-id type="integer">9</client-id>
      <email>Jane@JaneDoe.com</email>
      <first-name>Jane</first-name>
      <last-name>Doe</last-name>
      <phone-office>555.555.1111</phone-office>
      <phone-mobile>555.555.2222</phone-mobile>
      <fax>555.555.3333</fax>
    </contact>

## DELETE A CLIENT CONTACT

    DELETE /contacts/#{contact_id}

    HTTP Response: 200 OK
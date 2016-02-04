# This documentation is out of date. Please see our new API docs [here!](http://help.getharvest.com/api)

## People

## SHOW A USER

GET `/people/#{user_id}`

HTTP Response: 200 Success

```xml
<user>
  <id type="integer">54234</id>
  <email>janedoe@example.com</email>
  <first-name>Jane</first-name>
  <last-name>Doe</last-name>
    <!-- If true the user will be added to all new projects automatically -->
  <has-access-to-all-future-projects type="boolean">
    false
  </has-access-to-all-future-projects>
    <!-- If present the user will bill out with this value on all future projects she is added to. To change existing rates You'll have to change the UserAssignment object -->
  <default-hourly-rate type="decimal">100</default-hourly-rate>
    <!-- If true the user will be able to login and record time entries -->
  <is-active type="boolean">true</is-active>
  <is-admin type="boolean">true</is-admin>
  <is-contractor type="boolean">true</is-contractor>
  <telephone></telephone>
  <department></department>
  <timezone>Eastern Time (US & Canada)</timezone>
  <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
  <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
</user>
```

Note: You may also pass a user's email address in place of a user_id.

## SHOW ALL USERS

GET `/people`

HTTP Response: 200 Success

```xml
<users>
  <user>
    <id type="integer">54234</id>
    <email>janedoe@example.com</email>
    <first-name>Jane</first-name>
    <last-name>Doe</last-name>
      <!-- If true the user will be added to all new projects automatically -->
    <has-access-to-all-future-projects type="boolean">
      false
    </has-access-to-all-future-projects>
      <!-- If present the user will bill out with this value on all future projects she is added to. To change existing rates you'll have to change the UserAssignment object -->
    <default-hourly-rate type="decimal">100</default-hourly-rate>
      <!-- If true the user will be able to login and record time entries -->
    <is-active type="boolean">true</is-active>
    <is-admin type="boolean">true</is-admin>
    <is-contractor type="boolean">true</is-contractor>
    <telephone></telephone>
    <department></department>
    <timezone>Eastern Time (US & Canada)</timezone>
    <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
    <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
   </user>
</users>
```

You can filter by updated_since. To show only the people that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

GET `/people?updated_since=2010-09-25+18%3A30`

HTTP Response: 200 Success

## CREATE NEW USER

POST `/people`

HTTP Response: 201 Created

Location: /people/#{new_user_id}

Sample post:

```xml
<user>
  <first-name>Edgar</first-name>
  <last-name>Ruth</last-name>
  <email>edgarruth@example.com</email>
  <timezone>Central Time (US &amp; Canada)</timezone>
  <is-admin type="boolean">false</is-admin>
  <telephone>444-444</telephone>
</user>
```

Upon creation, an email will be sent to the new user with instructions for setting a password.

Note: we accept only a limited set of values for the timezone attribute.

## UPDATE USER

PUT `/people/#{user_id}`

HTTP Response: 200 OK

Location: /people/#{user_id}

You can update selected attributes for a user. Note that updates to password are disregarded.

```xml
<user>
  <first_name>John</first-name>
  <last_name>Doe</last-name>
  <email>johndoe@example.com</email>
  <timezone>Central Time (US & Canada)</timezone>
  <is_admin type="boolean">true</is-admin>
  <telephone>212-555-1212</telephone>
</user>
```

## DELETE EXISTING USER

DELETE `/people/#{user_id}`

HTTP Response: 200 OK

Returned if user does not have any hours logged. Otherwise the user is not removable and HTTP Response: 400 Bad Request is returned. You can still archive the user to disable the user's login.

## TOGGLE AN EXISTING USER

POST `/people/#{user_id}/toggle`

This will archive an active user or activate an archived user. Note that activation is not performed if it results in more users allowed than the plan limit. Response is HTTP Response: 400 Bad Request or in case of success HTTP Response: 200 OK

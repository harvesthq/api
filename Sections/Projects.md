# This documentation is out of date. Please see our new API docs [here!](http://help.getharvest.com/api)

## Projects

## SHOW A PROJECT

GET `/projects/#{project_id}`

HTTP Response: 200 Success

```xml
<project>
  <id type="integer">1</id>
  <client-id type="integer">2</client-id>
  <name>SuprGlu</name>
  <code>HA-0001</code>
  <active type="boolean">true</active>
  <billable type="boolean">false</billable>
  <!-- Shows if the project is billed by task hourly rate or
       person hourly rate. Options: Tasks, People, none -->
  <bill-by>none</bill-by>
  <hourly-rate nil="true"/>
  <!-- Shows if the budget provided by total project hours,
       total project cost, by tasks, by people or none provided.
       Options: project, project_cost, task, person, none -->
  <budget-by>project</budget-by>
  <budget type="decimal">40.0</budget>
  <notify-when-over-budget type="boolean">true</notify-when-over-budget>
  <over-budget-notification-percentage type="decimal">80.0</over-budget-notification-percentage>
  <over-budget-notified-at nil="true"/>
  <show-budget-to-all type="boolean">true</show-budget-to-all>
  <created-at type="datetime">2015-04-09T12:07:56Z</created-at>
  <updated-at type="datetime">2015-04-09T12:07:56Z</updated-at>
  <starts-on type="date">2015-03-09</starts-on>
  <ends-on type="date">2015-07-09</ends-on>
  <!-- These are hints to when the earliest and latest date when a
       timesheet record or an expense was created for a project. Note
       that these fields are only updated once every 24 hours, they
       are useful to constructing a full project timeline. -->
  <hint-earliest-record-at type="date">2015-03-04</hint-earliest-record-at>
  <hint-latest-record-at type="date">2015-04-09</hint-latest-record-at>
  <notes>Project notes go here.</notes>
  <cost-budget type="decimal">1000.0</cost-budget>
  <cost-budget-include-expenses type="boolean">false</cost-budget-include-expenses>
</project>
```

## SHOW ALL PROJECTS

GET `/projects`

HTTP Response: 200 Success

```xml
<projects>
  <project>
    <id type="integer">1</id>
    <client-id type="integer">2</client-id>
    <name>SuprGlu</name>
    <code>HA-0001</code>
    <active type="boolean">true</active>
    <billable type="boolean">false</billable>
    <!-- Shows if the project is billed by task hourly rate or
         person hourly rate. Options: Tasks, People, none -->
    <bill-by>none</bill-by>
    <hourly-rate nil="true"/>
    <!-- Shows if the budget provided by total project hours,
         total project cost, by tasks, by people or none provided.
         Options: project, project_cost, task, person, none -->
    <budget-by>project</budget-by>
    <budget type="decimal">40.0</budget>
    <notify-when-over-budget type="boolean">true</notify-when-over-budget>
    <over-budget-notification-percentage type="decimal">80.0</over-budget-notification-percentage>
    <over-budget-notified-at nil="true"/>
    <show-budget-to-all type="boolean">true</show-budget-to-all>
    <created-at type="datetime">2015-04-09T12:07:56Z</created-at>
    <updated-at type="datetime">2015-04-09T12:07:56Z</updated-at>
    <starts-on type="date">2015-03-09</starts-on>
    <ends-on type="date">2015-07-09</ends-on>
    <!-- These are hints to when the earliest and latest date when a
         timesheet record or an expense was created for a project. Note
         that these fields are only updated once every 24 hours, they
         are useful to constructing a full project timeline. -->
    <hint-earliest-record-at type="date">2014-01-04</hint-earliest-record-at>
    <hint-latest-record-at type="date">2014-06-06</hint-latest-record-at>
    <notes>Project notes go here.</notes>
    <cost-budget type="decimal">1000.0</cost-budget>
    <cost-budget-include-expenses type="boolean">false</cost-budget-include-expenses>
  </project>
  <project>
    ...
  </project>
</projects>
```

You can filter by client_id and updated_since. For example to show only the projects belonging to client with the id 23445.

GET `/projects?client=23445`

HTTP Response: 200 Success

To show only the projects that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

GET `/projects?updated_since=2010-09-25+18%3A30`

HTTP Response: 200 Success

## CREATE NEW PROJECT

POST `/projects`

HTTP Response: 201 Created

Location: /projects/#{new_project_id}

You will have to post the following:

```xml
<project>
   <name>SuprGlu</name>
   <active type="boolean">true</active>
   <bill-by>none</bill-by>
   <!-- Valid and existing client id -->
   <client-id type="integer">2</client-id>
   <code></code>
   <notes></notes>
   <budget type="decimal"></budget>
   <budget-by>none</budget-by>
</project>
```

An error is returned if you've hit the limit in number of projects for the account's plan.

## UPDATE EXISTING PROJECT

PUT `/projects/#{project_id}`

HTTP Response: 200 OK

Location: /projects/#{project_id}

Post similar XML as with create a new project, but include client-id as part of the project. For activating a project a separate method needs to be used.

## (DE)ACTIVATE AN EXISTING PROJECT

PUT `/projects/#{project_id}/toggle`

HTTP Response: 200 Success

Location: /projects/#{project_id}

Note that if your account goes over the allowed project limit, Harvest will return HTTP Response: 400 Bad Request, with a Hint header.

## DELETE A PROJECT

DELETE `/projects/#{project_id}`

If the project does not have any associated hours, it is deleted with HTTP Response: 200 OK. If the project has hours, the project is not deleted and HTTP Response: 400 Bad Request is returned.

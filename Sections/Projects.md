# Projects

## SHOW A PROJECT

GET `/projects/#{project_id}`

HTTP Response: 200 Success

```xml
<project>
  <name>SuprGlu</name>
  <over-budget-notified-at type="date" nil="true"></over-budget-notified-at>
  <billable type="boolean">false</billable>
  <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
    <!-- True if hours can be recorded against this project. False if project is archived/inactive -->
  <active type="boolean">true</active>
    <!-- Shows if the project is billed by task hourly rate or
   person hourly rate. Options: Tasks, People, none -->
  <bill-by>none</bill-by>
  <client-id type="integer">2</client-id>
    <!-- Optional project code -->
  <code></code>
  <notes></notes>
    <!-- Shows if the budget provided by total project hours, total project cost, by tasks, by people or none provided. Options: project, project_cost, task, person, none -->
  <budget-by>none</budget-by>
    <!-- Optional total budget in hours -->
  <budget type="decimal"></budget>
    <!-- These are hints to when the earliest and latest date when a timesheet record or an expense was created for a project. Note that these fields are only updated once every 24 hours, they are usuful to constructing a full project timeline. -->
  <hint-latest-record-at type="date">2007-06-06</hint-latest-record-at>
  <hint-earliest-record-at type="date">2006-01-04</hint-earliest-record-at>
    <!-- FOR FUTURE USE -->
  <fees></fees>
  <id type="integer">1</id>
  <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
</project>
```

## SHOW ALL PROJECTS

GET `/projects`

HTTP Response: 200 Success

```xml
<projects>
  <project>
    <id type="integer">1</id>
    <name>SuprGlu</name>
      <!-- True if hours can be recorded against this project. False if project is archived/inactive -->
    <active type="boolean">true</active>
    <billable type="boolean">false</billable>
      <!-- Shows if the project is billed by task hourly rate or person hourly rate. Options: Tasks, People, Project, none -->
    <bill-by>none</bill-by>
      <!-- The hourly rate for the project, when bill-by is set to "Project" -->
    <hourly-rate type="decimal">150.0</hourly-rate>
    <client-id type="integer">2</client-id>
      <!-- Optional project code -->
    <code></code>
    <notes></notes>
      <!-- Shows if the budget provided by total project hours, total project cost, by tasks, by people or none provided. Options: project, project_cost, task, person, none -->
    <budget-by>none</budget-by>
      <!-- Optional total budget in hours -->
    <budget type="decimal"></budget>
      <!-- These are hints to when the earliest and latest date when a timesheet record or an expense was created for a project. Note that these fields are only updated once every 24 hours, they are usuful to constructing a full project timeline. -->
    <hint-latest-record-at type="date">2007-06-06</hint-latest-record-at>
    <hint-earliest-record-at type="date">2006-01-04</hint-earliest-record-at>
      <!-- FOR FUTURE USE -->
    <fees></fees>
    <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
    <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
  </project>
  <project>
    ...
  </project>
</projects>
```

You can filter by client or any number of other properties. For example to show only the projects belonging to client with the id 23445

GET `/projects?client=23445`

HTTP Response: 200 Success

You can also filter by updated_since. To show only the projects that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

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
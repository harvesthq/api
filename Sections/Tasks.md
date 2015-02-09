# Tasks

## SHOW ONE TASK

GET `/tasks/#{task_id}`

HTTP Response: 200 Success

```xml
<task>
    <!-- If true task will be added as billable upon assigning it to a project -->
  <billable-by-default type="boolean">true</billable-by-default>
    <!-- False if hours can be recorded against this task.  True if task is archived -->
  <deactivated type="boolean">false</deactivated>
  <default-hourly-rate type="decimal">120</default-hourly-rate>
  <id type="integer">1</id>
    <!-- If true task is added to new projects by default -->
  <is-default type="boolean">true</is-default>
  <name>UI Design</name>
  <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
  <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
</task>
```

## SHOW ALL TASKS

GET `/tasks`

HTTP Response: 200 Success

```xml
<tasks>
  <task>
      <!-- If true task will be added as billable upon assigning it to a project -->
    <billable-by-default type="boolean">true</billable-by-default>
    <deactivated type="boolean">false</deactivated>
    <default-hourly-rate type="decimal">120</default-hourly-rate>
    <id type="integer">1</id>
      <!-- If true task is added to new projects by default -->
    <is-default type="boolean">true</is-default>
    <name>UI Design</name>
    <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
    <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
  </task>
  <task>
     ...
  </task>
  ...
</tasks>
```

You can filter by updated_since. To show only the tasks that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

GET `/tasks?updated_since=2010-09-25+18%3A30`

HTTP Response: 200 Success

## CREATE NEW TASK

POST `/tasks`

HTTP Response: 201 Created

Location: /tasks/#{new_task_id} Sample post:

```xml
<task>
  <billable-by-default type="boolean">false</billable-by-default>
  <default-hourly-rate type="decimal">100</default-hourly-rate>
  <is-default type="boolean">false</is-default>
  <name>Server Administration</name>
</task>
```

## ARCHIVE OR DELETE EXISTING TASK

DELETE `/tasks/#{task_id}`

HTTP Response: 200 OK

Returned if task does not have any hours associated - task will be deleted.

Returned if task is not removable - task will be archived.

## UPDATE AN EXISTING TASK

PUT `/tasks/#{task_id}`

HTTP Response: 200 OK

Location: /tasks/#{task_id}

Sample post:

```xml
<task>
  <billable-by-default type="boolean">true</billable-by-default>
  <default-hourly-rate type="decimal">150</default-hourly-rate>
  <is-default type="boolean">true</is-default>
  <name>Flash design</name>
</task>
```

## ACTIVATE EXISTING ARCHIVED TASK

POST `/tasks/#{task_id}/activate`

HTTP Response: 200 OK

Location: /tasks/#{task_id}

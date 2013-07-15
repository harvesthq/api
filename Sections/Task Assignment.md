# Task Assignment: Assigning tasks to projects

## GET ALL TASKS ASSIGNED TO A GIVEN PROJECT

GET `/projects/#{project_id}/task_assignments`

HTTP Response: 200 OK

```xml
<task-assignments>
  <task-assignment>
    <id type="integer">23</id>
    <project-id type="integer">3</project-id>
    <task-id type="integer">2878</task-id>
      <!-- True if task is billable for this project -->
    <billable type="boolean">true</billable>
      <!-- True if task was deactivated for project, preventing further hours to be logged against it -->
    <deactivated type="boolean">true</deactivated>
      <!-- The budget (if present) for the task in project -->
    <budget type="decimal">80</budget>
      <!-- The hourly rate (if present) for the task in project -->
    <hourly-rate type="decimal">80</hourly-rate>
    <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
    <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
  </task-assignment>
  <task-assignment>
    ...
  </task-assignment>
</task-assignments>
```

You filter by updated_since. To show only the task assignments that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

GET `/projects/#{project_id}/task_assignments?updated_since=2010-09-25+18%3A30`

HTTP Response: 200 Success

## GET ONE TASK ASSIGNMENT

GET `/projects/#{project_id}/task_assignments/#{task_assignment_id}`

HTTP Response: 200 OK

```xml
<task-assignment>
  <id type="integer">23</id>
  <project-id type="integer">3</project-id>
  <task-id type="integer">2878</task-id>
    <!-- True if task is billable for this project -->
  <billable type="boolean">true</billable>
    <!-- True if task was deactivated for project, preventing further hours to be logged against it -->
  <deactivated type="boolean">true</deactivated>
    <!-- The budget (if present) for the task in project -->
  <budget type="decimal">80</budget>
    <!-- The hourly rate (if present) for the task in project -->
  <hourly-rate type="decimal">80</hourly-rate>
  <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
  <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
</task-assignment>
```

## ASSIGN A TASK TO A PROJECT

POST `/projects/#{project_id}/task_assignments`

HTTP Response: 201 Created
Location: /projects/#{project_id}/task_assignments/#{new_task_assignment_id}

You will have to post the following:

```xml
<task>
  <id type="integer">653425</id>
</task>
```

## CREATE A NEW TASK AND ASSIGN IT TO A PROJECT

POST `/projects/#{project_id}/task_assignments/add_with_create_new_task`

HTTP Response: 201 Created

Location: /projects/#{project_id}/task_assignments/#{new_task_assignment_id}

You will have to post the following:

```xml
<task>
  <name>Backend Development</name>
</task>
```

## REMOVING A TASK FROM A PROJECT

DELETE `/projects/#{project_id}/task_assignments/#{task_assignment_id}`

HTTP Response: 200 OK

Note that Harvest requires one task per project.

Harvest will not remove the task if it has recorded hours against the project. Instead we will archive the assignment. Harvest will reply with a custom Hint header if archiving occurred.

## CHANGING A TASK FOR A PROJECT

PUT `/projects/#{project_id}/task_assignments/#{task_assignment_id}`

HTTP Response: 200 Success

You will have to post the following:

```xml
<task-assignment>
  <billable type="boolean">true</billable>
  <deactivated type="boolean">false</deactivated>
  <budget type="decimal">3234</budget>
  <hourly-rate type="decimal">100</hourly-rate>
</task-assignment>
```
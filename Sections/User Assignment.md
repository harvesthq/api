# User Assignment: Assigning people to projects

## GET ALL USERS ASSIGNED TO A GIVEN PROJECT

    GET `/projects/#{project_id}/user_assignments`

    HTTP Response: 200 OK

```xml
    <user-assignments>
      <user-assignment>
        <id type="integer">3</id>
        <user-id type="integer">1</user-id>
        <project-id type="integer">3</project-id>
          <!-- If true, user cannot log more hours toward the project -->
        <deactivated type="boolean">false</deactivated>
          <!-- Hourly rate of user on current project -->
        <hourly-rate type="decimal">100.0</hourly-rate>
          <!-- If true, user has project manager permission on this project -->
        <is-project-manager type="boolean">true</is-project-manager>
        <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
        <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
      </user-assignment>
      <user-assignment>
        ...
      </user-assignment>
    </user-assignments>
```

You can filter by updated_since. To show only the user assignments that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

    GET `/projects/#{project_id}/user_assignments?updated_since=2010-09-25+18%3A30`

    HTTP Response: 200 Success

## GET A USER ASSIGNMENT

    GET `/projects/#{project_id}/user_assignments/#{user_assignment_id}`

    HTTP Response: 200 OK

```xml
    <user-assignment>
      <id type="integer">3</id>
      <user-id type="integer">1</user-id>
      <project-id type="integer">3</project-id>
        <!-- If true, user cannot log more hours toward the project -->
      <deactivated type="boolean">false</deactivated>
        <!-- Hourly rate of user on current project -->
      <hourly-rate type="decimal">100.0</hourly-rate>
        <!-- If true, user has project manager permission on this project -->
      <is-project-manager type="boolean">true</is-project-manager>
      <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
      <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
    </user-assignment>
```

## ASSIGN A USER TO A PROJECT

    POST `/projects/#{project_id}/user_assignments`

    HTTP Response: 201 Created
    Location: /projects/#{project_id}/user_assignments/#{new_user_assignment_id}

You will have to post the following:

```xml
    <user>
     <id type="integer">23445<id>
    <user>
```

## REMOVING A USER FROM A PROJECT

    DELETE `/projects/#{project_id}/user_assignments/#{user_assignment_id}`

    HTTP Response: 200 OK

Note that if the user has recorded hours against the project Harvest will archive the assignment record instead of deleting it. From the end result there is no difference between archiving or deletion, but if you need to make a difference Harvest will reply with a custom Hint header if deletion did not occur.

## CHANGING A USER INSIDE A PROJECT

    PUT `/projects/#{project_id}/user_assignments/#{user_assignment_id}`

    HTTP Response: 200 OK
    Location: /projects/#{project_id}/user_assignments/#{new_user_assignment_id}

You will have to post the following:

```xml
    <user-assignment>
      <user-id type="integer">1423</user-id>
      <project-id type="integer">3423</project-id>
      <deactivated type="boolean">true</deactivated>
      <hourly-rate type="decimal">95.0</hourly-rate>
      <is-project-manager type="boolean">false</is-project-manager>
    </user-assignment>
```
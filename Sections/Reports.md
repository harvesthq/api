# Reporting

Harvest provides a powerful reporting interface along with CSV and QuickBooks export. If you need to get raw data in a scriptable manner, Harvest provides direct XML export through the Reporting API. You can import XML data into other systems or create your own custom reporting interfaces on top of your Harvest account's data.

The reporting API endpoints described here are only available to administrator users.

## GET ALL TIME ENTRIES LOGGED TO A PROJECT FOR A GIVEN TIMEFRAME

    GET `/projects/#{project_id}/entries?from=YYYYMMDD&to=YYYYMMDD`

    HTTP Response: 200 Success

```xml
    <day-entries>
      <day-entry>
        <hours type="decimal">0.3</hours>
        <id type="integer">755730</id>
        <notes>Follow up with Danny</notes>
        <project-id type="integer">4234</project-id>
        <spent-at type="date">2007-11-15</spent-at>
        <task-id type="integer">1</task-id>
        <user-id type="integer">5</user-id>
          <!-- was this record invoiced, or marked as invoiced -->
        <is-billed type="boolean">false</is-billed>
          <!-- was this record approved or not (for accounts with approval feature) -->
        <is-closed type="boolean">false</is-closed>
        <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
        <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
      </day-entry>
      <day-entry>
        ...
      </day-entry>
      ...
    </day-entries>
```

This call requires parameters (from and to) to describe the timeframe to use for a report.

## GET ALL TIME ENTRIES BY THE CURRENT USER LOGGED TO A PROJECT FOR A GIVEN TIMEFRAME

    GET `/projects/#{project_id}/entries?from=YYYYMMDD&to=YYYYMMDD&user_id=#{user_id}`

This call requires parameters (from, to) to describe the timeframe to use for a report. This method can be used to view your own entries on a specific project.

## FILTERING

    GET `/projects/#{project_id}/entries?from=YYYYMMDD&to=YYYYMMDD&billable=yes`

Acceptable values for the billable parameter are "yes" and "no".

    GET `/projects/#{project_id}/entries?from=YYYYMMDD&to=YYYYMMDD&only_billed=yes`

Acceptable value for the only_billed parameter is "yes". Anything else will be ignored.

    GET `/projects/#{project_id}/entries?from=YYYYMMDD&to=YYYYMMDD&only_unbilled=yes`

Acceptable value for the only_unbilled parameter is "yes". Anything else will be ignored.

    GET `/projects/#{project_id}/entries?from=YYYYMMDD&to=YYYYMMDD&is_closed=no`

Acceptable values for the is_closed parameter are "yes" and "no".

    GET `/projects/#{project_id}/entries?from=YYYYMMDD&to=YYYYMMDD&updated_since=2010-09-25+18%3A30`

Acceptable value for the updated_since parameter is a UTC date time value, URL encoded.

    GET `/projects/#{project_id}/entries?user_id=1334`

## GET ALL TIME ENTRIES LOGGED BY A USER FOR A GIVEN TIMEFRAME

    GET `/people/#{user_id}/entries?from=YYYYMMDD&to=YYYYMMDD`

    HTTP Response: 200 Success

```xml
    <day-entries>
      <day-entry>
        <hours type="decimal">0.2</hours>
        <id type="integer">755689</id>
        <notes></notes>
        <project-id type="integer">2</project-id>
        <spent-at type="date">2007-11-19</spent-at>
        <task-id type="integer">2</task-id>
        <user-id type="integer">1</user-id>
          <!-- was this record invoiced, or marked as invoiced -->
        <is-billed type="boolean">false</is-billed>
          <!-- was this record approved or not (for accounts with approval feature) -->
        <is-closed type="boolean">false</is-closed>
        <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
        <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
      </day-entry>
      <day-entry>
       ...
      </day-entry>
      ...
    </day-entries>
```

This call requires parameters (from and to) to describe the timeframe to use for a report.

## GET ALL YOUR TIME ENTRIES FROM A PROJECT AND A GIVEN TIMEFRAME

    GET `/people/#{user_id}/entries?from=YYYYMMDD&to=YYYYMMDD&project_id=#{project_id}`

    HTTP Response: 200 Success

This call requires parameters (from, to and project_id) and it will return results .

## FILTERING

    GET `/people/#{user_id}/entries?from=YYYYMMDD&to=YYYYMMDD&billable=yes`

Acceptable values for the billable parameter are "yes" and "no".

    GET `/people/#{user_id}/entries?from=YYYYMMDD&to=YYYYMMDD&only_billed=yes`

Acceptable value for the only_billed parameter is "yes". Anything else will be ignored.

    GET `/people/#{user_id}/entries?from=YYYYMMDD&to=YYYYMMDD&only_unbilled=yes`

Acceptable value for the only_unbilled parameter is "yes". Anything else will be ignored.

    GET `/people/#{user_id}/entries?from=YYYYMMDD&to=YYYYMMDD&is_closed=no`

Acceptable values for the is_closed parameter are "yes" and "no".

    GET `/people/#{user_id}/entries?from=YYYYMMDD&to=YYYYMMDD&updated_since=2010-09-25+18%3A30`

Acceptable value for the updated_since parameter is a UTC date time value, URL encoded.

## GET ALL EXPENSE ENTRIES LOGGED BY A USER FOR A GIVEN TIMEFRAME

    GET `/people/#{user_id}/expenses?from=YYYYMMDD&to=YYYYMMDD`

    HTTP Response: 200 Success

```xml
    <expenses>
      <expense>
        <expense-category-id type="integer">50543</expense-category-id>
        <id type="integer">11</id>
        <notes>Drive to pick up office supplies</notes>
        <project-id type="integer">19034</project-id>
        <spent-at type="date">2008-01-23</spent-at>
        <total-cost type="decimal">4.85</total-cost>
        <units type="decimal">10.0</units>
        <user-id type="integer">12334</user-id>
          <!-- was this record approved or not (for accounts with approval feature) -->
        <is-closed type="boolean">false</is-closed>
        <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
        <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
      </expense>
      <expense>
       ...
      </expense>
      ...
    </expenses>
```

This call requires parameters (from and to) to describe the timeframe to use for a report.

You can filter by is_closed status, for example to show only the expenses that have not yet been approved by an administrator. Acceptable values are "yes" and "no".

    GET `/projects/#{project_id}/expenses?from=YYYYMMDD&to=YYYYMMDD?is_closed=no`

    HTTP Response: 200 Success

You can also filter by updated_since. To show only the expenses that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

    GET `/projects/#{project_id}/expenses?from=YYYYMMDD&to=YYYYMMDD?updated_since=2010-09-25+18%3A30`

    HTTP Response: 200 Success

## GET ALL EXPENSE ENTRIES LOGGED TO A PROJECT FOR A GIVEN TIMEFRAME

    GET `/projects/#{project_id}/expenses?from=YYYYMMDD&to=YYYYMMDD`

    HTTP Response: 200 Success

```xml
    <expenses>
      <expense>
        <expense-category-id type="integer">50543</expense-category-id>
        <id type="integer">11</id>
        <notes>Drive to pick up office supplies</notes>
        <project-id type="integer">19034</project-id>
        <spent-at type="date">2008-01-23</spent-at>
        <total-cost type="decimal">4.85</total-cost>
        <units type="decimal">10.0</units>
        <user-id type="integer">12334</user-id>
          <!-- was this record approved or not (for accounts with approval feature) -->
        <is-closed type="boolean">false</is-closed>
        <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
        <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
      </expense>
      <expense>
       ...
      </expense>
      ...
    </expenses>
```

This call requires parameters (from and to) to describe the timeframe to use for a report.

You can filter by is_closed, only_billed or only_unbilled status, for example to show only the expenses that have not yet been approved by an administrator. Acceptable values are "yes" and "no".

    GET `/projects/#{project_id}/expenses?from=YYYYMMDD&to=YYYYMMDD?is_closed=no`

    HTTP Response: 200 Success

    GET `/projects/#{project_id}/expenses?from=YYYYMMDD&to=YYYYMMDD&only_billed=yes`

    HTTP Response: 200 Success

Acceptable value for the only_billed parameter is "yes". Anything else will be ignored.

    GET `/projects/#{project_id}/expenses?from=YYYYMMDD&to=YYYYMMDD&only_unbilled=yes`

    HTTP Response: 200 Success

Acceptable value for the only_unbilled parameter is "yes". Anything else will be ignored.

You can also filter by updated_since. To show only the expenses that have been updated since "2010-09-25 18:30", pass the UTC date time value (URL encoded).

    GET `/projects/#{project_id}/expenses?from=YYYYMMDD&to=YYYYMMDD?updated_since=2010-09-25+18%3A30`

    HTTP Response: 200 Success

In conjunction with the reporting calls, you may wish to retrieve all users, projects, expense categories and tasks to convert numeric IDs to users, projects, tasks, and expense categories.
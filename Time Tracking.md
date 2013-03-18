# Time Tracking API

The Time tracking API allows you to access and manipulate time entries in similar fashion to using the daily timesheet view. This allows developers to create lightweight clients or widgets to track time beyond directly interacting with Harvest through the web browser.

## RETRIEVING ENTRIES AND PROJECTS/TASKS FOR A DAY

GET /daily

Retrieves entries for the today paired with the projects and tasks that can be added to the timesheet by the requesting user.

GET /daily/#{day_of_the_year}/#{year}

Optionally you may ask for entries for a given day by passing in date composed by the year and the day of the year (1..366).

HTTP Response: 200 Success
```xml
    <daily>
      <for_day type="date">Wed, 18 Oct 2006</for_day>
      <day_entries>
        <day_entry>
          <id type="integer">195168</id>
          <spent_at type="date">2006-10-18</spent_at>
          <user_id type="integer">218356</user_id>
          <client>Iridesco</client>
          <project_id>1234567</project_id>
          <project>Harvest</project>
          <task_id>126789</task_id>
          <task>Backend Programming</task>
          <!-- Includes running timer if any -->
          <hours type="float">2.06</hours>
          <notes>Test api support</notes>
          <!-- OPTIONAL returned only if a timer is running -->
          <timer_started_at type="datetime">
            Wed, 18 Oct 2006 09:53:06 -0000
          </timer_started_at>
          <created_at type="datetime">Thu, 17 Nov 2011 10:19:57 +0000</created_at>
          <updated_at type="datetime">Thu, 17 Nov 2011 12:24:03 +0000</updated_at>
          <!-- OPTIONAL if account has timestamps enabled -->
          <started_at>8:00am</started_at>
          <ended_at>10:00am</ended_at>
        </day_entry>
        <day_entry>
          ...
        </day_entry>
      </day_entries>
      <!-- These are the project-task combinations that can be added to the timesheet. Not present in readonly timesheets or for users without assigned projects. -->
      <projects>
        <project>
          <name>Click and Type</name>
          <code></code>
          <id type="integer">3</id>
          <client>AFS</client>
          <tasks>
            <task>
              <name>Security support</name>
              <id type="integer">14</id>
              <billable type="boolean">true</billable>
            </task>
            <task>
              ...
            </task>
          </tasks>
        </project>
        <project>
          ...
        </project>
      </projects>
    </daily>
	```
	
## RETRIEVING A SINGLE ENTRY

    GET /daily/show/#{day_entry_id}

Retrieves the selected entry.

    HTTP Response: 200 Success

    <timer>
      <day_entry>
        <id type="integer">195168</id>
        <spent_at type="date">2006-10-11</spent_at>
        <user_id type="integer">52</user_id>
        <client>Iridesco</client>
        <project_id>123456</project_id>
        <project>Harvest</project>
        <task_id>678901</task_id>
        <task>Backend Programming</task>
        <hours type="float">0.00</hours>
        <notes>Test api support</notes>
        <created_at type="datetime">Thu, 17 Nov 2011 10:19:57 +0000</created_at>
        <updated_at type="datetime">Thu, 17 Nov 2011 12:24:03 +0000</updated_at>
        <!-- OPTIONAL returned only if the timer is running -->
        <timer_started_at type="datetime">
          Wed, 17 Oct 2006 10:45:07 +0000
        </timer_started_at>
        <!-- OPTIONAL returned only if timestamps are enabled -->
        <started_at>8:00am</started_at>
        <ended_at>10:00am</ended_at>
      </day_entry>
    </timer>

## TOGGLING A TIMER

    GET /daily/timer/#{day_entry_id}

Starts and stops a timer for a selected entry.

    HTTP Response: 200 Success

    <timer>
      <!-- day_entry with toggled timer -->
      <day_entry>
        <id type="integer">195168</id>
        <spent_at type="date">2006-10-11</spent_at>
        <user_id type="integer">52</user_id>
        <client>Iridesco</client>
        <project_id>123456</project_id>
        <project>Harvest</project>
        <task_id>678901</task_id>
        <task>Backend Programming</task>
        <hours type="float">2.06</hours>
        <notes>Test api support</notes>
        <created_at type="datetime">Thu, 17 Nov 2011 10:19:57 +0000</created_at>
        <updated_at type="datetime">Thu, 17 Nov 2011 12:24:03 +0000</updated_at>
        <!-- OPTIONAL returned only if timestamps are enabled -->
        <started_at>4:45pm</started_at>
        <ended_at></ended_at>
        <!-- OPTIONAL returned if the timer was started -->
        <timer_started_at type="datetime">
          Wed, 18 Oct 2006 10:45:07 +0000
        </timer_started_at>
      </day_entry>
      <!-- OPTIONAL if a timer for another day_entry was stopped by side effect, returns a confirmed value for the timer. Protects against different clock rates on the client side. -->
      <hours_for_previously_running_timer type="float">
        0.87
      </hours_for_previously_running_timer>
    </timer>

Note: if your account uses timestamp timers, timers cannot be restarted. Instead, a new timer will be created with the same project, task, and notes and be returned.

## CREATING AN ENTRY

    POST /daily/add

Create an entry on the daily screen

    HTTP Response: 201 Created

You need to POST the following:

    <request>
      <notes>Test api support</notes>
      <hours>3</hours>
      <project_id type="integer">3</project_id>
      <task_id type="integer">14</task_id>
      <spent_at type="date">Tue, 17 Oct 2006</spent_at>
    </request>

Note: you can transmit a blank string as hours if you want to start a timer against the new day_entry record. For example: 

    <hours> </hours>

You'll get the following reply:

    <timer>
      <!-- new entry -->
      <day_entry>
        <id type="integer">195168</id>
        <client>Iridesco</client>
        <project>Harvest</project>
        <task>Backend Programming</task>
        <hours>0.00</hours>
        <notes>Test api support</notes>
        <!-- OPTIONAL returned only if a timer was started -->
        <timer_started_at type="datetime">
          Wed, 17 Oct 2006 10:45:07 +0000
        </timer_started_at>
      </day_entry>
      <!-- OPTIONAL returned only if a timer for another -->
      <!--          day_entry was stopped as a result    -->
      <hours_for_previously_running_timer type="float">
        0.87
      </hours_for_previously_running_timer>
    </timer>

If your account uses timestamp timers, you may alternatively POST a started_at and ended_at time:

    <request>
      <notes>Test api support</notes>
      <started_at>8:00am</started_at>
      <ended_at>9:00am</ended_at>
      <project_id type="integer">3</project_id>
      <task_id type="integer">14</task_id>
      <spent_at type="date">Tue, 17 Oct 2006</spent_at>
    </request>

Note: you can transmit blank strings as started_at and ended_at if you want to start a timer against the new day_entry record. For example: <started_at> </started_at>

## DELETING AN ENTRY

    DELETE /daily/delete/#{day_entry_id}

Deletes a day entry.

    HTTP Response: 200 Success

## UPDATING AN ENTRY

    POST /daily/update/#{day_entry_id}

Updates the note, effort, project or task for a day entry. All sensible values are overwritten for the day entry with the data provided in your request.

    HTTP Response: 200 Success

You need to POST the following:

    <request>
      <notes>New notes</notes>
      <hours>1.07</hours>
      <spent_at type="date">Tue, 17 Oct 2006</spent_at>
      <project_id>52234</project_id>
      <task_id>67567</task_id>
    </request>

If your account uses timestamp timers, you may alternatively POST a started_at and ended_at times in lieu of hours.

## WORKING WITH TIMESHEETS FOR OTHER USERS

You may add an of_user=#{user_id} parameter to the URL of any time tracking API call to work with the timesheet of another user. The authenticating user must be an administrator for this to work.

## WORKING WITH LOCKED TIME

Administrators may edit and delete locked time entries. The following fields are considered readonly on locked time entries: project_id, task_id, spent_at.
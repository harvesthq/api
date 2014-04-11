# Account Information

## USER AND ACCOUNT INFORMATION FOR CURRENT USER

GET `account/who_am_i`

HTTP Response: 200 Success

```xml
<hash>
  <company>
    <base-uri>https://subdomain.harvestapp.com</base-uri>
    <full-domain>subdomain.harvestapp.com</full-domain>
    <name>My Company Name</name>
    <active type="boolean">true</active>
    <!-- Sunday, Monday or Saturday -->
    <week-start-day>Monday</week-start-day>
    <!-- decimal or hours_minutes -->
    <time-format>decimal</time-format>
    <!-- 12h or 24h -->
    <clock>12h</clock>
    <!-- period or comma -->
    <decimal-symbol>.</decimal-symbol>
    <!-- Options are orange, spring, green, legacy, behance, blue,
        purple, red, lt-grey, gray -->
    <color-scheme>blue</color-scheme>
    <modules>
      <expenses type="boolean">true</expenses>
      <invoices type="boolean">true</invoices>
      <estimates type="boolean">true</estimates>
      <approval type="boolean">true</approval>
    </modules>
    <!-- comma, period, apostrophe or space -->
    <thousands-separator>,</thousands-separator>
  </company>
  <user>
    <timezone>Pacific Time (US &amp; Canada)</timezone>
    <timezone-utc-offset type="integer">-25200</timezone-utc-offset>
    <id type="integer">123456</id>
    <email>janedoe@example.com</email>
    <admin type="boolean">true</admin>
    <first-name>Jane</first-name>
    <last-name>Doe</last-name>
    <avatar-url>https://subdomain.harvestapp.com/uploads/users/avatar/000/123/123/normal.jpg</avatar-url>
    <project-manager>
      <is-project-manager type="boolean">true</is-project-manager>
      <can-see-rates type="boolean">true</can-see-rates>
      <can-create-projects type="boolean">true</can-create-projects>
      <can-create-invoices type="boolean">true</can-create-invoices>
    </project-manager>
    <!-- true or false (track time using timestamps or duration) -->
    <timestamp-timers type="boolean">false</timestamp-timers>
  </user>
</hash>
```
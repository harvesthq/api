# Introduction to the Harvest API

Harvest provides two API interfaces, serving two distinct roles. If you need to access and manipulate your daily timesheet the [Time Tracking API](https://github.com/harvesthq/api/blob/master/Timesheet%20API/Time%20Tracking.md) fits the bill. Notable uses of the [Time Tracking API](https://github.com/harvesthq/api/blob/master/Timesheet%20API/Time%20Tracking.md) are the widgets we provide for PC and Mac as well as other third party timesheet software integrations.

If you need to access and edit your projects, clients, users and tasks the extended API is your choice. You can use this to mass import your existing projects setup, add users and generally integrate with your existing back-office setup.

Remember to write your application carefully, caching when possible. In case of abuse you may be blocked, disallowing further API access. As an act of courtesy, please provide User-Agent strings denoting your application.

## Sample Code & Examples

To help you get started with the Harvest API, we have provided sample scripts in several programming languages. These scripts depict authentication and basic API actions. Visit the [Harvest API Samples GitHub project](http://github.com/harvesthq/harvest_api_samples) to take a look at the scripts.

You may also want to check out the open-source libraries for the Harvest API below.

## Share Your Creation

If you've built something interesting with the Harvest API or the [Harvest Platform](http://www.getharvest.com/platform), share the love! Created a library or other cool connector another Harvest user might find useful? Add it to our [Community Creations and Hacks page](https://github.com/harvesthq/api/wiki/Community-Creations-&-Hacks), and contribute to the solid list of great projects people have made. If it's a connector between another app and ours, ping us at [support@harvestapp.com](mailto:support@harvestapp.com) — we may be able to list it on our [Add-ons](http://www.getharvest.com/add-ons) page.

## Authorization

All requests to the Harvest API are made on the behalf of an actual user (see the [HTTP Basic Authentication](https://github.com/harvesthq/api/blob/master/Authentication/HTTP%20Basic.md) or [OAuth 2.0 Authentication](https://github.com/harvesthq/api/blob/master/Authentication/OAuth%202.0.md) sections for detail on authenticating your requests). You can use a regular account for requests against the [Time Tracking API](https://github.com/harvesthq/api/blob/master/Timesheet%20API/Time%20Tracking.md), but for private integrations accessing the Extended REST API we recommend creating a special admin user.

Harvest will check your role on each request, and actions that are unavailable to you on the UI will be unavailable over the API as well. Administrators can generally access all API resources, and regular users are limited to their own timesheets. Project Managers can access projects they manage in addition to their own timesheets.

## Supported Data Formats

The Harvest API supports both XML and JSON data formats.

For an XML request, send `application/xml` in the `Accept` and `Content-Type` headers. Send `application/json` for JSON responses. All examples in this documentation assume XML input and output, however JSON output follows similar structure to the XML documented.

## Throttle Limit - HTTP 503

We have an API throttle that blocks accounts emitting more than 100 calls per 15 seconds. We reserve the right to tune the limitations, but they are always set high enough to allow a well-behaving interactive program to do its job.

For batch processes and API developers who still need to perfect their code, this throttle may be an inadvertent blocker. Just wait and make no API calls (the throttle is reset with each call). The throttle will lift itself in few minutes and API calls may resume.

When the rate limit is exceeded Harvest will send an HTTP 503 status code. The number of seconds until the throttle is lifted is sent via the `Retry-After` HTTP header, as specified in [RFC 2616](http://tools.ietf.org/html/rfc2616#section-14.37). You can use `GET /account/rate_limit_status` to programmatically query your current throttle status.

## Notational Conventions

Throughout our documentation you'll find the following set of notational conventions:

* `#{expression}`: Should be substituted with the value of the expression. For example, `/#{project_id}` should be replaced with `/12345` (assuming your `project_id` is 12345)

* `...`: For brevity, we have skipped repetitive parts of the response.

* `<!-- Comment -->`: Optional comment in the response added for clarity. The actual response will not contain comments.

## Harvest API Libraries

A few of our users have implemented their own Harvest API wrappers. If you plan on writing a Harvest API client, you may want to check out some of these excellent projects:

* Ruby: [Harvested](https://github.com/zmoazeni/harvested), by Zach Moazeni
* Python: [Harvest Time Tracking API Client](https://github.com/aurorasoftware/python-harvest), by Aurora Software, LLC
* Python: [Harvest API Wrapper in Python](http://github.com/lann/Harvest), by Lann Martin and Brian Glass
* Java: [Harvest-client](http://github.com/moffermann/harvest-client), by Mauricio Offermann
* PHP: [HaPi - PHP Harvest API](http://labs.mdbitz.com/harvest-api/), by Matthew Denton
* Node.js: [node-harvest](https://github.com/nrub/node-harvest), by Paul English
* Drupal: [Harvest Module for Drupal](http://drupal.org/project/harvest), by ImageX Media

We also have sample scripts in several languages. These scripts depict authentication and basic API actions. Visit the [Harvest API Samples GitHub project](http://github.com/harvesthq/harvest_api_samples) to take a look at the scripts.

## Have More Questions?

Please email us at [support@harvestapp.com](mailto:support@harvestapp.com).

---
layout: post
title: "Ex 3: DB2 reader"
---
## DB2 Reader App

Quality is key, and the more Find property information integrates with existing Land Registry systems and tables on DB2, testing must also reduce the risk of these changes impacting existing services and systems.  While all of our changes are unit and integration tested, our acceptance testing must make sure to test the service as a whole.

Our tests are written in Ruby, and not within a framework like Sinatra or Ruby on Rails.  The issue is gaining access to the DB2 tables from the tests, the only Ruby gem available requires Active Record which we do not use in our testing suite.

We already have DB2 access in existing Python applications, so taking that as an example I designed and developed a lightweight version which could be run and used by our acceptance tests.  The first iteration is in use and resides in the acceptance tests here:

* Please note that this repo is only available on the network or via VPN.

<b>
    <a href="http://192.168.249.38/digital-register-view/fpi-acceptance-tests/tree/develop">FPI Acceptance Tests</a>
</b>

I have implemented it in a slightly unusual way, although it is still designed as a standard web application to keep simplicity.  The DB2 Reader exists as within the test suite; launched locally as part of the initial preparation every time the tests run.  The application is launched in the background and currently has one route which allows the test to interrogate a route that has a specific SQL statement.

We were able to automate testing of the new reset password link functionality fully, allowing us to ensure quality consistently and reducing the need for manual testing in that area. Notably, this improved delivery times as a result.

The more testing that needs to read from DB2, the further the app could expand for the project.  Naturally, it will be monitored and consideration will be given as it gets used in the future, thus allowing me to improve the usability and efficiency of the tool to be used in testing going forward.

I feel that this tool could be implemented in any team to help further automated acceptance testing. A later iteration will make it accessible enough that test team members can input additional SQL statement routes as required.  To better help the adoption of this I will run small workshops.  From previous experience in running successful Git workflow workshops I find that people learn and consolidate this information better by working through real examples.

---
layout: post
title: "Ex 1: Reset Password"
---

<h2>Project: Find property information</h2>

For Find property information we were currently sending out a temporary password via email.  With this, the user would then have to log in with the temporary password and then they would be prompted to change their password.

A user story wanted to change this so that an password reset link would be sent out via email.  The user then would follow this link and they would just change their password and login.

Design and user research had taken place when I took up the story, what needed to be considered next was what technology we were going to use to deliver.  We could continue using our current methods using the likes of ECNOT to send out our emails, or potentially use the GOV.UK Notify which is currently in BETA but useable in LIVE by invited services.

I created an options paper which documented the directions we could take, with the pros and cons of each potential option we had.  This was then submitted to the architect, lead technical designer and team with my recommendation of what option to take.  The pdf below was what was submitted.

<b>[Reset password with link options paper]({{ site.baseurl }}/assets/Resetpasswordwithlinksoption.pdf)</b>

While GOV.UK is being considered a future method/technology, option using pre existing routes through ECNOT was chosen to ensure timely delivery while keeping quality.

Knowing the I was able to fully investigate the impact and changes needed for micro web services used by FPI, mainframe based systems such as ECNOT and DB2.

I determined that changes were needed to our services:

<h3>digital-register-frontend</h3>
 - New routes and pages were needed for changing password, and requesting a password email.  
 - Changes to the information we send to our public-account-service-api.

<h3>esecurity-frontend</h3>
 - Changes to existing links to match the new routes.
 - Amendments to error messages for readability purposes as part of the design.

<h3>public-account-service-api</h3>
 - New routes had to be added, we would need to be able to request a reset password email be sent, but not for an actual reset password action be completed.
 - A token would have to be created and saved in according to option 1 - Postgres.  This would have a time limit and could only be used once for security purposes.
 - Once someone visited the change password link with a token it would have to be validated.

<h3>reconcilliation-client</h3>
 - It would have to send the token through to a DB2 table, which would then, via a queue allocate the needed information to the correct tables in other DB2 tables.

<h3>SQL inserts, amendments and deletions in DB2</h3>
 - Instead of one business event that would write audit and email trigger information to the correct tables, we need two separate events, one which would send audit information and an email trigger, the other would send audit information.
 - Instead of amending ECNOT we just had to amend the email content that we were going to use to send out the new information.  We deleted one part that was no longer used at all.

I took lead on developing the digital-register-frontend and the esecurity-frontend changes which can be found in these repos, I am of course available to talk through the decisions in the code:

* Please note that these two repos are only available on the network of via VPN.

<a href="http://192.168.249.38/digital-register-view/digital-register-frontend">digital-register-frontend</a>

<a href="http://192.168.249.38/digital-register-view/esecurity-frontend">esecurity-frontend</a>

I enlisted a team member to complete the work on public-account-service-api.

The SQL code I put together myself and had it checked by our web ops representative to confirm that it was correct.  I unfortunately cannot show it on this site but will quite happily show this in person.

<h2>Conclusion</h2>

The reset password story has been deployed into our pre prod environment, due to the changes we need to the DB2 tables we need to schedule according to other changes going in.  This should be delivered within a week though.

These changes will be monitored in the feedback and the analytics we have in place to measure how many people use it, and complete the journey.

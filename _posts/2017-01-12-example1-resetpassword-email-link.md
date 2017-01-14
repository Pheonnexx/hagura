---
layout: post
title: "Ex 1: Reset Password"
---

<h2>Project: Find property information</h2>

For Find property information we are currently sending out a temporary password via email.  With this, the user would then have to log in with the temporary password and then they would be prompted to change their password.

A user story wanted to change this so that a password reset link would be sent out via email.  The user would then follow this link and they change their password and login.

Design and user research had taken place when I took up the story, what needed to be considered next was what technology we were going to use to deliver the solution.  We could continue to employ our current methods using the likes of ECNOT, an internal application, to send out our emails, or potentially use the GOV.UK Notify service which is currently in beta but useable in live by invited services.

To this end, I created an options paper which documented the directions we could take, with the pros and cons of each potential option we had.  This was then submitted to the architect, lead technical designer and team with my recommendation of what option to take.  The pdf below was what was submitted to these stakeholders.

<b>[Reset password with link options paper]({{ site.baseurl }}/assets/Resetpasswordwithlinksoption.pdf)</b>

While the GOV.UK Notify service is being considered as a future technology, option 1 from the aforementioned paper using pre existing routes through ECNOT was chosen to allow for timely delivery while ensuring quality.

With this plan of action I was able to fully investigate the impact and changes needed for the micro web services used by FPI and mainframe based systems such as ECNOT and DB2.

I determined that changes were needed to these services:

<h3>digital-register-frontend</h3>
 - New routes and pages were needed for changing passwords, and requesting password reset emails.  
 - Changes to the information we send to our public-account-service-api.

<h3>esecurity-frontend</h3>
 - Changes to existing links to match the new routes.
 - Amendments to error messages for readability purposes as part of the design.

<h3>public-account-service-api</h3>
 - New routes had to be added.  We would need to be able to request a reset password email be sent, but not for an actual reset password action to be completed.
 - A token would have to be created and saved in accordance to option 1 - in a Postgres Database.  This would have a time limit and could only be used once for security purposes.
 - Once someone visited the change password link with a token it would have to be validated.
 - Once someone has completed the action of changing their password, this would then have to be enacted.

<h3>reconcilliation-client</h3>
 - The client would have to send the token through to a DB2 table, which would via a queue allocate the needed information to the correct tables in other DB2 tables.

<h3>SQL inserts, amendments and deletions in DB2</h3>
 - Instead of one business event that would write audit and email trigger information to the correct tables, we need two separate events.  One which would send audit information and an email trigger, while the other would send audit information.
 - Instead of changing ECNOT we just had to amend the email content that we were going to use to send out the new information.  We deleted one part that was no longer used at all.

I took lead on developing the digital-register-frontend and the esecurity-frontend changes which can be found in these repos.  I am of course available to talk through all the code changes that have been made:

* Please note that these two repos are only available on the network or via VPN.

<a href="http://192.168.249.38/digital-register-view/digital-register-frontend">digital-register-frontend</a>

<a href="http://192.168.249.38/digital-register-view/esecurity-frontend">esecurity-frontend</a>

Additionally, I enlisted a team member to complete the work on public-account-service-api.

I put the SQL code together myself and had it checked by our web ops representative to confirm that it was correct.  I unfortunately cannot show it on this site but will quite happily demonstrate this in person.

<h2>Conclusion</h2>

The reset password story has been deployed successfully into our pre prod environment, due to the SQL changes to be ran in the DB2 tables we need to schedule according to other changes going in.  This should be delivered within a week.

The success of these changes will be monitored in the feedback and the analytics we have in place to measure how many people use it, and thus complete the journey.

---
layout: post
title: "Ex 1: Reset Password"
---

<h2>Project: Find property information</h2>

For Find property information we are currently sending out a temporary password via email.  With this, the user would then have to log in with the temporary password and then they would be prompted to change their password.

 In order to improve the security of the password reset solution and reduce the risk of hacking, a user story wanted to change the current solution so that a password reset link would be sent out via email.  The user would then follow this link and they change their password and login.

Design and user research had taken place when I took up the story, what needed to be considered next was what technology we were going to use to deliver the solution.  We could continue to employ our current methods using the likes of ECNOT, an internal application, to send out our emails, or potentially use the GOV.UK Notify service which is currently in beta but useable in live by invited services.

To this end, I created an options paper which documented the directions we could take, with the pros and cons of each potential option we had.  This was then submitted to the architect, lead technical designer and team with my recommendation of what option to take.  The pdf below was what was submitted to these stakeholders.

<b>[Reset password with link options paper]({{ site.baseurl }}/assets/Resetpasswordwithlinksoption.pdf)</b>

While the GOV.UK Notify service is being considered as a future technology, option 1 from the aforementioned paper using pre existing routes through ECNOT was chosen to allow for timely delivery while ensuring quality.

With this plan of action I was able to fully investigate the impact and changes needed for the micro web services used by FPI and mainframe based systems such as ECNOT and DB2.

I determined that changes were needed to these services:

<h3>digital-register-frontend</h3>
 - New routes were needed for changing passwords, and requesting password reset emails.  
 - New pages which followed the design were needed for changing passwords, and requesting password reset emails.
 - Changes to the information we send to our public-account-service-api, additional checks were needed to validate the token.

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
 - Instead of changing ECNOT we just had to amend the email content that we were going to use to send out the new information, reducing the impact on other services.  We deleted one part that was no longer used at all.

I took lead on developing the digital-register-frontend and the esecurity-frontend changes which can be found in these repos.  I am of course available to talk through all the code changes that have been made:

* Please note that these two repos are only available on the network or via VPN.

<b>
    <a href="http://192.168.249.38/digital-register-view/digital-register-frontend">digital-register-frontend</a>
</b>

<b>
    <a href="http://192.168.249.38/digital-register-view/esecurity-frontend">esecurity-frontend</a>
</b>

The work on public-account-service-api was completed by a contract team member.  I provided guidance and support on what was needed to be sent to our internal systems, such as business event_id's.

While implementing the solution, I made the decision to validate the token part of the password link as soon as the user used it instead of when they submitted their password change.  This allowed for the user to be informed if the password link was invalid quicker and without wasting their time.  As well as making sure that the public-account-service-api had the ability to do this I then had to implement it in the frontend as shown by the code snippet below.

I also made the password reset page more dynamic, providing the text needed to the HTML, therefore improving the reusability of the template and reducing upkeep.

```Python
@app.route('/change-password/<token>', methods=['GET', 'POST'])
def change_password(token):
    form = ChangePasswordForm()
    if request.method == 'GET':
        # GET request should carry a token; first validate it.
        response = api_client.validate_password_reset_token(token)
        if response and hasattr(response, 'json'):
            if response.json().get('status') is not True:
                print(response.json())
                return reset_password_page(
                    heading="Sorry that link isn't valid",
                    message="Enter your email address - we'll send you a new link")
            else:
                return render_template('account/change_password.html', form=form, token=token)
    elif form.validate_on_submit():
        # submit form with valid token, is validated and submitted and will say whether password
        # succesfully changed
        response = api_client.change_password(token, form.data)
        if response and hasattr(response, 'json'):
            if response.json().get('status') != 'success':
                return render_template(
                    'account/change_password.html',
                    processerrors=_sanitise_error_message(response.json(), 'reset your password'))
            if response.json().get('data'):
                # The password change has happened successfully so shown the succesful page
                return render_template('account/change_password_successful.html')
    return reset_password_page(
        heading="Sorry that link isn't valid",
        message="Enter your email address - we'll send you a new link")


def reset_password_page(heading, message, processerrors=None):
    form = PasswordResetForm()
    return render_template('account/request_reset_password_email.html', form=form, heading=heading, message=message, processerrors=processerrors)
```


I developed the SQL code myself and had it checked by our web ops representative to confirm  correctness.  I unfortunately cannot show it on this site but will quite happily demonstrate this in person.

<h2>Conclusion</h2>

The reset password story has been deployed successfully into our pre prod environment, due to the SQL changes to be ran in the DB2 tables we need to schedule according to other changes going in.  This should be delivered within a week.

The success of these changes will be monitored in the feedback and the analytics we have in place to measure how many people use it, and thus complete the journey.

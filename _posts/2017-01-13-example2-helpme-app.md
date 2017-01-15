---
layout: post
title: "Ex 2: Alexa HelpMe"
---

## Amazon Alexa HelpMe App

As part of developing my skills within software engineering I aim to attend a number of external events.  This allows me to get a feel of new and relevant technology and skills which could help me and the Land Registry develop quality services.

On December 2nd of last year I attended DataPlay, ran by Plymouth City Council.  They release open source data according to the theme of the hackday that is being run, in this case, health related data.

By attending this event, I would be gain practice in understanding and utilising different data, whilst also improving my ability to create prototypes to prove an idea within a short amount of time.

To this end, I authored an article on the experience that can be viewed in the link below.

<b>
    <a href="https://medium.com/@Pheonnexx/data-play-helpme-app-949487637206#.p0mj7n8l5">Dataplay HelpMe App Article</a>
</b>

The first iteration of the prototype built on the day was a Python Flask application written with the 'flask-ask' package which allows the application to communicate with Amazon's Alexa (also known as Echo) device.  It was deployed in Heroku and then set up as an Alexa skill in Amazon's developer portal.

Using the portal you have to set up intents, which is how variables from a person's spoken request are captured.

![Setting up Alexa's intents and sample utterances]({{ site.baseurl }}/assets/alexa_utterance_and_intents.png){:class="img-responsive"}

Finally after setting up the rest of the Alexa skill in Amazon by providing the SSL certificate and pointing it at the application which is running on Heroku, you can then test it via the simulator.

![Testing the application using the service simulator]({{ site.baseurl }}/assets/alexa_service_simulator.png){:class="img-responsive"}

To demonstrate the idea of someone asking the application to contact their emergency person, all aspects were hardcoded.  The prototype alongside an description of how it could be used was submitted to a committee.  I received excellent feedback that it was their highest quality submission so far, however my application was too 'global' and there were looking for something that would help Plymouth directly.  However, I am in communication with Plymouth community homes about testing and developing the prototype further to help them support older or infirm people in their own homes.

Below is the git repository which is public and viewable by anyone.  The idea of it being open source allowing it to provide an example to others.

<b>
    <a href="https://github.com/Pheonnexx/alexa_helpme">Alexa HelpMe github</a>
</b>

In improving the application, I have changed the the api used for sending an SMS to the contact to use one provided by <b><a href="https://www.twilio.com">Twilio</a></b>.  Learning to use this API will allow me to understand and utilise the GOV.UK Notify service at work.

I have started adding functionality to add a contact by a user, it currently takes in their information with plans to then add it to a table, probably using Postgres.

I will be bringing my Amazon Alexa unit into the presentation to give a working demonstration.

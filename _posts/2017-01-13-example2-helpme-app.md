---
layout: post
title: "Ex 2: Alexa HelpMe"
---

## Amazon Alexa HelpMe App

As part of developing my skills within software engineering I aim to attend a number of external events.  This allows me to get a feel of new and relevant technology and skills which could help me and the Land Registry develop quality services.

On December 2nd of last year I attended DataPlay, ran by Plymouth City Council.  They release open source data according to the theme of the hackday that is being run, in this case, health related data.

By attending this event, I would be gain practice in understanding and utilising different data, whilst also improving my ability to create prototypes to prove an idea within a short amount of time.

I have authored an article on the experience that can be viewed in the link below.

<b>
    <a href="https://medium.com/@Pheonnexx/data-play-helpme-app-949487637206#.p0mj7n8l5">Dataplay HelpMe App Article</a>
</b>

The first iteration of the prototype built on the day was a Python Flask application written with the 'flask-ask' package which allows the application to communicate with Amazon's Alexa (also known as Echo) device.  It was deployed in Heroku and then set up as an Alexa Skill in Amazon's developer portal.

Using the portal you have to set up intents, which is how variables from a person's spoken request are captured.

![Setting up Alexa's intents and sample utterances]({{ site.baseurl }}/assets/alexa_utterance_and_intents.png){:class="img-responsive"}

Finally after setting up the rest of the Alexa Skill in Amazon by providing the SSL certificate and then pointing it at the application which is running on Heroku, you are able to test it via the simulator.

![Testing the application using the service simulator]({{ site.baseurl }}/assets/alexa_service_simulator.png){:class="img-responsive"}

To demonstrate the idea of someone asking the application to notify their emergency contact, all aspects were hardcoded.  The prototype, alongside a description of how it could be used, was then submitted to a committee.  I received excellent feedback, with the committee stating that it was the highest quality submission they had seen, however my application was deemed too 'global' and they were looking for something that would help Plymouth more directly.  Nonetheless, I am in communication with Plymouth Community Homes about testing and developing the prototype further in order to help them support older or infirm people in their own homes.

Below is the Git repository which is public and viewable by anyone.  By being open source, I am hopeful it will provide an example to others.

<b>
    <a href="https://github.com/Pheonnexx/alexa_helpme">Alexa HelpMe github</a>
</b>

In improving the application, I have changed the the API used for sending an SMS to the contact to use one provided by <b><a href="https://www.twilio.com">Twilio</a></b>.  By learning to use this API, I will gain the requisite knowledge to understand and utilise the GOV.UK Notify service at the Land Registry.

I have started developing functionality to allow the user to add a contact. Currently, it takes in their information but does not save it to a database. The changes that I have planned will see this data added to a database, probably using Postgres.

 In order to provide a working demonstration, I will be bringing my Amazon Alexa unit into the presentation.

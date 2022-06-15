# **AWS : Building Serverless Application**
### **Services used** :
    Amazon Lex, Amazon S3,

## **1. Creating a Lex Bot**

Our first goal is to create a simple chatbot in the LEX console that can gather basic information from a user, which we call "eliciting slots".

### **Steps for creating a LEX bot**

    1. Go to Amazon Lex
    2. Make sure you are in the N. Virginia region at the top right
    3. Under Creation method, choose Create a blank bot
    4. Bot Name: WeatherCatBot
    5. Under IAM permissions, choose Create a role with basic Amazon Lex permissions
    6. Under Children’s Online Privacy Protection Act (COPPA), choose No
    7. Under Idle session timeout, choose 1 minute
    8. Under Language, choose English (AU)

Now we have created a bot we need to prepare it for the type of questions we might throw at it.
If we prepare LEX correctly with a good sample set of questions (that are basically saying the same
thing) it can use it as "training data".

The idea is that over time, and the more it is used, it can figure out what you are trying to imply. Or what you mean when you talk to it. Pretty cool right? Even if you ask it a question that is not in the sample questions you supply, it is usually smart enough to figure out your intent anyway.

So lets say you want to know if it is too cold for you cat, there are a gazillion edge case ways that you might phase that conversationally right?

"Hey silly bot, cool enough for my cat yet?"

"Cat bot, I bet it is too hot for my cat in Arizona, isn't it?"

"Can my cat go outside yet? He's really bored..oh yeah we're in Texas."...the list goes on.

They call these common question samples "utterances".

We obviously can't type every conceivable utterance, and we don't need to. LEX is pretty smart, and gets smarter over time. We just need to provide a good sample of commonly phased questions that you think users would say. These are related to the INTENT of finding out if your cat should go out or not. And let LEX over time, figure out all the fancy humanized edge case intents.

Let's do that now.

### **Steps for creating an Intent**

    1. Under Intent details, Intent name : CatWeather
    2. Uder Sample utterances, write "Can my cat go outside?"
    3. Click on Add utterance
    4. Enter these five utterances one at a time, in order:
        a. Can my cat go outside?
        b. Is it warm enough for my cat?
        c. Can I let my cat out in {city_str} ?
        d. Should my cat wear booties in {city_str} ?
        e. Will my cat stay dry in {city_str} ?

Did you notice the (city str}? Think of this as a variable, as the user can say any city they want.
This maps to what we call a SLOT.

The advantage in putting this in our utterances, is that if a slot is used in the initial user utterance the LEX bot has all it needs. It no longer needs to elicit that slot and ask the user what city, because it has just been volunteered.

Are you ready to create the slot that we used as a placeholder as city str in some of our utterances?

### **Steps for creating a Slot**

    1. Under the Slots, choose Add slot
    2. Type city_str under Name
    3. For Slot type start typing in AMAZON.City
    4. Under Prompt, type which city? and hit Add
    5. {city_str} will now be colorized in the Sample utterances drop down

Before you save your intent, we want to humanize it a little more by configuring a confirmation step
with "So you want to know if your cat can go out today in (city_str)?"

Let's add this now.

    1. Expand Confirmation prompts and decline responses
    2. Under Confirm Prompt type in the following:
        -> So you want to know if your cat can go out today in {city_str}?
    3. Under Decline response type in the following:
        -> Sorry, can you please repeat your initial question?
    4. Expand Fulfillment
    5. On successful fulfillment type in the following:
        -> Your request completed successfully
    6. In case of failure type in the following:
        -> Something went wrong
    7. Click Save Intent at the bottom
    8. Click Build at the bottom

### **Steps for testing the bot in the Lex console**

    1. Click Test at the bottom
    2. Try these conversations so you can see how LEX responds to you in different scenarios,

| Chatbot                                                        | User                              |
|:---:                                                           |:---:                              |
|                                                                | Will my cat stay dry in Seattle?  |
| So you want to know if your cat can go out today in Seattle?   |                                   |
|                                                                | Yes                               |
| Intent CatWeater is ReadyForFulfillment: city str:Seattle      |                                   |
|                                                                | Can I let my cat out in Arizona?  |
| So you want to know if your cat can go out today in Arizona?   |                                   |
|                                                                | Yes                               |
| Intent CatWeather is ReadyForFulfillment: city:Arizona         |                                   ||                                                                | Is it warm enough for my cat?     |
| Which city?                                                    |                                   |
|                                                                | Denver                            |
| So you want to know if your cat can go out today in Denver?    |                                   |
|                                                                | Sure                              |
| Intent CatWeather is ReadyForFulfillment: city:Denver          |                                   |
|                                                                | Is it too cold for my cat?        |
| Which city?                                                    |                                   |
|                                                                | Las Vegas                         |
| So you want to know if your cat can go out today in Las Vegas? |                                   |
|                                                                | Yes                               |
| Intent CatWeather is ReadyForFulfillment: city:Las Vegas       |                                   |


**NOTE**: Notice we didn't include "Is it to cold for my cat?" in the utterances. Yet chat bot is intelligent enough to prompt us for the city. I think that is so cool. LEX can actually figure out what you are trying to ask.

## **2. Creating a S3 bucket and configuring as a static website**

As you would imagine, creating an application like this involves many moving parts. We will need:

    a front end web site
    a backend API
    some sort of logic (functional intelligence)
    a data source

We already have a website front end all prepared in a zip file. All you need to do is download this zip file, unzip it and push it into the cloud. We will use S3 (Simple Storage Service) which can happily "host" these kinds of static websites.

### **Steps to create a S3 bucket and configure it as an static website**

    1. Go to the Amazon S3 Console
    2. Click Create bucket
    3. Type a unique DNS-compliant name for your new bucket
    4. For Region choose US East (N. Virginia)
    5. Under Block Public Access settings for this bucket, Deselect Block all public access
    6. Under Bucket Versioning, Select Enable Versioning
    7. Click on the bucket and select the Properties, Enable Static Website Hosting
        - Hosting type : Host a static website
        - Index document : text.html
        - Error document : error.html
    8. Click the endpoint. It show an error 403 forbidden, as no permissions have been added yet
    9. Select the Permissions tab and choose Bucket Policy
    10. Paste the following code below in the Bucket policy editor
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Sid": "AddPerm",
                    "Effect": "Allow",
                    "Principal": "*",
                    "Action": [
                        "s3:GetObject"
                    ],
                    "Resource": [
                        "arn:aws:s3:::<name of the bucket>/*"
                    ]
                }
            ]
        }
    11. Click Save

This bucket will now have public access as it will host our web objects. You will notice you can now see absolutely nothing. i.e you will get a 404 error.

Now let's get the website up there.

### **Steps to upload objects to an existing S3 bucket**

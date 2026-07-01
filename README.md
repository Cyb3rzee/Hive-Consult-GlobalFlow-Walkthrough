# Hive-Consult-GlobalFlow-Walkthrough 
Welcome to the Globalflow vulnerable web application walkthrough. This repository contain the comprehensive solutions to all the 10 vulnerabilities discovered in the web application.


# Overview
The GlobalFlow Supply Chain Management (SCM) vulnerable lab is an environment designed for security training and testing for cybersecurity enthusiast and security professionals created by HIVE Consult and Nana Sei Anyemedu. 

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


# Broken Access Control (v1)
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

There was also the distributor endpoint where I can request a distributor by entering and ID. When I entered a number I got access to territory, pricing and credit data.

<img width="1288" height="495" alt="2026-06-23_20" src="https://github.com/user-attachments/assets/cb82c31e-93a4-4f1e-966b-d8b29baa246f" />


# Security Misconfiguration (v10)
Security Misconfiguration occurs when a system, application, or server is set up incorrectly in a way that exposes it to attack. This is a mistake in how the environment around the code is configured. Examples include leaving default credentials unchanged, exposing sensitive files publicly, and leaking internal system details through error messages.

# Exploitation
While checking the features in the exposed and unauthenticated admin panel, I saw a feature for checking system status. I clicked on the load system info and then boom! again. JWT secret, The DB path and admin credentials were exposed without authentication.

<img width="877" height="431" alt="2026-06-23_10" src="https://github.com/user-attachments/assets/01fcb645-9b6e-4ec7-9e67-31202781e887" />

# SQL Injection (v2)
SQL Injection occurs when an attacker is able to insert or manipulate SQL queries through user-supplied input that the application passes directly to a database without proper validation or sanitization. Instead of treating the input as data, the database executes it as a command, allowing the attacker to read, modify, or delete data they should not have access to.

# Exploitation
To exploit the SQL Injection vulnerability, I logged in using the credentials I discovered in the broken access vulnerability I exploited. I noticed the search for shipment feature even though the first thing that came to my mind was to test for XSS which I did but did not work so I went ahead to test for the SQLi instead. I crafted my payload using the database engine clue I got from exploiting the security misconfiguation vulnerability and it dumbed all the shipment. 

<img width="1237" height="282" alt="image" src="https://github.com/user-attachments/assets/25baf08a-56f3-4ca9-ac29-59c4fb5947e7" />

Although the challenge expected a UNION SELECT attack, reviewing the backend source code revealed that the application inserts the input into multiple LIKE clauses, causing UNION payloads to produce SQL syntax errors. This indicates the SQL injection is real, but the intended UNION exploitation path is flawed due to the lab's implementation which did not allow me to get the flag but i was able to get the flag by checking the source code.

<img width="1175" height="93" alt="2026-06-23_22" src="https://github.com/user-attachments/assets/de7b29bf-57b9-4890-90e6-e73a04ff5802" />


# IDOR — Purchase Orders (v3)
Insecure Direct Object Reference (IDOR) occurs when an application uses a user-controllable value such as an ID, filename, or token to directly access an object or resource without verifying that the requesting user is authorized to access it. An attacker can exploit this by simply modifying that value to reference another user's data.

# Exploitation
There is also the purchase order lookup feature where you lookup for an order you purchase. I was able to view  competitor pricing and margin data by increasing the number to 2.

<img width="1239" height="532" alt="2026-06-23_12" src="https://github.com/user-attachments/assets/9dd07ac5-cdd5-48da-ac4c-409dcdc2077e" />

# IDOR — Invoices (v4)
Since we already have idea of what IDOR is, there will be no need to define it again so we will just go straight to the exploitaion part

# Exploitation
Another feature in the application is the invoices, where you lookup invoices using ID. I was able to use sequential invoice ID to access competitor financial records.

<img width="948" height="335" alt="2026-06-23_16" src="https://github.com/user-attachments/assets/65815451-45af-4a4c-91a2-21522c241cd0" />

# IDOR — Supplier Contracts (v9)
Same concept applies to the previous definition.

# Exploitation
There was also ID used to view the supplier contracts details which by increasing the ID to 2, I was able to access confidential supplier terms without authorisation

<img width="602" height="538" alt="image" src="https://github.com/user-attachments/assets/a2687106-1b6e-4c1f-a20e-17ba0d86427c" />

# Price Tampering (v5)
Price Tampering occurs when an attacker manipulates the price or quantity values of items during a transaction typically by intercepting and modifying requests sent between the client and server. If the application trusts client-supplied pricing data instead of recalculating it server-side, an attacker can purchase items at an unauthorized price, including zero or negative values.

# Exploitation
So since I can update shipment, I decided to temper with the price by adding a negative number which ended up working 

<img width="950" height="441" alt="2026-06-23_14" src="https://github.com/user-attachments/assets/ed19d6ac-05df-4484-8777-4a8e59674f2a" />

# Broken Authentication (v6)
Broken Authentication occurs when weaknesses in an application's login, session management, or token handling allow attackers to compromise user accounts or impersonate other users. This can include weak passwords, missing brute force protection, improper session expiry, exposed or predictable tokens, and flawed multi-factor authentication logic any of which can allow an unauthorized party to gain access to accounts they do not own

# Exploitation
To exploit this vulnerability, I decided to login to the admin account by using default username and password admin/admin123 which logged me in

<img width="421" height="503" alt="2026-06-23_19" src="https://github.com/user-attachments/assets/2419eef6-7d3c-47f1-955b-ecc0002e81f9" />

# Sensitive Data Exposure (v7)
Sensitive Data Exposure occurs when an application inadvertently reveals information that should be protected such as passwords, tokens, API keys, personal user data, or internal system details. This can happen through unencrypted storage or transmission, verbose error messages, publicly accessible files, or sensitive values hardcoded into client-facing code such as JavaScript bundles.

# Exploitation
While checking the supplier profile feature, I noticed internal cost prices and margin percentages returned in the supplier API response

<img width="938" height="345" alt="2026-06-23_15" src="https://github.com/user-attachments/assets/d52ebc7f-5772-4169-830d-17171efc8e28" />

# Business Logic Flaw (v8)
Business Logic Flaw occurs when an attacker exploits weaknesses in the intended workflow or rules of an application rather than technical vulnerabilities in the code. The application functions as designed, but the design itself can be abused in ways the developers did not anticipate such as skipping payment steps, applying discounts multiple times, accessing features reserved for other user roles, or manipulating the order of operations to achieve an unauthorized outcome.

# Exploitation
Since I can create a new order, I manipulated the price by adding a negative number and the oder was accepted which resulted in fraudulent credit manipulation

<img width="1238" height="466" alt="2026-06-23_21" src="https://github.com/user-attachments/assets/545ed15a-0733-470c-8c56-b38e8da355c3" />


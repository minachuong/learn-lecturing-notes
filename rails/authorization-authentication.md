Authentication: “Are you who you say you are?”
Verifying identity of user (401 Unathorized)
Bouncer: checks driver license photo and compares it to your face


Authorization: “What do you have access to?”
Verifying permissions of user (403 Forbidden)
Bouncer: checks what color of wrist band you have to direct you to what area of the club you have access to; green wristband means backstage access; orange means mid-level arena access

Sessions:
Authentication Workflow
User provides credentials (username & password); login
Server checks credentials in db
Server creates a “session” in the db associated with the user
Server then sends back something called a “cookie” which has the “session ID” 
Cookie is a piece of data stored on your computer when you visit a website. (It’s why you get notifications from websites asking you Accept Cookies - often talking about how it’ll help better your experience. Often third-party cookies are used to track your browsing habits and that data gets sold so that advertisers can target their products/clients. Most of the time it’s for transparency to tell you they’re storing something on your computer. - legalities because of internet privacy acts that have been enacted). Remember username? —cookies
Session (represents the duration for when the user is using the app through a browser). 
Session cookie (expires session when browser/tab is closed - deletes cookie)
From then on, as a user uses the app making requests to the server, the cookie is attached to the requests
Server gets requests, looks for the cookie, parses the session ID from cookie, goes to db to check if session is still in the db,  if it is, grant access to user, if not, return a 401 or redirect 300
When the user logs out, client makes a request to the server to destroy session and clear cookies.

Show where cookies live in browser (look at kinds of data it’s storing)



Friday was prepping for the Authentication / Authorization lecture but because we have jumpstart this weekend, I’ll actually be out of office that day which is Wednesday. Also finally finished up reviewing assessments, looks like everyone but Alan put a lot of work into their assignments. 

Right now, working through items on the TODO list for Jumpstart start. 
I’ll continue to work on that

# Co-op CRUD application

## Motivation
A couple years ago, my Mom became the treasurer for a local homeschool co-op I went to. I noticed she started to spend a lot of time sitting on her bed, doing stuff on her computer. I would ask her what she was doing, and she would say that she was doing work for the co-op. At some point, she spent so much time on the computer that I decided to dig deeper, to see if I could help. Mom never was a computer person. 

She explained that she was updating some family information on the Co-op Excel spreadsheet, and when I asked how many updates she had done that day (she had been at the computer for a solid hour) she said it was her first one. In a whole hour, she had not completed the task of changing the recorded age of one student. This wasn't her fault, the Co-op used to have someone who considered themselves an Excel savant, and after the "Excel" person left, the board put my Mom in charge. It probably wasn't the boards fault, Mom probably volunteered out of the good will of her heart, but they chose the wrong personto run this Excel spreadsheet.

## This Mess of a Spreadsheet
> NOTE: I no longer have access to the spreadsheet, so this description of how it functions is coming from memory. You will get the gist, though the details may not be 100% accurate.

The main purpose of this spreadsheet was to generate reports and summaries on who is registered for which class at which time throughout the co-op day. These summaries were posted on walls throughout the co-op and would be the thing you would go to reference if you didn't know where you were going next. These reports were generated through a macro, which sourced data from 3 tables: Grades, Families, and Classes. The primary issue with this system is that because of the way the spreadsheet was designed, if you manually edited data from any table, the reports would generate wrong. Instead, you had to use some macros that the Excel Person wrote to input your data. That data you put in the macro would then be duplicated and copied to several different tables across the entire spreadsheet. There was no single source of truth, and storing and manipulating data felt like writing a database in HTMX (if that were possible).

Moreover, all the information about each student was held in a column in the "Families" table. There was a "Student1" column, a "Student2" column, and so on. So every time the "Co-op Largest Family" record was broken (which happens often at gatherings of Homeschooling Christians), the macros governing the relationship and flow of data in the spreadsheet had to be COMPLETELY RE-WRITTEN.

There were also various bugs with the macros, and getting them to execute properly was difficult.

## The First Solution

I saw this and decided that what the Co-op really needed was a database. No-one on the board, however, had any familiarity with or willingness to learn even basic SQL. It looked like if I was going to solve this, I needed a web app. I had no previous experience at all with web-dev, but after watching a FireShip video on HTMX, I figured I could probably pull it off.


The original version was written in Python using Flask as my web-server. To the detriment of that project, I didn't really bother to learn to much about Flask. I saw how you could use some decorators to route requests into functions, and then access that request in the function, and I thought "oh, that is enough to get me going". So having never heard of Templating lanugages, Auth Middleware, innocent little me got to work. I designed a sqlite database schema with some basic relationships, and when I was ready to get into the web stuf, I decided to start with just CRUD-ing the Families table. (Yes, I did have a Students).

I ended up essentially a single file of flask request handlers which manipulated html in the form of raw strings, with a few lines of raw SQLite interspersed throughout. It was a horrible mess, but it (kinda) worked. I was super proud of it.

At this point the leader of the board (or some similar title) started getting her 2 cents in and insisting on scope creep at every opporunity. She wanted payment integration, algorithmic class-swapping so admins wouldn't have to deal with the family requests, online registration, and the list goes on. I was just trying to replace an Excel spreadsheet, and I was working for free. I had just learned what a private key is, and what the S in HTTPS stands for. It was clearly never going to work out, but I though that maybe if I kept working on it myself, I could end up with something which could end up being a useful product in the future. It gave me direction for learning web-development.


So I took a break from that iteration, and decided to go study some more. By the time I came back, I had learned enough to know that it would be best if that break were permanent, and I dropped the project to start again from scratch.


(Here)[https://youtu.be/0scHPvwYvVg] is a screen-recording of me using with the last (semi-)working version of this iteration.


## The Second Solution - TODO




Then I discovered templating, and Go’s SQLC library. With these tools I accomplished what is probably my biggest web-dev accomplishment, which is a to-do list BUT… with hand-rolled session auth. I made a significant effort to make sure that it was actually secure. I salted and hashed passwords, used constant time checks, and implemented CSRF protection. I did not think to use rate-limiting to prevent brute-force attacks, but now

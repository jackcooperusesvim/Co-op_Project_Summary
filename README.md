# Co-op CRUD application

## Motivation
A couple years ago, my Mom became the treasurer for a local homeschool co-op I went to. I noticed she started to spend a lot of time sitting on her bed, doing stuff on her computer. I would ask her what she was doing, and she would say that she was doing work for the co-op. At some point, she spent so much time on the computer that I decided to dig deeper, to see if I could help. Mom never was a computer person. 

She explained that she was updating some family information on the Co-op Excel spreadsheet, and when I asked how many updates she had done that day (she had been at the computer for a solid hour) she said it was her first one. In a whole hour, she had not completed the task of changing the recorded age of one student. This wasn't her fault, the Co-op used to have someone who considered themselves an Excel savant, and after the "Excel" person left, the board put my Mom in charge. It probably wasn't the boards fault, Mom probably volunteered out of the good will of her heart, but they chose the wrong personto run this Excel spreadsheet.

## This Mess of a Spreadsheet
> NOTE: I no longer have access to the spreadsheet, so this description of how it functions is coming from memory. You will get the gist, though the details may not be 100% accurate.

The main purpose of this spreadsheet was to generate reports and summaries on who is registered for which class at which time throughout the co-op day. These summaries were posted on walls throughout the co-op and would be the thing you would go to reference if you didn't know where you were going next. These reports were generated through a macro, which sourced data from 3 tables: Grades, Families, and Classes. The primary issue with this system is that because of the way the spreadsheet was designed, if you manually edited data from any table, the reports would generate wrong. Instead, you had to use some macros that the Excel Person wrote to input your data. That data you put in the macro would then be duplicated and copied to several different tables across the entire spreadsheet. There was no single source of truth, and storing and manipulating data felt like writing a database in HTMX (if that were possible).

Moreover, all the information about each student was held in a column in the "Families" table. There was a "Student1" column, a "Student2" column, and so on. So every time the "Co-op Largest Family" record was broken (which happens often at gatherings of Christian homeschoolers), the macros governing the relationship and flow of data in the spreadsheet had to be COMPLETELY RE-WRITTEN.

There were also various bugs with the macros, and getting them to execute properly was difficult.

## The First Iteration

I saw this and decided that what the Co-op really needed was a database. No-one on the board, however, had any familiarity with or willingness to learn even basic SQL. It looked like if I was going to solve this, I was going to need to turn that app into a website. I had no previous experience at all with web-dev, but after watching a Fireship video on HTMX, I figured I could probably pull it off.

The original version was written in Python using Flask as my web-server. To the detriment of that project, I didn't really bother to learn to much about Flask. I saw how you could use some decorators to route requests into functions, and then access that request in the function, and I thought "oh, that is enough to get me going". So having never heard of Templating languages, Auth Middleware, and a million other important things, innocent little me got to work. I designed a sqlite database schema with some basic relationships, and when I was ready to get into writing the server, I decided to start with just CRUD-ing the Families and Classes table.

I ended up essentially a single file of flask request handlers which manipulated html in the form of raw strings, with a few lines of raw SQLite interspersed throughout. It was a horrible mess, but it (kinda) worked. I was super proud of it.

At this point the leader of the board (or some similar title) started getting her 2 cents in and insisting on scope creep at every opporunity. She wanted payment integration, algorithmic class-swapping so admins wouldn't have to deal with the family requests, online registration, and the list goes on. I was just trying to replace an Excel spreadsheet, and I was working for free. I had just learned what a private key is, and after reading one page of Stripe API docs I decided that this was clearly never going to work out. But I also thought that maybe if I kept working on this project myself, I could end up with something marketable by the time I had learned everything. It gave me direction for learning web-development.

So I took a break from that iteration, and decided to go study some more. By the time I came back, I had learned enough to know that it would be best if I made the break permanent, so I dropped the project to start again from scratch.

[Here](https://youtu.be/0scHPvwYvVg) is a screen-recording of me using with the last (semi-)working version of this iteration.

If you dare, you can read also the code [here](https://github.com/jackcooperusesvim/cdb/tree/presentable).


## The Second Iteration

Around this time I had also started taking classes on Boot.dev, which meant that Go was my new favorite shiny toy. It was through this project that I learned about Session Authentication, Cookies, Middleware, Web-server architecture, Routers, Controllers, Templating, SQL Injection, CSRF attacks, XSS attacks, and more.

I used the Echo library for Go to build this, and it was actually the default middlewares on the documentation page which really taught me about web-security. "Oh, this Middleware is supposed to stop CSRF attacks? What on God's green earth is a CSRF attack? Let me look that up."

I learned a lot this way, and through that process I built a simple one-table CRUD app, but with hand-rolled auth. I was and still am very proud of this achievement. The best part, in my opinion, was realizing that every time I re-write this project, it gets written in a similar amount of time.

I had so much fun with this project that I was kind of slacking off at school. So as my parents started questioning my grades, and other things were being added to my plate, I had to drop this. But I left off on a good note. I learned a lot, and I'm proud of this project.

The code for this project can be found [here](https://github.com/jackcooperusesvim/CoopGo/tree/ee06bb171e04c8f17fb6dd978ae22f4f4c200d59)

## The Current Iteration

I am coming back to this old project because of the Junior Programmer position that is opening at 37 Signals. There is probably no-where I would rather work than 37 Signals, so I decided it was worth resurrecting this old project to re-write in in Ruby (on Rails). This project is currently in a private git repo, but I am working on documenting my work and making it public right now (to be finished tonight).

I started with the ORM; designing a schema to fit my problem and figuring out how to express it in ActiveRecord. Having never used an ORM, and hating the thought of ditching my beloved SQL for some inefficient abstraction for dummies, I was initially resistant, but as I went on I grew to love the combination of low-level transparency and high-level abstraction that ActiveRecord provides.

Then I discovered Rails generators, which just completely flipped my world upside-down. I some "rails generate"-ing  and stumbled upon the new authentication generator. I built family, teacher, and admin models off of the generated model, so now I can begin working on the rest of the project with my structure set in stone. I am starting with the controller for the admin page listing all the families. CSS is also something that I feel under-educated on, so I am going to be learning that as I work with these views.



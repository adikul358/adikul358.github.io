---
title: Developing the Kaafila Website
date: 2020-11-26
tags: [web-development,ubuntu,dev-ops,dovecot,express-js]
media_subpath: /assets/posts/2020-11-26-Developing the Kaafila Website/
---
I was added to the Instagram group for Kaafila’s website development out of the blue one day. I asked the team lead Kalyaani Manoj about it, who said I was a suggested person but as I wasn’t in the AHA Theatre basket, it wasn’t mandatory for me to be in. But I was interested in being part of a development project as I hadn’t coded in months. I asked the lead developer Abhimanyu if I could pitch in and he agreed.

First step to get started was to get the design guidelines from the design team. They gave us theme colours and fonts for individual events and some rough sketches on the visuals of the webpages. The colours and fonts were in separate Word documents and the fonts were just named so were difficult to visualise. Hence, to understand the system better I organised them in to one page. This included downloading all fonts and adding CSS snippets to link the fonts on the webpage, to have as handy later on during development.

![Kaafila's Design System](/img/0002 - design system.png)
_Kaafila's Design System_

Post this we (Abhimanyu and I) had a couple of meetings with Manjima ma’am to get a lot of specifics and functionalities of the website decided and jotted down. After it was clear what all was expected from the website, we proceeded to narrow down on a lot of crucial choices such as database schemas, external backend services, frameworks and libraries and most importantly the Backend and Frontend flows. We resorted to a 2-hour meeting and Lucidchart to properly decide and remove any ambiguity on what we were going to do. We split the work, added tasks and subtasks on a Todoist project and broke ground.

![](/img/0002 - ux and backend flow.svg)
_UX and Backend Flow for Kaafila_

We had the codebase of the previous year’s Kaafila website but as that was based on a PHP backend and we were using Express.js, a much more efficient and modern framework, we scraped it and started from a scratch express-generator project. Express.js has several advantages such as using Javascript, based on node.js, really well-documented and supporting multiple highly useful libraries.

For our web server, we went with our previous year’s hosting service provider GoDaddy; Buying a VPS this time due to our increased requirements of services such as a mail server, node.js instances and increased bandwidth. I had taken up the task of setting up the server, which was a big struggle with multiple frustrating road blocks.

I had set up a node.js web-app previously for Heisenberg’s Corner, so that was an easy job of 20 minutes. What the biggest challenge was, was setting up a mail server. I had attempted to do that too for another web project for the school, but had given up due to its lack of necessity. But that wasn’t the case this time as having a mail server was critical for multiple operations such as communications and automated email notifications. I started by following a lengthy and detailed tutorial on linuxbabe.com but due to some unknown fault, it didn’t work at the end and no mails were getting transferred with IMAP authentication not working either. Second attempt was using iRedmail, an all-inclusive linux package which included scripts for setting up and configuring Postfix, Dovecot, ufw (uncomplicated firewall) among other things. First attempt at iRedmail broke all configurations for my NGINX virtual server blocks hosting the node.js instances for the website and didn’t transfer mails either. Upon further reading it was found that it needed a fresh install of Linux. Thus, the third overall attempt included rebuilding the server and then installing iRedmail. This ran into multiple errors and dead ends along with being less transparent too, which affected debugging any issues as multiple configurations and logs were layered with iRedmail’s scripts. Thus, the fourth attempt was scraping iRedmail all together, by rebuilding the server, and following the initial tutorial again, this time inspecting every line of configuration added or edited and reading further into individual documentations of Dovecot, Postfix and NGINX. This time, the mail server finally worked with all IMAP authentications and SMTP and LMTP transfers smoothly working. This process took two long weeks.

![](/img/0002 - nginx config.png)
_Snippet of the NGINX Configuration of the Main Website_

The road concerning server setup was paved from that point on. I set up SSL certification for the main domain using certbot (python and NGINX) and the SSL certificate issued by GoDaddy. I made virtual server blocks for a development website to test features and UI changes, and an administrator website to monitor registrations. As the issued certificate was valid for only the www and non-www domains, I made self-signed certificates from Let’s Encrypt for the admin and dev subdomains. All email accounts were successfully integrated into one Gmail account for ease of use thanks to Gmail’s extensive IMAP and POP3 services.

![](/img/0002 - certs.png)
_Security Certificates for the Main and Development Domains_

Coming to the main codebase for the website, it included a lot of new and used frameworks. Both Abhimanyu and I had worked with Handlebars.js, Express.js and NoSQL databases before for Heisenberg’s corner. But we were using Semantic UI, Firebase Auth and Cloud Firestore for the first time.

We stumbled upon Semantic UI while deciding on a UI framework to use. As backend developers, using a UI framework is a world of help as it provides you with pre-written CSS and JS concerning all UI elements such as buttons, grids, containers, etc. Semantic UI is a really well documented framework which even supports multiple server rendering libraries such as Reach and Vue. We also had a repository of templates made with it on Semantic UI Forest, which was helpful for components such as the navbar and to get us started with sample code. Handlebars.js is such an efficient and easy to render dynamic content on the server side. We used to render all sorts of content such as list of videos, text on about pages, YouTube and Zoom links, the image gallery, event and sub-event pages, etc. To put its efficiency into perspective, it just took us to input a 150 line file of handlebars to render a 2050 line file of browser-compatible HTML.

A working UI was our primary target as our website launch was scheduled on 12 August 2020 as it was the International Youth Day, and all webpages were required with all event details and media. Collecting these took a long time as multiple files required access viewing or editing access, some were delayed from their respective departments, etc. But due to hard-working Resources and Outreach teams, these were edited and consolidated in time for the website to successfully launch on the twelfth.

Authentication was so new and complicated to initialise, it ate up most of the backend development time. It required us to study JWTs, CSRF security, session management, server and frontend jumbling of auth tokens, and the list went on. Post struggling with all those concepts, we had to intensively read Firebase’s documentation in order to learn how to implement all these with Firebase’s JS libraries. Although to Firebase’s credit, it did provide us kickstart guides which helped big time.

![](/img/0002 - auth whiteboards.png)
_Final Whiteboards of User Authentication_

Working with Cloud Firestore was comparatively way easy as we had experience with working with NoSQL with MongoDB, which Firestore also used. There were some new concepts such as Firestore’s library, read and write rules, but these were overcome too with time and constant learning and debugging. I wrote multiple python scripts too to do a lot of grunt and repetitive work regarding both user and database management.

Regarding scripts, I also wrote some bash scripts on the server for some makeshift DevOps. The scripts help stop the forever.js background process, update code from the git repository, and restart the forever.js process with a single one-line ssh command.

Specifics aside, this was a phenomenal learning opportunity which made us dive deep into the workings of node.js, handlebars.js, Semantic UI, Firestore and Firebase Auth. It gave us new knowledge about javascript practices, async-await functions, git shell, bash shell, ssh, Linux, which are usable in a vast spectrum of programming projects.

Everything aside, this course of working on something as big as Kaafila was such an enthralling experience with meetings with officials from the school administration, artists, department heads; late night meetings with friends, combining work and laughs, and having the ‘good exhaustion’ after feeling you’ve done considerable work for a brilliant project.
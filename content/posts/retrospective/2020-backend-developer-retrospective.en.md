---
title: "2020 Backend Developer Retrospective"
date: 2021-01-01T19:59:18+09:00
series:
  - General
categories:
  - Retrospective
tags:
  - Retrospective
  - General
draft: false
summary: >
 Looking back on 2020 and beyond
---

> Note: This English page scaffold was auto-generated. Full manual translation will follow.


Looking back, this year was truly an eventful year. Like a stylish lion who has been active for a year, the 7th term was successfully completed and the 8th term management team was selected. And while conducting management training, preparation for employment was carried out at the same time. In fact, he was successful in serving in the industry, but in the end, all sorts of things happened before he quit the company and joined the military. 2020 is a year that leaves me with many regrets. Through this retrospective, we want to look back on our shortcomings and improve them.

Like a stylish lion
---

### Goals are important.

Ever since I was in middle school, I always liked computers, and I shared IT-related knowledge through Naver blog. As a result, I naturally developed an interest in coding. But that didn’t mean I fell in love with coding in earnest. This is because the C language I first encountered with `first` was too difficult, and I had no clear goal of wanting to do something with `second`.

### Finding my dream while working as an executive.

To me, `like a stylish lion` was like a sweet rain to me. I got to know Mutsa through the recommendation of [Friend](https://github.com/jongwooo), whom I dated during my first year of college, and thanks to that, I even applied to the 7th management team. At first, I didn't want to be an administrator because I had to stand in front of people and teach classes. But looking back now, doing `management activities` was of great help to me. In order to fulfill the role of an executive, I needed to study a lot, and this became `driving force` that constantly improved me.

His activities as a member of the 7th class of Mootsa were successfully completed with completion in early 2020. Although I did not get any special results in the hackathon during the 7th semester, the process of conducting the class itself meant a lot to me. While preparing the lesson plan, I had a goal of `backend developer`, and I came to think that coding is not difficult and fun. From then on, I started dreaming of becoming a programmer in earnest.

### Disappointing points

From my experience, programming theories and concepts are important, but the biggest part of what I feel and learn is by making things myself. Therefore, the class was conducted using `Practice-oriented` rather than PPT, and the necessary concepts were explained through the contents summarized in `Notion` and `Panseo`. At the time, we thought that this method would be efficient for learning, but there may have been people who did not understand the class well. As I was preparing for class and studying at the same time, I think there were a lot of things I was not prepared for during the class, and I still feel sorry for the club members for this.

Decided to get a job instead of the military.
---

### Instrument

I didn't want to go to the military. I had just started planning a dream called `backend developer`, but it was difficult for me to put this aside for a while and go somewhere else. I constantly wanted to move forward, and the way to do that was to get a job in industry. Entering an industrial company not only had the advantage of not having a career break, but also the advantage of being able to receive a salary that was incomparable to military service during the three years of service. I took a leave of absence from school for the first semester of my third year, thinking that I had to go to work at all costs. Like a stylish lion, I was conducting training for the 8th class of executives and preparing for employment at the same time.

### Become a job seeker.

I was able to succeed after several months of job hunting preparation. Information needed for employment was usually obtained from [Awesome Alternative Military Service](https://github.com/sesang06/awesome-alternative-military-service) and [Unauthorized Information Sharing Cafe](https://cafe.naver.com/webcenter), and job applications were usually made from [Rocket Punch](https://www.rocketpunch.com/) and [Industrial Support Military Service Workplace](https://iljari.mma.go.kr/caisBYIS/main.do). After seeing the announcements posted here and applying to several companies, one company contacted me and I was able to get a job opportunity.

### Succeeded in finding a job.

The task for one week was to build `API server` based on `Flask` ​​and create a simple service that manages data and user sessions using `postgresql`, `sqlalchemy`, and `redis`. During that period, I only focused on doing the assignment and put everything else on the back burner. This is because the technology stacks listed in the requirements were unfamiliar to me. In a situation where I had to upload `web server`, `WAS server`, and `DB server` onto the `Docker` container, there were so many things I didn't know.

Since I couldn't use Docker, which I needed to build right away, I couldn't even use the other technologies. So, I devoted a week to development, excluding sleeping time. As a result, we were able to build [API Server](https://github.com/junghoon-vans/forum-API) that satisfied all requirements. I thought that what I learned during the short entrance exam period was so valuable that I had no regrets even if I failed. Moreover, since I was unable to answer many questions well in the subsequent interview, I was giving half up, thinking I would definitely fail. Contrary to my expectations, I passed the entrance exam.

> I almost got automatically disqualified because I didn't know that the acceptance email was in my spam folder.

### Things I gained from corporate life

The company was located in a shared office located in Gangnam. The sight of commuting to work instead of school was new and exciting. I also liked that the company provided a 15-inch MacBook and a Dell monitor. As soon as I joined the company, I did not work and was given `adaptation period` for about 10 days. During this period, I improved my understanding of the service by performing the task of running the server source code in GitLab on `Development Server`.

Afterwards, I worked on writing `test code` and fixed bugs registered in `issue`. The most memorable work is `Slackbot feature improvement`, the last work I did at the company. One of the service features was chat, and this feature was used to allow users to ask questions while using the service. After opening a chat, users close the chat when they no longer need to continue asking questions. This method is similar to creating an issue on GitHub and continuing comments below it.

In order to effectively manage these users' chats (inquiries), the company used Slackbot to send the same message to the Slack chat room. However, this function outputs messages sent by users one by one to Slack, making it difficult to understand the contents at a glance. In other words, it was not very readable when the chat corresponding to one inquiry was separated into several.

So, messages about the same inquiry were posted on GitHub to create `Function to show by combining them into one`, and I took on the role of resolving this. When I searched for `Slack API`, there was no function to `merge` a message. Instead, we solved the problem by using `chat.update` and `search.messages` to display the new message by merging it if it corresponds to an existing inquiry, and otherwise create a new message.

### Finally, I quit the company.

About a month after I started working at the company, the supervisor quietly called me and my co-worker. They said that there were many people like us in the company who wanted to serve as industrial agents, and that one or two of them were selected, but that it was not us. He said that if you want to continue working at the company, you can do so, but if not, you can leave the company.

The purpose of joining the company may have been to make money, but the main purpose was to replace military service, so this proposal was essentially the same as asking me to leave the company. In the end, I was fired... no, I decided to resign.

The reasons why I had to leave the company at that time were as follows.

- Insufficient contribution
- Keep your appointments on time
- Passion(?)

I don't remember exactly, but the shooter said that startups cannot afford to wait for new employees to grow sufficiently, and that they want talent who can produce `performance` right away. I have often been pointed out that the process of setting and maintaining `period` is not carried out well when carrying out certain tasks. I was also told that if I couldn't achieve results, I had to show `passion` by staying at the company late and working.

These teacher's words ultimately came back to me as a sharp reminder and served as an opportunity for me to truly reflect on my shortcomings. *Next time I join a company, I promise to become a capable person who can do my part at the company without having to stay up late.*

> I experienced the bitter taste of life when I left the company, but ironically, the first paycheck deposited that day was very sweet.

The end of this year is military service.
---

### I applied to the company again, but

In the end, the only thing I could do after quitting my job was to find the next company or join the military. Afterwards, I applied to several companies, but I wasn't able to get into the company I wanted to work for. One company told me that if I liked it, they would give me a TO that would come out next year (2021) and that I should wait while working at the company. They even put up an unusual condition that I could get a job at the same time, but I said no and kicked the opportunity away. (Now that I think about it, I don't know why I did that...) And I thought I would get hired at another company if I went for an interview, but I politely declined to go to the company because I didn't think it would be helpful to my career.

### Information protection unit support

As I said earlier, the reason I didn't want to go to the military was `career break`. Now, for me, who is trying to pursue my dream and move forward, the military was just an ‘obstacle’. No matter how much my parents told me that men should go to the military, I didn't listen. However, since it was not possible to work in an industrial company, it was not possible to take endless leave of absence from school and stay at home. So I ended up choosing the military, and applied for a position called `Information Security Guard` that was still related to my major. In addition, I had high expectations after hearing that the unit would be raised to the level of a corps or higher rather than a division.

### Self-deployment, despair

Even though my grades were among the top five among intelligence protection soldiers, I was assigned to the 28th Division. When I think about all my friends going to `Jijaksana Gungun` in the second half of the year, my stomach still hurts. During the last week of the second half, I felt like I was almost out of my mind. I kept wondering why I was assigned to a division. As a result, I finally realized it. **Don't gamble.** That's my conclusion. I can't help but regret that my life is something I pioneer, and that this is what happens when I entrust myself to something like the military where I don't know where I will end up. *The decision to apply to the military was made by me...*

future goals
---

Since you are in the military, you can never leave before a certain period of time has passed. That's the rule of the military. So, the important thing is how much you can accomplish in that time. I want to spend my military life without regrets by making a plan and executing it. Below is a rough goal for that.

- Steady blogging activities
- Preparing for coding tests
- Use of Baekjun problems, etc.
- Study software architecture
- GoF design pattern
- Java object-oriented design patterns
- Clean architecture
- Studying Agile
- Refactoring
- Clean code
- Insufficient Java study
- Effective Java
- In addition, read 60 books
- Diet and exercise for 100 days

Due to the nature of the military, it is not easy to code properly, and I want to be a better developer after being discharged from the military than I am now. Therefore, I decided to study as much as possible in the military to gain the knowledge I needed to have as a developer. And here, my values ​​are revealed: **People feel happy only when they lead a life of constant study**.

I truly regretted joining the military. Especially after unit deployment. But when I think about it again, when will I have the opportunity to study this much again? It's a good opportunity to study now, which you wouldn't have been able to do while going to school or work because you were busy. I have absolutely no intention of lightly throwing away the remaining time of the year. *I don't want to leave behind any regrets as I am responsible for the choices I made.*
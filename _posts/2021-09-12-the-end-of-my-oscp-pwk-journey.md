---
title: The End of My OSCP/PWK Journey
excerpt: My thoughts and review of OSCP/PWK as well as recommendations for taking the certification
date: 2021-09-12 21:00:00 +03 GMT
categories: OSCP PWK
---

## Preface
Before I begin, I'd like to state that I usually keep a pretty "clean" and "formal" language in my blog posts, tweets, 
and such, but I guess this post will be an exception. I also try to be "scientific", stating all the details 
precisely and as unbiased as I could be, but considering that this is about a journey that lasted approximately a 
year, some details are bound to be fuzzy, and I apologize for that.

I also planned on posting this back in 2021-08-22, but I've had some personal issues and TODOs arise and this kept 
getting pushed back to the end of my TODO list. But after nearly a month of delays, I decided to put the nail on this 
coffin and deliver what I initially promised.

## Introduction
Back in mid-2020, I started preparing for my first attempt at being an Offensive Security Certified Professional 
(OSCP). Around July, I started practicing, taking notes, and researching by doing Hack The Box (HTB), before I felt I 
had a comfortable date to start my OSCP journey. That date was on August 14, when I signed up for the course, and 
started accessing the labs on August 23rd. After a year with nearly 100+ boxes pwned, a total of 3 attempts, on July 
25th, 2021, my OSCP journey concluded when I got the e-mail from Offensive Security stating that I passed: 

<center>
    <blockquote class="twitter-tweet" data-lang="en" data-dnt="true" data-theme="dark">
        <p lang="en" dir="ltr">
            Nearly a year ago, I started my <a href="https://twitter.com/hashtag/OSCP?src=hash&amp;ref_src=twsrc%5Etfw">#OSCP</a> journey. Today, I&#39;m more than happy to announce that journey has concluded and now that I&#39;m officially
            OSCP certified!<br />
            <br />
            Thanks to <a href="https://twitter.com/offsectraining?ref_src=twsrc%5Etfw">@offsectraining</a> for the challenge and the experience. <br />
            <br />
            Here&#39;s to more to come! <a href="https://t.co/kxEdN7t2L5">pic.twitter.com/kxEdN7t2L5</a>
        </p>
        &mdash; Arszilla (@arszilla) <a href="https://twitter.com/arszilla/status/1419321910081560580?ref_src=twsrc%5Etfw">July 25, 2021</a>
    </blockquote>
    <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</center>

Nearly a month later, my certificate arrived at my door on August 20th, 2021:

![OSCP Certificate][OSCP Certificate]

I'll admit, the journey was harder than I thought for reasons I'll explain later in the blog post, but after a year of 
pain, sweat, and tears, I feel like it was worth it. OSCP allowed me to improve myself to a point I doubted to reach 
as a junior penetration tester (at the time of me writing this) amid unemployment, and I'm proud of what I've achieved.

## OSCP
Continuing what I've said in my [introduction](#introduction), there are some individuals that trash OSCP because of 
it's price and content it offers. I'll give those critics some credit, as I complained about OSCP's (and Offensive 
Security's) pricing on Twitter back in June, when they increased the price of a retake by 66%:

<center>
<blockquote class="twitter-tweet" data-lang="en" data-dnt="true" data-theme="dark">
    <p lang="en" dir="ltr">
        Earlier today I received this mail from <a href="https://twitter.com/offsectraining?ref_src=twsrc%5Etfw">@offsectraining</a> regarding the drastic price change in OSCP’s retake fees.<br />
        <br />
        The price change is too drastic and should be either reverted or lowered IMO. <a href="https://t.co/NrjvPicndk">pic.twitter.com/NrjvPicndk</a>
    </p>
    &mdash; Arszilla (@arszilla) <a href="https://twitter.com/arszilla/status/1407744542015733760?ref_src=twsrc%5Etfw">June 23, 2021</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</center>

However, the issue that most people (seem to ignore) is that OSCP (and other Offensive Security) certifications are 
globally respected by organizations and companies. Unless the "establishment of measuring knowledge" is changed to 
other certifications (definitely not CEH though), OSCP will be one of the core certifications that a pentester will 
need to have.

However, I have a dislike for the prices of certifications. I've posted my OSCP story both on Twitter and LinkedIn, 
where I've observed a lot of people reacting with the aforementioned posts. The majority of these people interacting 
with my post were mainly in one of these 2 categories:
1. Students
2. People in developing countries

With such a demographic, the pricing of such a "core" certification is troublesome, because of exchange rates. As I've 
stated previously, many organizations and companies use OSCP or other certifications as a way of certifying the 
knowledge of an applicant. With that comes a vicious cycle for unemployment and a shot in the dark for acquiring such 
certifications, because the applicant needs money to get the certification (or their place of employment's 
sponsorship), but they can't get that since they can't get employed. You see where I'm going with this, right?

Now, despite people giving Offensive Security and OSCP shit for this cycle, I don't fully blame Offensive Security. 
This is an issue that needs to be addressed by many companies that provide certifications as well as countries in 
which the people live at. When your minimum wage is utterly useless due to inflation, I cannot blame you for not being 
able to afford such an expense. Because you'll have to focus on your survival and basic needs rather than a piece of 
paper stating that you're actually capable of doing something you claim to do.

I'd talk more about this matter, but this post is (sadly) not about this matter. Maybe this will be a topic for 
another time, but this is an issue that has to be addressed in my opinion.

## Preparation for the Exam
I started preparing for OSCP around mid-July if I recall correctly. I started off by reading other people's blog 
posts, like my good friend and mentor [TJNull]{:target="_blank"}'s 
[The Journey to Try Harder: TJnull’s Preparation Guide for PWK/OSCP][TJNull's Blogpost]{:target="_blank"} and many 
other blog-posts like it, in order to see what boxes I should try to do, what should I watch out for, etc.

By this point, I have been holding on to a 3-month HTB VIP pass that I've acquired by achieving 2nd place with my 
teammates in THMC x HTB CTF, thus I had no issues with the majority of the boxes listed by TJNull and others being 
retired. But despite having a 3 month VIP voucher, my main concern was rooting as many boxes as I could before the end 
of my voucher and before my (first) exam attempt. The reasoning behind this mentality is because I felt that once the 
3-month voucher ended, I wouldn't be able to purchase a subscription since at the time I was unemployed and was 
conserving my money for emergencies and alike. 

As a result of such a mindset, I've set myself a goal of rooting a box a day until my OSCP lab time started. By the 
time August 23rd came, I recall rooting over 40 boxes around the span of 30 days. This was also a good practice to see 
what I might be able to do with my 30 days of lab-time in Offensive Security's network.

On August 23rd, I shifted my focus towards Offensive Security's content. It took me roughly a week to read the 853 
page PDF while doing at least a root a day. Since I only had 30 days of lab time (compared to HTB's 90 total), every 
day I spent on Offensive Security's labs was precious, compared to the time I'd spend on HTB. By the end of my lab 
time, I had 42 out of 60-70 boxes rooted. 

But all this nonstop grind came at a cost. After 2 months of nonstop hacking, where I hardly took any day off since 
time was not a commodity I had to spare, I started having a massive burnout by the end of September. I was constantly 
tired, unable to sleep, enjoy the music I've used to listen to, play any games with or without friends. I couldn't 
even bring myself to do any work no matter how hard I tried, because my body was exhausted and disgusted by the 
routine at this point. As a result, I decided to take a few days off to recharge myself, both mentally and physically, 
It honestly felt odd, not doing anything for hours, since I've gotten used to a (harmful) routine for the last 2 months.

After I took some time off, I continued doing some more HTB and worked on a cheatsheet for the exam, based on the 80+ 
boxes that I've rooted so far.

By the time I came to the end of my OSCP journey, I've completed the following boxes in HTB:
- Academy
- Active
- Admirer
- Arctic
- Armageddon
- Bart
- Bashed
- Bastard
- Bastion
- Beep
- Blue
- Blunder
- Bounty
- Brainfuck
- Buff
- Cache
- Cap
- Cronos
- Delivery
- Devel
- Doctor
- Forest
- Fortune
- Fuse
- Grandpa
- Granny
- Hawk
- Heist
- Jeeves
- Jerry
- Knife
- Laboratory
- Lame
- Legacy
- Love
- Magic
- Networked
- Nibbles
- Omni
- OneTwoSeven
- OpenAdmin
- OpenKeyS
- Ophiuchi
- Optimum
- Passage
- Poison
- Remote
- Sauna
- ScriptKiddie
- Sense
- Shocker
- SneakyMailer
- Spectra
- Tabby
- Tally
- Tenet
- Time
- Traceback
- Traverxec
- Worker
- Writeup

## The Exam (Strategy)
I've always booked my exams nearly a month or more before the actual date I'd take the exam. Since I am a night owl, I 
always opted in for 20:00 or 21:00 for the exam start time, as I preferred having a couple of hours work before I 
decide to hit the sack and start brainstorming in my bed.

In all my 3 exam attempts, I always:
- Booked the date of my choice at least a month if not 1.5 months in advance,
- Prepared my notes (boilerplates etc.),
- Prepared all my tools and initial reconnaissance commands,
- Prepared my report (boilerplates etc.)

before the exam's starting time. I also planned out a methodology for the exam. For those unaware, OSCP consists of 5 
boxes to be hacked:
- 10 pointer
- 20 pointer
- 20 pointer
- 25 pointer
- 25 pointer (Buffer Overflow)

I planned my methodology on the idea that `nmap` or even `autorecon` scans might take some time. Thus, as the scans 
were being conducted, I'd focus first on the buffer overflow and root it within the first hour (at most), then move on 
to the 10 pointer to cross the halfway mark. From that point on, it was a test of my enumeration skills. I planned to 
call it a day before the 01:00 AM or 02:00 AM mark, then on the morning, I'd resume, and hopefully, finish the first 
24 hours with at least 4 boxes under my belt.

For context, below are all the dates of my attempts:
- 9th of October, 2020
- 6th of January, 2021
- 21st of July, 2021

October 9th was a date I chose back in early August as I wasn't sure what the future held for me in many aspects, 
thanks to COVID-19. At the time I was still looking for a job while starting my senior year in university, so I had a 
lot of things on my plate. By the time October came, I was feeling confident in myself, thinking that nearly 2.5 
months of practically non-stop practice would amount to something, hopefully for passing in my first shot 
(*Morgan Freeman's voice: It certainly didn't amount to anything and Arslan failed*).

By the time I slept, I only had ~45 points in the bag with more than 18 hours left on the clock. When I woke up I 
continued pushing but couldn't get the root on the system where I've got a low-privilege user. The exploit I was 
trying either kept blowing up the whole system or took a good 10+ minutes to run and grant root, which was pure pain. 
I tried focusing on the other machines in the meantime but I kept falling for the rabbit holes that Offensive Security 
placed. Sadly, this cat-and-mouse game of exploiting the low-priv and attempting to get low-priv on the other boxes 
continued for the next 12 hours... 

By the end of the exam on October 11th, any shred of self-confidence I've ever had to that point was blown up to 
kingdom come, ran over, then lit on fire. This failure just poured gasoline on the fire that was kindling inside of 
me, which was the imposter syndrome. It also didn't help that a lot of people (including me) are generally "ashamed" 
of their failures and refrain from talking about said failures. Instead, they put on this "facade" that their success 
happened without any failures. This was something I've talked about in my previous 
[blogpost][blogpost]{:target="_blank"} from last year. 

I genuinely felt gutted, thinking I was fucking insane that I'd pass on my first try, constantly telling myself that 
I'm stupid for making a fool of myself. But as time went on, I slowly became somewhat glad I failed my first attempt. 
Now, you might be thinking that I'm insane and that I'd agree with you.

The reason why I'm glad I failed is that most people don't know me and my flaws I **hate** imperfections and it 
doesn't help that I have OCD and ADHD. Taking a look back at my mid-exam notes and my report, I'm not even remotely 
satisfied with the overall quality of my work. I'm genuinely disgusted at myself for having the audacity to submit a 
report that's an utter dumpster fire (in my opinion) and so low in quality that it looks like a porn image from a 90s 
dial-up modem. Thus I see my first exam attempt being a lesson and a reality check rather than a failure for that 
reason.

By the time January 6th, 2021 came around, I started working as an intern and was about to finish my fall classes. 
Around this time, I worked upon my previous mistakes, worked on my enumeration skills, note-taking, and my report 
writing skills. I felt confident that this was it, that I've learned from mistakes and became a better version of 
myself. The exam started out smoothly, and if I recall correctly, by the time I slept, I had 55 points in the bag. 

Now you might think that that was it, that this was the home run. 1 more root and I had the certificate in my hands. 
Because of this, I had a little trouble sleeping, as my brain was trying to think of ways to get that last box. After 
(somehow) managing to sleep, I woke up to continue working on the last 2 boxes. I had around 10-12 hours left, and 2 
boxes to go. I was thinking that this was a piece of cake at this point 
(*Morgan Freeman's voice: It wasn't a piece of cake*). I got the low-priv user on the 20 pointer, but no matter what I 
tried or ran, I couldn't see the way to the privilege escalation. I spent hours going over all the logs I've acquired 
from `lse`, `linpeas` etc, but to no avail...

Do note that I did exploit the 25 pointer, but getting a shell there was harder than the 20 pointer, and I constantly 
shifted my focus between getting a low-priv on the 25 pointer and root on the 20 pointer, but sadly, time ran out. 
There I was, with the OSCP certificate within my reach but getting away at the end. If my idea of how Offensive 
Security calculates points was correct, I missed the certificate by merely 5 points. 5 lousy points.

Now you might be saying "hope you did the lab and the exercise report", but in my honest opinion: that's a waste of 
time. The only thing that might be beneficial there is the report writing for the lab boxes but besides that, I've 
seen this as an utter waste of time and energy where you could spend the time you'd waste with with more HTB boxes, 
research, etc. The P/T (Point-to-Time) ratio is so shit, that I personally see it as the worst way of spending your 
time.

However, this time I knew it was a matter of "RNG", getting some reasonable machines on my end, and using the 
methodology I've somewhat "mastered" and finishing the job. I worked on my mistakes, as usual, and tried keeping my 
head up high, in order to not burn myself out with imposter syndrome. 

By April 2021, I got a full-time job and was starting my midterms for my spring classes, thus I rescheduled my 3rd 
attempt to the Eid Al-Adha holiday on the week of July 21st. Since the price of a retake was not 66% more expensive, 
as well as considering the recent changes in my life, this attempt was a make-or-break deal for me. 

Following the methodology I've highlighted before, I got 55 points before I called it a night and hit the sack. In the 
morning, I continued where I left off and got the 25 pointer. That was it, I fucking did it (well, in theory at least, 
because there was still a report to submit). I remember taking a break just to calm my nerves down and relax, so I 
could attempt to get that last 20 pointer and perhaps do a 5/5 this time. Sadly, the privilege escalation was a bit 
harder than I thought, and I couldn't see a way to get administrative access on the machine. As a result, I spent the 
last hour I had on making sure that my notes had *everything* I needed to write a perfect report. I didn't want to 
risk anything, so I made sure to hack all the boxes swiftly and check my notes for anything missing. By the time the 
timer zeroed in, I finished 4.5/5 boxes.

## Report Writing
In my opinion, if you managed to take proper notes during your exam attempt, the report writing process shouldn't be 
painful or as time-consuming as some people put it. To put it into perspective, in my 3 attempts, I've used 2 report 
formats:
- [Noraj's Offensive Security Exam Report Template in Markdown][Noraj Github]{:target="_blank"}
- [Offensive Security's Report Template][Offensive Security]{:target="_blank"}

With both reports, I've modified them beforehand so I could start typing ASAP and not have any time issues.

In my first exam attempt, I used Noraj's template, which didn't go as smoothly as I hoped, despite my knowledge and 
experience in Markdown, LaTeX, and `pandoc`. Taking a look back at that report, it makes me feel disgusted that I 
could write such a piece of garbage, but I guess you could say that was part of my first exam attempt's reality check. 
If you're acquainted with `LaTeX`, `pdfLaTeX` and `pandoc`, you'll know what I am talking about. All I can say about 
Noraj's template is that in theory it's great and I like it, but sadly, it doesn't pan out well in application.

In my second and third exam attempts, I used Offensive Security's Microsoft Word template. As stated before, I 
prepared everything that I thought I would need, from a dedicated page for the flags, to the graphs indicating the 
open ports in the machines. I was still a bit upset with my 2nd report (as I was still mastering the process), but I 
still think that it wasn't as bad as my first report.

When writing a report, keep these in mind:
1. Use proper English. Know your words, grammar, and such. Don't use Canadian English then change to American English 
out of nowhere. Be consistent.
2. Try to keep the executive summary short, clear, and easy to follow.
3. In the technical aspect of the report, don't write vulnerabilities, etc. like you're writing a writeup. A writeup 
$\neq$ report.
4. While talking about how you've exploited a vulnerability, don't tell your life's story. Keep it to the point, keep 
the relevant parts. Don't talk a lot about `gobuster` and its output if you're heading is about an SQLi in the admin 
login panel. Just state that you've found it by performing reconnaissance via `gobuster`.
5. Place the `local.txt` and `proof.txt` proofs in the vulnerabilities that granted you access to the proofs.
6. Keep the blocks of code brief and on to the point. Trim the bush if you have to. Don't put the whole `nmap` output 
just to show a single port being open.
7. You don't need to make the image as big as the page. Keep it small, but large enough so it could be read if it was 
printed out.
8. Read your report as if you're the person going to evaluate it. Export it into a PDF and open it in Adobe Acrobat, 
your browser, or something. Just proofread it.

## Words of Advice
1. Time management is the key to OSCP. Try to simulate the exam. Choose 5 boxes from HTB etc., sit at your desk for 24 
hours, and hack away with the same rules you'd face in OSCP.
2. Get comfortable with (Kali) Linux. Know it well as if your life depends on it. If something breaks, learn to fix 
it. Because if you're an insane person like I, who spent the past 3 exams running Kali Linux on metal with i3-gaps, 
you better know your environment, or you're gonna spend a lot of time trying to fix it when something breaks. And 
something will break, trust me, it's Murphy's Law.
2. Enumeration is king. Learn to enumerate properly.
3. Sometimes simple is the key. Don't overcomplicate things, This is a fairly "simple" exam at the end, not a real, 
corporate network. Don't overthink.
4. Stay hydrated.
5. Take breaks if you need a breather.
6. Have snacks at your desk. Preferably baby carrots etc. Try to stay away from sugar and alike.

## What's Next For Me?
Well, with OSCP achieved, I'll have to set higher goals for myself and rise above the challenge, with its ups and 
downs. I plan to work towards a few more certifications in order to improve and certify my pentesting and red teaming 
capabilities:
- [OSEP][OSEP]{:target="_blank"}
- [OSWE][OSWE]{:target="_blank"}
- [CRTO][CRTO]{:target="_blank"}
- [eCPTX][eCPTX]{:target="_blank"}

I have no idea when I'll get to them, but I'd like to at least start one by early 2022, that's for sure. If I do, I 
plan to write a blog post similar to this at the end of the journey, as it might help out others.

I also plan on starting my own GitBook containing various stuff I've learned, done, etc. as a note for both myself and 
others, containing stuff from pentesting, red teaming, social engineering, etc., similar to [HackTricks][HackTricks]' 
GitBook. I don't know when I'll get to it, but this is in my TODOs..

## Special Thanks
For my closing words, I'd like to thank a few individuals (in no specific order) that have helped me a lot 
tremendously in many ways, helping me achieve this feat. They've helped regarding many stuff, whether it be my highs 
and lows, with my self doubt or over self-esteem, I can't thank them enough:
- [TJNull][TJNull]
- [Berk Cem Göksel][BCG]
- [goeo_][goeo]
- [CarrotCake][CarrotCake]
- [initinfosec][initinfosec]

[OSCP Certificate]:     images/2021-09-12-the-end-of-my-oscp-pwk-journey/certificate.jpg
[TJNull]:               https://twitter.com/TJ_Null
[TJNull's Blogpost]:    https://www.netsecfocus.com/oscp/2019/03/29/The_Journey_to_Try_Harder-_TJNulls_Preparation_Guide_for_PWK_OSCP.html
[Noraj Github]:         https://github.com/noraj/OSCP-Exam-Report-Template-Markdown
[Offensive Security]:   https://www.offensive-security.com/pwk-online/PWKv1-REPORT.doc
[OSEP]:                 https://www.offensive-security.com/pen300-osep/
[OSWE]:                 https://www.offensive-security.com/awae-oswe/
[CRTO]:                 https://www.zeropointsecurity.co.uk/red-team-ops/overview
[eCPTX]:                https://elearnsecurity.com/product/ecptx-certification/
[HackTricks]:           https://book.hacktricks.xyz/
[BCG]:                  https://twitter.com/berkcgoksel
[goeo]:                 https://twitter.com/goeo_
[CarrotCake]:           https://twitter.com/carrotcakesec
[initinfosec]:          https://twitter.com/initinfosec

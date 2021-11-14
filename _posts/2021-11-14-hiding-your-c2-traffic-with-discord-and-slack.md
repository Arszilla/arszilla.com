---
title: Hiding Your C2 Traffic With Discord & Slack
excerpt: A short blog post revisiting my IsolationCon 2 talk about using Discord & Slack as a way to hide your C2 framework traffic
date: 2021-11-14 23:30:00 +03 GMT
categories: Red Teaming, C2, Discord, Slack
---

## Introduction
On April 24 2021, [Berk Cem Göksel][Berk Cem Göksel] and I presented "Hiding Your C2 Traffic Under Discord & Slack 
Traffic" at [IsolationCon 2][IsolationCon2]. There, we talked about using Discord or Slack's API and bot 
functionalities in order to hide your C2 traffic from curious eyes.

We talked about the advantages and disadvantages of such a concept alongside our proof-of-concepts, 
[SierraOne][SierraOne] and [SierraTwo][SierraTwo], where we developed a basic yet functional C2 framework on Discord 
and Slack.

Since the Twitch vod has been lost to the time and the talk hasn't been uploaded to YouTube, I decided to turn it into 
a blog post after 7 months (apologies for the long delay), making the talk accessible to everyone in text.

## Why?
Now, the first question that might come into the mind is: "why?", which is a valid question. Typically, APT groups and 
red teamers use mallable C2 profiles that seem less suspicious when inspected up close, but in reality, if a network 
administrator look a proper look at the traffic for the device in question, they might notice the suspicious traffic 
and alert their SOC team if it hasn't been detected yet.

By using Discord and/or Slack as a C2 framework, your traffic is:
1. Encrypted
2. Does not raise much suspicion in most networks thanks to the increasing use of such programs in a post-COVID-19 
workplace

Most workplaces I know tend to use Slack or Microsoft Teams, but I've seen some workplaces use Discord as well.

## Infrastructure and Traffic
If we were to take a look at a traditional C2 infrastructure, we'd see an image similar to the one below:

![Traditional C2 Infrastructure][Traditional C2 Infrastructure]

Here, we can see that since the C2 server is "out in the open". If a  blue teamer was to discover the presence of such 
a traffic in their network, they will be able to IoC and take appropriate actions to prevent the attacker from 
persisting their connection, if not establishing other connections in their network

But by using Discord and/or Slack as a C2 framework, we turn the image above into the image below:

![New C2 Infrastructure][New C2 Infrastructure]

Notice how instead the "C2 Server" has been replaced by Discord and Slack. Backtracking to what I said, in my previous 
paragraph, if we were to take a look at a standard PowerShell Empire HTTP traffic in a network, we'd see images 
similar to the ones below: 

![Empire HTTP Tunneling 1][Empire HTTP Tunneling 1]

![Empire HTTP Tunneling 2][Empire HTTP Tunneling 2]

Since this is the default HTTP listener, it's open to customization. Thus, an attacker might use a mallable C2 profile 
that's ready-to-go or available on their C2's panel, making it less obvious, but this still doesn't hide the fact that 
the system's traffic is going to an "odd" location. Given the system is using a decent EDR/XDR or SIEM solution, they 
might have the capability to detect anomalous traffic from the said system, thus allowing a blue teamer to detect such 
traffic and generate IoCs to prevent further actions from being performed on the system.

If we were to compare the images above to `SierraOne`'s traffic, we'll see a major difference:

![SierraOne Traffic 1][SierraOne Traffic 1]

![SierraOne Traffic 2][SierraOne Traffic 2]

Similarly, the same difference applies to `SierraTwo`'s traffic:

![SierraTwo Traffic 1][SierraTwo Traffic 1]

![SierraOne Traffic 2][SierraTwo Traffic 2]

As seen in the images above, the C2's traffic is hidden in plain sight, under Discord and/or Slack's traffic. When 
compared to a normal Discord client's (or Slack) traffic from the same system, it'd be hard to spot the difference or 
call the traffic anomalous:

![Benign Discord Traffic 1][Benign Discord Traffic 1]

![Benign Discord Traffic 2][Benign Discord Traffic 2]

## Concepts Brought to Life: SierraOne and SierraTwo
`SierraOne` and `SierraTwo` are two proof-of-concepts Berk and I developed were published in July 2020. Both of these 
proof-of-concepts were written in Python, had similar functions and were packaged with `PyArmor` for obfuscation. Even 
though Discord's API is more stable than Slack's, which changed it's API as we were developed `SierraTwo`, we thought 
that by the time we'd demonstrate these two PoCs in a talk one or more of the following scenarios would've happened:
a. Easily detected by Discord or Slack (Just like how Dropbox easily detects PowerShell Empire usage on their end)
b. The accounts, servers, and bots we used would've been destroyed by the respective companies
c. Windows Defender, VirusTotal, etc. would've easily managed to detect the executables
d. The API's would be updated, bringing dramatic changes (*ahem Slack ahem*)

But to our surprise, none of these happened (minus a small change to Slack's API, which I expected).

After a small fix to `SierraTwo` and a "download" functionality to both projects, we were able to generate new 
binaries to demonstrate in our talk. Below, you can see 2 videos demonstrating these proof-of-concepts:

<div align="center">
<iframe 
    width="560" 
    height="315" 
    src="https://www.youtube.com/embed/6rKYL3JHDE4" 
    title="SierraOne" 
    frameborder="0" 
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
    allowfullscreen>
</iframe>
</div>

<br>

<div align="center">
<iframe 
    width="560" 
    height="315" 
    src="https://www.youtube.com/embed/08uziyDVAqk" 
    title="SierraTwo" 
    frameborder="0" 
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
    allowfullscreen>
</iframe>
</div>

In terms of VirusTotal detection, when I initially submitted the samples to VT on April 18th, we saw:
- 3/67 detection for `SierraOne`
- 2/67 detection for `SierraTwo`

Since then, I checked the scans today (2021-11-14) and saw that these numbers have changed a bit:
- 5/67 detection for `SierraOne`
- 3/67 detection for `SierraTwo`

If you're interested in taking a look at the VT scans:
- [SierraOne VirusTotal][SierraOne VirusTotal]
- [SierraTwo VirusTotal][SierraOne VirusTotal]

## The Advantages & Disadvantages
Now, given that these API's have existed for years with Python libraries available or them, you might be asking "then 
why has no one used them?", which is another valid question. Both the method and implementation have some advantages 
and disadvantages worth mentioning:

### Method
#### Advantages
1. The traffic is hidden behind a well known, public service that might be used in the workplace
2. Only API keys and/or user IDs and/or server IDs are present instead of a domain or an IP
3. Much harder to generate mitigations based on IoCs

#### Disadvantages
1. Dependencies might change unexpectedly (Especially Slack's API is prone to this)
2. Confidentiality (For example Discord keeps a copy of all your messages and images etc.)

### Implementation
#### Advantages
1. Projects are easy to setup
2. Agents work on both Windows and Linux
3. Cross-compilation is possible on Kali Linux (or Linux in general)
4. Obfuscated (via PyArmor)
5. Fun to use with friends and colleagues

#### Disadvantages
1. The executable has to be signed in order to bypass Windows SmartScreen
2. A new API key will be needed for each shell (i.e. each unique agent)
3. The API key will be present in the executable
4. No terminal, less l33t
5. "Get jailed" speedrun attempt (possible WR)

Given the 3rd disadvantage in implementation, you might be prone to API key hijacking. In other words, if someone 
manages to get your API key by reverse-engineering your binary (or by some other means), they'll have access to your 
C2 server, thus destroying your operation. A mitigation for this is an implementation of a crypto scheme, but since 
`SierraOne` and `SierraTwo` were just proof-of-concepts, Berk and I didn't feel the need to do it ourselves.

## Takeaways
A few takeaways from this concept could be summed up to the following:
1. Write your own payloads (You may not want to use Python, it's a personal preference)
2. Workplace application traffic is contrastingly less suspicious
3. Discord and Slack bots rarely get banned (Despite the project being published 1.5 years ago, our Discord bots were 
still active and fully functional)
1. Using API's functionality, such as "file download", would help evade detection in a possible use (LotL)
2. Slack's API and documentation is horrendous

Additionally, Berk and I have 2 additional takeaways for those interested in this concept:
1. **DO NOT** use Sierra (as is) in actual engagements
2. Be responsible

[Berk Cem Göksel]:                  https://twitter.com/berkcgoksel
[IsolationCon2]:                    https://www.tmhcisolationcon.com/
[Sierraone]:                        https://github.com/berkgoksel/SierraOne
[SierraTwo]:                        https://github.com/berkgoksel/SierraTwo
[Traditional C2 Infrastructure]:    images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/traditional-c2-infrastructure.png
[New C2 Infrastructure]:            images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/new-c2-infrastructure.png
[Empire HTTP Tunneling 1]:          images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/empire-http-tunneling-1.png
[Empire HTTP Tunneling 2]:          images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/empire-http-tunneling-2.png
[SierraOne Traffic 1]:              images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/sierraone-1.png
[SierraOne Traffic 2]:              images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/sierraone-2.png
[SierraTwo Traffic 1]:              images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/sierratwo-1.png
[SierraTwo Traffic 2]:              images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/sierratwo-2.png
[Benign Discord Traffic 1]:         images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/benign-discord-traffic-1.png
[Benign Discord Traffic 2]:         images/2021-11-14-hiding-your-c2-traffic-with-discord-and-slack/benign-discord-traffic-2.png
[SierraOne VirusTotal]:             https://www.virustotal.com/gui/file/6c77ed9ccc7a63bce5af149b784f36e97f349f43d4aad08fbcfb31cc94bb9530/detection
[SierraTwo VirusTotal]:             https://www.virustotal.com/gui/file/8abf5ecbca27daa56d2b8d47ff764632e4b4b51d3d0c73584bfc16136b91b9a7/detection

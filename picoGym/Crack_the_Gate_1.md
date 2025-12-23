# Crack the Gate 1
 
**Date**: 12-22-2025
**Author**: Lukas Somwong  
**Platform**: PicoCTF PicoGym
**Category**: Web

## Solution

Source file for page contained comment " <!-- ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf" --> "
which is encryped using the Caesar Cypher. Decrypting gives the message "<!-- NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes" -->".
Changing the heder in login request as described provides a response containing flag.

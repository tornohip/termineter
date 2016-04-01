## Frequently Asked Questions ##
### Will Termineter integrate with Metasploit? ###
No, Termineter will not integrate with Metasploit.  Because of the highly specialized nature of the application there is no need to integrate with Metasploit at this time.

### Will Termineter work with Non-ANSI Meters? ###
No, Termineter will only support meters that conform to the ANSI standards, specifically ones that support C12.18 and C12.19.

### Can Termineter read how much power is being used? ###
Technically, yes if the tables can be accessed.  The information would however be raw and unparsed.  Because Termineter was designed with a focus on the need for a security orientated tool, most consumer-related features have not been fully developed.  This may change at a later point in time as development continues.

### Something isn't working where can I get help fixing it? ###
Please start termineter with logging set to debugging, use "-L DEBUG" when starting it from the command line. Then attempt to reproduce the problem and submit a ticket to the [issues tracker](http://code.google.com/p/termineter/issues/list).
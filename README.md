# Make Me Admin #
Make Me Admin is a simple application for Windows that allows standard user accounts to be elevated to administrator-level, on a temporary basis.

You can find documentation in the [wiki](https://github.com/pseymour/MakeMeAdmin/wiki).

The original code repo is here: https://github.com/pseymour/MakeMeAdmin

The software is very impressive and could be a great improvement to our environment. 

## Our MIS ARTG fork ##

We have forked this repo because I noticed that the Make Me Admin program wasn't working as expected in our environment, and I think I could make changes to make it work for us. I believe this is due to misconfiguration or some sort of difference with the systems I tried it on, not an issue with the software itself. 

More specifically, in my poking around, I discovered that some of the functions were returning the identity of the `NT AUTHORITY\SYSTEM` user, rather than the identity of the logged-in user, for evaluation for privilege escalation. I _think_ it may have been doing that because the `SYSTEM` user was running the service, but that's merely a first guess on my part. There could be a larger misconfiguration/configuration problem on my end. 

I am hopeful that if I can make some minor changes to force the calling function to send the identity of the logged-in user, that I can make it work on our test systems, then I can do broader testing from there. 

I also plan to make minor changes to the service name/GUI name to designate what's running is our fork's binaries, as not to confuse myself while testing (or in production), since I've already run into that issue where I think I'm running one build of Make Me Admin, but I was running a different one. 

-JDM

### Usage ###

Install this forked version's binary from the [/Installers](https://github.com/misartg/MakeMeAdmin/tree/main/Installers) in our repo. 

For en-US x64, it's right here: https://github.com/misartg/MakeMeAdmin/blob/main/Installers/en-us/MakeMeAdmin%202.3.0%20x64%20Debug.msi

#### Silent installation ####

We install this silently with `msiexec /i "MakeMeAdmin 2.3.0 x64 Debug.msi" /qb /norestart /l* "%TEMP%\MakeMeAdmin-install.log"`

#### Silent uninstallation ####

Since we were testing a lot, we did a silent uninstallation. It seemed important to do this before re-installation on our test systems. 

Running `wmic product where "name = 'Make me Admin'" call uninstall /nointeractive` seemed to work well for us. 

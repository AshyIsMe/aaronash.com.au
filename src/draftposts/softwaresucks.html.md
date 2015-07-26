```
title: Software Sucks
layout: post
tags: ['software', 'Linux', 'OSX', 'windows', 'post']
```

# Software Sucks

This post is a bit of a vent about all the software annoyances I run into on a regular basis.
Some of these are minor annoyances but at the same time shipping software with these issues is pretty piss-poor of us as an industry.

Language warning: I made most of these notes in the heat of the frustrated moment when I hit them again for the Nth time.  I haven't edited out the language.

## Windows Server 2012
- A fucking touch interface on a server OS?
-- I dunno what world microsoft lives in but a touch interface on a server os is highly unlikely to help sysadmins to automate tasks or diagnose problems on their servers.  Modern convenient tablet usage hardly makes sense for a server OS...
- You can't drag and drop a powershell file into the Powershell ISE to open it for editing.
-- I'm actually struggling to believe this one myself looking back, TODO: confirm this and make a video of it.


## OSX 
- Why does the whole gui of OSX freeze every now and then when Time Machine spins up my usb drive?
On the whole I'm pretty happy with OSX as an everyday local machine OS.  The default window management isn't the best but there is plenty of cheap apps to fix that (I use BetterSnapTool currently).  It's rare that I run into issues on OSX that I consider bugs but this is one of them.
It makes very little sense in a modern OS for the whole gui thread to pause when a background process tries to access an external HDD that has gone to sleep.  The accepted work around seems to be "turn off hdd sleeping".  Apparently this is better for the longevity of the drives anyway but this is still a ridiculous issue in the first place.  TODO: link to the work around threads.


## Microsoft Office on Mac
MS Office on the mac, I barely know where to start with this.  Almost every app in the suite has a huge amount of issues.  As far as I can tell, Microsoft got it to an alpha version, released it and called it a day.
- Lync is unusable.  I can sign in but if I send a message or try to call anyone in my organisation I'm signed out instantly.  From the googling I've done it seems like this is probably a configuration issue in the installation at my work but the fact that it works fine on any of my windows machines shows how much of a second class citizen the mac is to MS.  I can somewhat understand it as macs are hardly a huge part of the market share for corporate organisations.  But at the same time, this is software sold by Microsoft, one of the top 3 names in the IT industry.  As a developer it shames me that this is the contempt one of our leading names shows for their customers. 
-- Crashes often, especially when switching audio devices
-- The historical conversation interface is terrible
- Word - wtf is with not using mac hotkeys for simple things?
- Outlook - Best of a bad bunch but still crashes often
-- Open an excel attachment directly from an email. Make some changes. File -> Save As:
          - "untitled.xls"  <- Why the fuck doesn't this default to the name of the existing document?  You then have to manually type the fucking name aorecugraectouhnsatodheusnth
-- Can't open a *.msg file on mac.
--- Seriously wtf: http://apple.stackexchange.com/questions/36508/opening-msg-file-in-outlook-for-mac-2011
-- Oh yeap, email rules aren't safe either: <a href="/images/OutlookError.png"> <img src="/images/OutlookError.png" class="img-responsive"> </a>
---- AA TODO: add in jackie chan wtf meme


## Spotify
- Why does my mouse back button skip back a track when im always trying to just browse back a page?
This one is pretty minor as overall spotify is a pretty sweet service.  An affordable monthly fee for almost all the music I ever listen to, on all of my devices.  This is exactly what we always wanted. (Although, if you're listening spotify, it'd be great if you also offered lossless quality as an option.  I certainly don't need it on my phone in the car but lossless is noticably better on my expensive headphone setup at home).


This post might come across as jsut whinging, and it is me having a whinge.  But I really think we as an industry need to pick up our game.  Having these issues from the major names in the industry is pretty damn terrible.

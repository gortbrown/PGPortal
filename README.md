# PGPortal
An easy way for people to send you PGP encrypted messages!

PGPortal is a self-hosted, PGP encrypted contact portal that makes it easy for people to send you information in a private and secure manner.

## How it works
PGPortal uses [openpgp.js](https://openpgpjs.org) to encrypt information entered into the form using your supplied PGP public key. Then, it will send the resulting encrypted message to your supplied email using [SmtpJs](https://smtpjs.com). The current implementation has this set up as a dedicated site, but this can be easily integrated into any webpage!

## Supported data
Right now, you can only send text through PGPortal, but we are currently working on support for encrypting and sending files.

## Setup
PGPortal can be run as a static website. It can be hosted on any webserver (something running apache, nginx, etc.) static hosting platform (Github Pages, Netlify, etc.) or in a Solid Pod. Please refer to your server's/provider's documentation on how to set it up. For Solid Pods, just upload the files to a folder in your pod, and make the folder public.

If you are integrating it to an existing webpage, just add the html code (modify it all you want if you wish!) 

## Configuration
To configure your PGPortal setup, you will need to do a few things:
-Supply your public key
-Set up an smtp server
-Configure SmtpJs's Send function to use it

### PGP Configuration
In ```index.html```, there will be a variable in the ```<script>``` area called ```keyArmored```. This is where you will put your PGP public key. To get your public key in a text format, open the Terminal (Linux, MacOS, Windows 11) or PowerShell (Windows 10 and older) and type in ```gpg --list-keys``` to get your fingerprint. It should be the line below the line that starts with 'pub.' From there, type ```gpg --export -a <your fingerprint here>```. The result will be your public key. Copy that and put it between the ``` `` ``` marks next to ```keyArmored```


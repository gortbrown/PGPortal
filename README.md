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
In ```index.html```, there will be a variable in the ```<script>``` area called ```keyArmored```. This is where you will put your PGP public key. To get your public key in a text format, open the Terminal (Linux, MacOS, Windows 11) or PowerShell (Windows 10 and older) and type in ```gpg --list-keys``` to get your fingerprint. It should be the line below the line that starts with 'pub.' From there, type ```gpg --export -a <your fingerprint here>```. The result will be your public key. Copy that and put it between the ``` `` ``` marks next to ```keyArmored```.

### Smtp Server
The next step is to set up an smtp server. If you already have one, you should be able to use that. The only issue might be that SmtpJs only allows certain smtp servers, like [ElasticEmail](https://elasticemail.com). I'm not sure if there would be an issue with a personal one, but I know smtp servers like gmail aren't allowed, so if yours doesn't work, SmtpJs will let you know, and you can set up a free trial for ElasticEmail, which allows for 100 emails a day.

If using Elastic Email, the process is fairly simple. Once logged in, choose the "Connect to SMTP API" option, then click "Create". It will ask you for a username, which should just be whatever email you are sending from. Click "Create" and a popup will give you your password. Copy this down, as this will be important.

Next, to to [smtpjs.com](https://smtpjs.com), and you can copy one of two bits of code: The one at the top that includes ```username``` and ```password``` options, or the more secure option a bit further down on the page. If you choose the first one, just copy it over to ```index.html```, put the username, password and the url of the smtp server (smtp.elasticemail.com for Elastic Email) in there and update the rest of the options. If you choose the secure option, copy the other code over to ```index.html```. Then click "Encrypt your SMTP Credentials". There, you will enter your username and password, as well as the url of your smtp server, and a url in the "Domain" section, (I just put deploy.netlify.app.) From there just put the secure code into the SmtpJs code, and update the rest of the information.

## Security
Here is an exerpt from the [openpgp.js Github](https://github.com/openpgpjs/openpgpjs) that explains some recommendations regarding security. I figured it would be best to just copy it since they said it better that I probably could.

>It should be noted that js crypto apps deployed via regular web hosting (a.k.a. [host-based security](https://www.schneier.com/blog/archives/2012/08/cryptocat.html)) provide users with less security than installable apps with auditable static versions. Installable apps can be deployed as a [Firefox](https://developer.mozilla.org/en-US/Marketplace/Options/Packaged_apps) or [Chrome](https://developer.chrome.com/apps/about_apps.html) packaged app. These apps are basically signed zip files and their runtimes typically enforce a strict [Content Security Policy (CSP)](https://www.html5rocks.com/en/tutorials/security/content-security-policy/) to protect users against [XSS](https://en.wikipedia.org/wiki/Cross-site_scripting). This [blogpost](https://tankredhase.com/2014/04/13/heartbleed-and-javascript-crypto/) explains the trust model of the web quite well.
>
>It is also recommended to set a strong passphrase that protects the user's private key on disk.

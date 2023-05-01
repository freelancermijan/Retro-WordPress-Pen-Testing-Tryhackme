<h1 align="center">:fire: TryHackMe Retro room :fire: </h1>

![preview](images/01-Screenshot.png)<br/>

TryHackMe [Room link](https://tryhackme.com/room/retro)<br/>


Copy your tryhackme machine ip and hit on browser. It will show like this.<br/>
![download](images/02-windows-server.png)<br/>

## Task 1: Pwn
### Q: A web server is running on the target. What is the hidden directory which the website lives on?

You need to run this command on dirsearch tool<br/>

    sudo dirsearch --full-url -w /usr/share/wordlists/Seclist/dirb/directory-list-2.3-medium.txt -u http://10.10.126.180/

After successfully run this command you'll found hidden directory called `retro`.<br/>
![download](images/03-found-hidden-directory.png)<br/>

#### A: `/retro`

### Q: user.txt

As you can see, it's detected WordPress CMS<br/>
![download](images/04-Wordpress-cms-detected-with-wp-version.png)<br/>

Also we found WordPress default admin login page.<br/>
![download](images/05-found-wp-default-login-page.png)<br/>

Now start to scan with wpscan tool for enumerate users of target website.<br/>

    wpscan --url http://10.10.126.180/retro -e u

![download](images/06-scan-with-wp-scanner-tool.png)<br/>

We found a user called `wade`<br/>
![download](images/07-found-user-named-wade.png)<br/>

We found a credintials in the blog post section. `parzival`<br/>
![download](images/09-guess-password-for-wade-user.png)<br/>

Successfully logged in WordPress Dashboard.<br/>
![download](images/10-logged-in-by-wade-user.png)<br/>

Now use any Remote desktop protocol to login, I'm using [Remmina](https://remmina.org).<br/>
![download](images/15-give-credintials-for-login.png)<br/>

After login in remote PC I've found a `user.txt` file in desktop.<br/>
![download](images/16-logged-in-and-found-user-flag.png)<br/>

#### A: `3b99fbdc6d430bfb51c72c651a261927`

### Q: root.txt
Now click on little `search` icon on remote windows PC and type `cmd` hit `Enter`. It will open command prompt. Now type this command and see about system information.<br/>

    systeminfo

![download](images/17-open-cmd-to-see-info.png)<br/>

Now go to this link [Exploit](https://github.com/SecWiki/windows-kernel-exploits/blob/master/CVE-2017-0213/CVE-2017-0213_x64.zip). And download the zip file and extract it.<br/>
![download](images/18-take-karnel-exploit-and-transfer-on-remote-windows.png)<br/>

Open python server on your local machine where you extracted `CVE-2017-0213_x64` file.<br/>

    sudo python3 -m http.server --bind your.tun0.ip 8080

![download](images/19-create-a-python-server-on-local-machine.png)<br/>

It'll generate a link. Now copy the link and hit on remote PC browser. After that download EXE file and make sure Keep it to continue downloading.
![download](images/20-download-on-remote-PC.png)<br/>

After downloading completed click to run this program. And type command below to get root flag.

    cd /
    cd Users/Administrator/Desktop
    type root.txt.txt

![download](images/21-root-flag.png)<br/>

#### A: `7958b569565d7bd88d10c6f22d1c4063`

## Finished!!
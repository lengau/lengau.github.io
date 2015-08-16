---
layout: post
title:  "What sudo does and why you shouldn't use it"
categories: linux, terminal, commandline, cli, sudo
---

Occasionally, I stumble upon [posts][raspi-tutorial] that include commands that
are… to say the least, weird. They normally cover their purpose fairly well, but
they often make some common mistakes, spreading these antipatterns to new users.
One of the most common has to be abuse of [`sudo`](http://www.sudo.ws/).

Before going any further, I'd like to discuss what sudo does.

## What sudo does

When asked, most people will tell you "it lets you run stuff as root". And
whilst that's certainly true and is probably the most common usage, it's not
all sudo does. For example, did you know you can run commands as another
(non-root) user with sudo? 

### Run as another user

Let's say I've got an ftp server on my machine, which I use only on my home
network. I want anyone on my home network to be able to read and write my ftp 
directory without logging in. The directory is /srv/ftp and has permissions
like this:

    lengau@hyperion:/srv/ftp$ ls -ld
    drwxr-xr-x 1 ftp nogroup 30 Aug 16 11:06 .
    
Obviously, I can't just write a file in there, since I don't have write access:

    lengau@hyperion:/srv/ftp$ touch dis
    touch: cannot touch ‘dis’: Permission denied

But I have sudo access, so the answer is obvious, right? Just write it as root!

    lengau@hyperion:/srv/ftp$ sudo touch dis
    lengau@hyperion:/srv/ftp$ ls -l dis
    -rw-r--r-- 1 root root   0 Aug 16 11:10 dis

Well that worked. Now anyone can see the contents of my new file. But the
anonymous user on my FTP server has the same permissions as the local user 
`ftp`. So I can't write to it over ftp. Well, since i have the file, the answer
is simple:

    lengau@hyperion:/srv/ftp$ sudo chown ftp:nogroup dis
    lengau@hyperion:/srv/ftp$ ls -l dis
    -rw-r--r-- 1 ftp nogroup 0 Aug 16 11:10 dis
    
Okay. But that required me to enter a second command, and unless I had been
paying attention to the permissions in the first place, I wouldn't have noticed.

What if I create the file as the ftp user? That's essentially the same as
creating it as a remote user, so I shouldn't have to mess with permissions and
ownership;

    lengau@hyperion:/srv/ftp$ sudo -u ftp touch dat
    lengau@hyperion:/srv/ftp$ ls -l dat
    -rw-r--r-- 1 ftp nogroup 0 Aug 16 11:16 dat

That was easy.

### Safely edit files

How would you edit files as another user?

If you're like most people I know (and my bash history tells me I haven't yet
managed to fully break this habit either), you just use sudo to run your
favourite text editor as root. A command like `sudo ed mytext` might be fairly
common in your command history. And for ed, that might be okay. But what if the
command is `sudo emacs mytext`? Or `sudo vim mytext`? Or 
`sudo some-notoriously-insecure-java-app mytext`?

And what if you downloaded `mytext` from http://suspicious-site.con?

Well I don't have a magical solution that'll prevent the security vulnerability
from becoming a problem at all (although sandboxing might help), but we can
limit the damage it's likely to do. For a start, let's not run any applications
as root unless we really need to. And you don't need to run your text editor
as root.

There's a convenient command called `sudoedit` that lets you edit a file
as root. (You can also run `sudo -e` to achieve the same effect.) It's a fairly
simple, but extremely powerful program that simply copies the referenced file
to a temporary file with permissions for you to edit it, starts your default
text editor on the file, and, once the editor exits, updates the original
file with the changed contents of the edited file.

So if we have this file:

    lengau@hyperion:/tmp$ ls -l CANT_TOUCH_DIS 
    ---------- 1 root root 0 Aug 16 11:48 CANT_TOUCH_DIS
    
I can edit it using:

    lengau@hyperion:/tmp$ sudoedit CANT_TOUCH_DIS 

When I do so, it opens my default text editor to `/var/tmp/CANT_TOUCH_DIS.XXfWyMR6`.
Let's take a look at that file:

    lengau@hyperion:~$ ls -l /var/tmp/CANT_TOUCH_DIS.XXfWyMR6 /tmp/CANT_TOUCH_DIS 
    ---------- 1 root   root   0 Aug 16 11:48 /tmp/CANT_TOUCH_DIS                                                        
    -rw------- 1 lengau lengau 0 Aug 16 11:48 /var/tmp/CANT_TOUCH_DIS.XXfWyMR6
    
So I can access the temporary copy of the file, and the best part is that my
editor only ever needs to run as me. I can keep all of my customizations, and
if I really want, I can use a graphical editor.

![Screenshot of sudoedit process tree. The editor process is a child of sudoedit
and is running as the same user that bash is running as.](/images/sudo-sudoedit-process-tree.png)

As soon as I close my text editor, sudoedit deletes the file.

#### Editing other files

As a side note, you can set what program you want as the editor for sudoedit by
setting the `$SUDO_EDITOR` environment variable. So if you want to edit an
image, you can:

    SUDO_EDITOR=krita sudoedit best_screenshot_ever.png

#### Editing as another user

Like `sudo`, `sudoedit` lets you edit a file as another user rather than as
root. So you can get output like this:

    lengau@hyperion:/etc$ sudoedit -u ftp /etc/shadow
    sudoedit: /etc/shadow: Permission denied
    lengau@hyperion:/etc$ sudoedit -u www-data /etc/shadow
    sudoedit: /etc/shadow: Permission denied
    lengau@hyperion:/etc$ cat /etc/shadow
    cat: /etc/shadow: Permission denied

### Restrict root access

Let's say you're the administrator of a shared machine. Your users may need to
perform certain actions as another shared user. The simple solution is to give
them all the password for that user.

That solution is also wrong.

Instead, you can use sudo to give them access to run some (or all) commands
as that user without giving them root access.

Why would you want this? LOGS! Every time you run a command with sudo, it gets
logged. So when someone messes up the machine as the shared user, they can't
deny what they've done:

    lengau@hyperion:/srv/ftp$ grep sudo /var/log/auth.log|tail
    Aug 16 12:03:55 hyperion sudo:   lengau : TTY=pts/0 ; PWD=/tmp ; USER=root ; COMMAND=sudoedit best_screenshot_ever.png
    Aug 16 12:03:55 hyperion sudoedit: pam_unix(sudo:session): session opened for user root by lengau(uid=0)
    Aug 16 12:04:28 hyperion sudoedit: pam_unix(sudo:session): session closed for user root
    Aug 16 12:06:59 hyperion sudo:   lengau : TTY=pts/0 ; PWD=/etc ; USER=ftp ; COMMAND=sudoedit /etc/shadow
    Aug 16 12:07:07 hyperion sudo:   lengau : TTY=pts/0 ; PWD=/etc ; USER=www-data ; COMMAND=sudoedit /etc/shadow
    Aug 16 12:07:21 hyperion sudo:   lengau : TTY=pts/0 ; PWD=/etc ; USER=ftp ; COMMAND=sudoedit /etc/shadow
    Aug 16 12:07:22 hyperion sudo:   lengau : TTY=pts/0 ; PWD=/etc ; USER=www-data ; COMMAND=sudoedit /etc/shadow
    Aug 16 12:07:52 hyperion sudo:   lengau : TTY=pts/0 ; PWD=/srv/ftp ; USER=ftp ; COMMAND=sudoedit something
    Aug 16 12:07:52 hyperion sudoedit: pam_unix(sudo:session): session opened for user ftp by lengau(uid=0)
    Aug 16 12:07:55 hyperion sudoedit: pam_unix(sudo:session): session closed for user ftp

## Why you shouldn't use it

In the title for this post, I told you that you shouldn't use sudo. Yet I've now
gone on for ages about how useful it is.

What I mean isn't that you shouldn't use sudo at all. All the above text shows
just how useful it can be when use correctly. But far too many people overuse
sudo or use it badly. So here are a few do's and don'ts for sudo:

### Do's
* DO use `sudoedit` when you need to edit a file as root or another user. 
  (Unless that file is your sudoers file, in which case PLEASE PLEASE PLEASE 
  use [visudo][man-visudo].)
* DO use `sudo` when you need to run a command as another user.
* DO use `sudo` to give out administrator access instead of handing out root 
  passwords or having shared accounts.
* DO read the [sudo manual pages][man] to find out just what sudo can do for you.

### Don't's
* DON'T edit a file by running the editor with `sudo`. Use `sudoedit` instead.
* DON'T use `sudo` to gain more permissions than you need. Always try to run
  any command as the least-privileged user with access to do so.
* DON'T give out `sudo` access unless you know precisely what access that is.



[man]: http://www.sudo.ws/man.html
[man-visudo]: http://www.sudo.ws/man/1.8.14/visudo.man.html
[raspi-tutorial]: http://www.circuitbasics.com/how-to-write-and-run-a-python-program-on-the-raspberry-pi/

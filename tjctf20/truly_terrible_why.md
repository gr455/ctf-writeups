**Problem Statement:**
Your friend gave you a remote shell to his computer and challenged you to get in, but something seems a little off... The terminal you have seems almost like it isn't responding to any of the commands you put in! Figure out how to fix the problem and get into his account to find the flag! Note: networking has been disabled on the remote shell that you have. Also, if the problem immediately kicks you off after typing in one command, it is broken. Please let the organizers know if that happens.

nc 52.205.246.189 9000

**Hints**
Think about program I/O on linux


connecting to the netcat given, i was greeted with a shell which had stdout and stderr disabled. But since I was able to put in commands, I figured stdin is available, so I piped all the outputs and errors from the shell into stdin
`ls >&0` output pipe
`wtfisthiscommand 2>&0` error pipe

To achieve this, I opened 2 shells (`/bin/bash`) one piping output and other shell(opened through the first one) piping errors.
This way I had a functional shell.

After that, ls gave this:

`message.txt  password.txt`

*message.txt*:

'Man, I just sure wish I could somehow impersonate other_user...  

I wonder what I need to do'

*password.txt*:

'password for problem-user: 1234qwer'

So i figured, I had to switch user to other-user from problem-user.
None of the `sudo`, `su` commands worked because the shell was not tty.

So I used `python -c 'import pty; pty.spawn("/bin/sh")'` to spawn a tty shell.
Here, sudo was extremely limited. The user had near to no permissions to do anything as root.

So i did `sudo -l` followed by the password and it gave this:

'User problem-user may run the following commands on ctf-challenge:
    (root) /usr/bin/chguser'

executing `sudo chguser` followed by the password changed my user to other-user. Then I just cd'd my way to the flag.

**FLAG:** tjctf{ptys_sure_are_neat}



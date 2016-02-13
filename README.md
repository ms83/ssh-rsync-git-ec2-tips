# ssh-rsync-git-ec2-tips
How to use ssh, git, rsync with nonstandard sshd setup (i.e. EC2)

When you buy standard Amazon EC2 server instance, the way you __ssh__ to the server is:

```
ssh -i your-ec2.pem ec2-user@your-ec2-hostname
```

If you initialize Git repository on EC2 server and want to __git clone__ via ssh to you localhost, you need to create ssh wrapper and set GIT_SSH:

```
$ cat ec2ssh.sh 
#!/bin/sh
exec /usr/bin/ssh -i your-ec2.pem "$@"

$ GIT_SSH="./ec2ssh"
$ git clone ssh://ec2-user@your-ec2-hostname:your-project.git
```

If you want to __rsync__ you localhost data with EC2 server:

```
$ rsync -e "ssh -i your-ec2.pem" dir/ ec2-user@your-ec2-hostname:dir/
```

If you need to convert __Putty__ key into __OpenSSH__ key:

```
$ puttygen your-ec2.ppk -O private-openssh -o your-ec2.pem
```


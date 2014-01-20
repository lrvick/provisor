# Provisor #
<http://github.com/lrvick/provisor>

## About ##

A user provisioning server that wehn combined with some system configuration 
can allow for new users of a shared Linux system be able to set up accounts for
themselves.

The target use case is for a hackerspace, or community shell service.

## Requirements ##

  * Linux server
  * Python3
  * supervisord
  * netcat (nc)

## Installation ##

The easiest way to install with needed library dependencies is with pip.

(Note: If you have both python 2 and 3 installed, use pip3)

### Stable ###

```bash
sudo pip install provisor
```

### Development ###

```bash
sudo pip install -e git+https://github.com/lrvick/provisor/#egg=provisor
```

## Deployment ##

The target use case for provisor is to be used via a connection to a dummy ssh
account that only exists to create new users.

For example, a user would ssh to new@example.org and be presented with a
series of prompts to collect the needed information to provision their account.

To deploy in this way, do the following:

1. Configure /etc/provisor.conf to your liking
 
2. Add the following to your /etc/supervisord.conf

    ```bash
    [program:provisor]
    command=/usr/bin/provisor
    stderr_logfile = /var/log/supervisord/provisor.err.log 
    stdout_logfile = /var/log/supervisord/provisor.info.log 
    ```

3. Restart supervisord

    ```bash
    /etc/init.d/supervisord restart
    ```

4. Create a new user named 'new'

    ```bash
    useradd new
    ```
5. Give the account no password

    ```bash
    passwd -d new
    ```
6. Add the following to the end of /etc/ssh/sshd_config

    ```bash
    Match User new*
      PubkeyAuthentication no
      PasswordAuthentication yes
      PermitEmptyPasswords yes
      AllowTcpForwarding no
      X11Forwarding no
      ForceCommand nc 127.0.0.1 8090

    ```

7. Restart sshd

    ```bash
    /etc/init.d/sshd restart
    ```

From here you can test creating a new user via:

```bash
ssh new@yourserver.com
```

## Notes ##

  Use at your own risk. You may be eaten by a grue.

  Questions/Comments?

  You can find me on the web via:

  [Email](mailto://lance@lrvick.net) |
  [Blog](http://lrvick.net) |
  [Twitter](http://twitter.com/lrvick) |
  [Facebook](http://facebook.com/lrvick) |
  [Google+](http://plus.google.com/109278148620470841006) |
  [YouTube](http://youtube.com/lrvick) |
  [Last.fm](http://last.fm/user/lrvick) |
  [LinkedIn](http://linkedin.com/in/lrvick) |
  [Github](http://github.com/lrvick/)

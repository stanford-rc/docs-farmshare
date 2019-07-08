---
title: Getting Connected
tags: 
 - ssh
 - mobaxterm
 - putty
 - xquartz
 - x11
 - vnc
---

# Getting Connected

## Secure Shell (SSH)

SSH provides a secure, remote terminal connection to the login nodes. There are a number of *rice* servers providing login service (*e.g.*, *rice01*, *rice02*); the easiest way to connect is using the load-balanced name, *rice.stanford.edu*, which will select one for you according to recent utilization.

### Using SSH on Linux and macOS

Simply open up your favorite terminal program (on macOS you can use the built-in Terminal application or something like [iTerm](http://iterm2.com)) and run the ssh command like so, replacing *sunetid* with your own SUNet ID:

```bash
ssh *sunetid*@rice.stanford.edu  
```

Depending on your environment and configuration you may be prompted for your SUNet ID password, and you'll need to complete your login using [two-step authentication](https://uit.stanford.edu/service/webauth/twostep). On most computers, that's all you need to do.

ssh does have many configuration options, however, and you can customize these in `~/.ssh/config` on your local computer. The following is a minimal config file you can use with FarmShare 2 that may make connecting more convenient (run `man ssh\_config` for a complete list of options).

```
IgnoreUnknown GSSAPIKeyExchange

Host rice rice?? wheat wheat?? oat oat??
  HostName %h.stanford.edu

Host rice rice?? rice??.stanford.edu
  HostKeyAlias rice.stanford.edu

Host wheat wheat?? wheat??.stanford.edu
  HostKeyAlias wheat.stanford.edu

Host oat oat?? oat??.stanford.edu
  HostKeyAlias oat.stanford.edu

Host rice rice.stanford.edu rice?? rice??.stanford.edu
  ControlMaster auto
  ControlPersist yes
  GSSAPIDelegateCredentials yes

Host *
  ControlPath ~/.ssh/%r@%h:%p
  GSSAPIKeyExchange yes
  GSSAPIAuthentication yes
  ServerAliveInterval 60
```

Current [host keys and fingerprints](connecting-keys.md) are published if you'd like to verify them.

### Using SSH on Windows

Windows does not have a built-in SSH client, but a number are available from third parties.

#### MobaXterm

[MobaXterm](http://mobaxterm.mobatek.net) is an SSH client with a built-in X server, making [remote display](#mobadisplay) extremely convenient, and the Home Edition is free for personal use. Detailed [configuration instructions](mobaxterm.md) are available.

#### PuTTY

[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/) is a popular, freely available SSH client for Windows. The default settings are appropriate for most users, so all you need to do is specify the host name and click the Open button to connect. Aat the login as: prompt, enter your SUNet ID, and authenticate by password if asked. A number of configuration options are available, and PuTTY allows you to customize your connection settings and save them as a session. The next time you start the application you can load the saved configuration by selecting the desired session and clicking the Load button.

PuTTY supports GSSAPI authentication as of version 0.61 and will automatically use your existing Kerberos credentials for login, if available. You may also want to enable Kerberos ticket forwarding by checking the **Connection** → **SSH** → **Auth** → **GSSAPI** → **Allow GSSAPI credential delegation** box.

#### SecureCRT

[SecureCRT](https://uit.stanford.edu/software/securecrt) is full-featured, commercial SSH client for Windows and is site-licensed for use at Stanford. Default settings should be appropriate; all you need to specify are the host name and (optionally) your user name (SUNet ID). Like PuTTY, SecureCRT offers a host of configuration options, and you can save a session to make connecting more convenient.

You can enable GSSAPI authentication in SecureCRT's **Session Options** dialog, in category **Connection** → **SSH2**. Make sure **Authentication** → **GSSAPI and Key exchange** → **Kerberos (Group Exchange)** is checked. SecureCRT attempts authentication and key exchange methods in the order listed, so these methods should be moved to the top of their stacks using the arrow buttons to the right of each field.

## Mobile Shell (Mosh)

[Mosh](https://mosh.org) is an alternative to SSH for Linux and macOS clients. It uses SSH for authentication, so you may want to review the suggested [SSH configuration](#sshconfig) above. Mosh has some advantages, including predictive display, which can be useful on high-latency connections, and improved network resiliency. Mosh connections can persist when you switch networks and can even survive putting your computer to sleep.

## Remote Display

Some applications allow (or require) the use of a graphical display. You can connect to *rice*systems using X Windows for remote display, but please remember that resources on these workstations are limited, so only modest applications are supported. If your use-case is CPU- or memory-intensive you should use the VNC method to submit a remote display job to one of the compute nodes.

### Using X11 on Linux

X Window is the native graphical interface on Linux systems, and SSH provides a built-in method for forwarding a remote display to the local computer. You can enable forwarding with the -X flag.

```bash
ssh -X *sunetid*@rice.stanford.edu Then, simply start your application (you can test with something like xeyes).
```

### Using XQuartz on macOS

The version of SSH distributed with macOS supports automatic display forwarding with the -X flag, but macOS does not provide an X Window server by default. Luckily, a port of the X.Org server is availble: [XQuartz](https://www.xquartz.org). You'll need to download and install the software and make sure it's running before you connect.

```bash
ssh -X *sunetid*@rice.stanford.edu   ### Using MobaXterm on Windows
```

MobaXterm *automatically* forwards the display when connecting. Check out the [configuration instructions](mobaxterm.md) documentation.

### Using VNC

VNC is less convenient than X Windows, but is useful for persistent sessions and cases where you need or would like a remote desktop environment. The TurboVNC server is available as a module; you can load it with module load turbovnc. Then, start a session with the vncservercommand.

You may be asked to set a VNC password if this is the first time you have started vncserver. We recommend setting a random 8-character password and using the -otp option to create a random, single-use password for every new session, instead.

```bash
$ vncserver -otp -geometry 1680x1050

Desktop 'TurboVNC: rice05:3 (sunetid)' started on display rice05:3

One-Time Password authentication enabled.  Generating initial OTP ...
Full control one-time password: 11578481
Run 'vncpasswd -o' from within the TurboVNC session or
    'vncpasswd -o -display rice05:3' from within this shell
    to generate additional OTPs
Starting applications specified in /home/sunetid/.vnc/xstartup.turbovnc
Log file is /home/sunetid/.vnc/rice05:3.log
```

Note you can specify the size of the display using the -geometry option. Once the server is running you'll need to create a secure tunnel using ssh to connect to the display. Note the system name and the display number in the output of vncserver (in this example, rice05 and display number 3: rice05:3), as you'll need them for the next step!

Open a new terminal window and make a new SSH connection to the target system. The syntax for creating the tunnel is obscure, and expects port numbers rather than display numbers. The local port is listed first, followed by "localhost," and finally the remote port. You can almost always use 5901 for the local port, but the remote port should be the display number plus 5900! In this example, the remote port is 5900 + 3 = 5903, and so the tunneling syntax to use is: -L 5901:localhost:5903.

```bash
ssh -C -L 5901:localhost:5903 rice05.stanford.edu  
```
Now you're ready to connect! Macs have a built-in VNC client called Screen Sharing, so you can connect by selecting Go → Connect to Server or pressing command-K in the Finder and using the Server Address: `vnc://localhost:5901`. The port number here is the local port used above, and should usually be 5901. You'll be prompted for a password; just use the one-time password from the output of vncserver (11578481 in the example above). If you see a blank desktop you can right-click to open a terminal window.

Connecting on a Windows or Linux machine may require additional software. On Windows the [TurboVNC](http://www.turbovnc.org) client is a popular choice, and there are some clients that have native support for SSH tunneling, which can make connecting much easier (as tunneling is less convenient to configure on Windows SSH clients).

When you're finished with your session: close your VNC client, quit the SSH connection you used for tunneling, and shutdown vncserver using the -kill option followed by a colon and the active display number:

```bash
$ vncserver -kill :3 Killing Xvnc process ID 14722
```
We hope to have an improved process for VNC desktops soon!

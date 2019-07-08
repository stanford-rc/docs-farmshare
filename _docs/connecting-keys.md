# Farmshare 2 SSH Host Keys

FarmShare 2 uses several common SSH keys, so you only have to verify keys a few times (instead of once for each server you connect to). You can also use GSSAPI Key Exchange to safely skip key verification.

When you connect to a system using SSH, but before any passwords are sent, your SSH client does a check to see if it is talking to the correct server. This is known as *host key verification*. The normal method of doing this verification is to display a message containing the hashed host key (known as a *fingerprint*), asking you if that key is correct. Once you confirm that the key is correct, your client should not ask you again, unless the key changes or the list of trusted keys is deleted.

Normally, each machine has its own SSH host key. To make things easier for you, while still maintaining a level of security, we have generated one host key for each type of FarmShare 2 node: There is one SSH host key for login nodes, one for compute nodes, and one for GPU nodes.

You can find the SSH host keys below, as well as key fingerprints in two formats (your SSH client will use one of the two).

## GSSAPI Key Exchange

It is possible to bypass the SSH host key verification step, while still ensuring that you are connecting to the machine you expect! To do this, you will need to be logged into Kerberos, while also using a properly-configured client. Then a different Kerberos protocol and key is used to verify the identity of the host.

On Linux, Mac OS X 10.9 & below, and any Mac OS X using non-system SSH, place the following configuration block in your `~/.ssh/config file`:

```
Host rice*.stanford.edu oat*.stanford.edu wheat*.stanford.edu
GSSAPIAuthentication yes
GSSAPIDelegateCredentials yes
GSSAPIKeyExchange yes
```

This requires a little bit of care (and probably a Host * line at the end) to ensure that you don't mess with the rest of your SSH configuration.

On Windows, you will need to download a [third-party build of PuTTY](https://marcussundberg.com/putty/), one that has GSSAPI key exchange support included.

## Host Keys and Fingerprints

Here are the SSH public keys and key fingerprints for FarmShare v2:

### For the rice login nodes:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILKHpqh9AM+h5dAR10kubaxMTwgsS9weCMUcZDHdT4Cr rice.stanford.edu
256  MD5:   f6:a4:69:2d:08:07:37:d4:1d:62:ce:33:f6:67:f1:c1 rice.stanford.edu (ED25519)
256  SHA256:rdOi2irXvc0RXGp0D1/fEo+A4vSLUC7k1ArtQTOD+V4     rice.stanford.edu (ED25519)

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDU0r1QTae9D+5K34iKuF6gA7NzoSEkfxOj2gz8L/KU+30COU9+nNV00DpnuZHKFlQvWzF+8CSGMdCyeWO3gpKC4QcN+J2IfuRhQvh1mD09dHsc+I/LlDYbUukYse+5sM8jEyQKWqGFbnfhvsMzPz/m3TyKvZpmu3k1ODxy/uTHUiDvgLPynUugVVMcHR8s5JWSr17uvAsdXjPA+C1Vkvoky9xSmOA0y/yND4QTxknpd8gG0YFcAJUE6gequBaP/Mc/TUQbcAtaMgZ7JsA2hoC0aAmW+wlPaq4QK8vvM5Pa/pDDAs3i/jVxCS9STdK+THNckTO/0foyC0MTa0ufIRYGkwTnyKfqQF+0tYdzJ0YmlhyEViXrQJYbwhVJ2nn4XG7UmteVP0bI+RBcv7JhIjGkLZ+oXd4O6OaKSLNyz3+yCmUENAlZbO51bQOI06w0tektyPppOwNxCsPDv3E//lrW2PPgpx+ZMXQDn3PUNmMIqBPIXOT0ej/D1Xp+bOOcXWk= rice.stanford.edu
​3072 MD5:   e4:bf:89:7d:54:c4:ec:de:83:86:d8:76:ff:d5:a3:7d rice.stanford.edu (RSA)
3072 SHA256:vsNgw9dGLWNWQVpWKtl8WxJpKH7yWQEB1vUq76LF4R8     rice.stanford.edu (RSA)
```

### For the wheat compute nodes:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAwl7Vy/xrQNF1gkf2D4YbOiSNdHVZZdO1B0ZOg93ZMo wheat.stanford.edu
256 MD5:   96:8f:95:6c:08:c0:7f:66:50:a2:3b:74:19:77:03:80 wheat.stanford.edu (ED25519)
​256 SHA256:hb+URFBJtnI66u8tJ884R6NYNoUWOUnZ9SY5p27/UR8     wheat.stanford.edu (ED25519)

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC6obzawQGg2a0el5/UmXF5Rax+iTX9MtblC3JqPDrqPZcDsnjtmxEJZpVVKmA0yUJTK3uB+An2m37uXxzcptm2DkzEL4GtA4KbZCqS/LXMszgihvssqDvHf/cDBL/7ewYo60K7vTRwCrXjJoOvsLkn3Qo5bTjUwO+lxtZsbndV3c+t6cBTPFChZQCt3oZXX+AOUdLxHyLvKEuknncd0w8EelnUcQIGsrVq9vPOVDsvc+7dWXc85XlI2RWdy/RQ9thP4QOD3/nWHSmt+XsFAMyPT9+tk7t6j309X1JZ7LDrR4PcPKdAV8Di18nhIZoCPvr8ZPLn1Ry69nnRiMIvCZ1UI+fgHd1c+qChqfaRc1tjrUwDuvd1fjZuPGBAtfYROTUTOU828FUddyqvZYwiNkxvbLo8uZGiKv/s6of1uMlY8EzrVXdNu3NEHiY2kxjRfkWQyNF+e4o+gVUYTaAdn6eqT41mY4EOJOhzhFwqhdPs6E/dxHuQxuRIvjQ1CZGOacE= wheat.stanford.edu
​3072 MD5:   05:f4:a8:dd:ec:84:d6:ee:f6:1d:fa:2e:5a:39:ad:48 wheat.stanford.edu (RSA)
3072 SHA256:Wc6M+ytUns/6aQQ7oGwhmUvTirh2F2+bC5XwCW82S1U     wheat.stanford.edu (RSA)```
```

### For the oat GPU nodes:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDkFtixLWlJK5vIKe7N8mbIp63NXUIVej007UrsJyqSz oat.stanford.edu
256  MD5:   a1:f3:73:39:5c:28:b6:41:5f:92:ef:9c:60:aa:b2:83 oat.stanford.edu (ED25519)
​256  SHA256:tzEH/cWHU0P9/gmGJ7ceQVGhbOToaghLvxruubIrPyk     oat.stanford.edu (ED25519)

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCUsUFS3UvNEGtJE3TWoOx0tx2NpIqQYiBfagtm0nPBqUOtl2ZDKADhrVBIajn3VRSjvAS2hpItdwDs1/NBtqHqkEYqOSt1WDPU1Ld5ktbisWQ+iIOLOwdLVacse+4pNXospYAEO0dok9ZFDBZPzF2zNaBqSefxQvoLC/iyO5xjoZ/LcYDkz036KoRm1Y6NFcLwhTxXdSNyyOhvuMqpZrDMlPXMujRlkEqhkDS+73CGGDKrDfW4SBJja5Tg+zizSD/TxsKXqUle6n0EVsFot9wz1zHw8mcXlhflyH/4Cz66fhbRMf6Tv9yzvoKQyij2XMWxRJ4OFz+wfQ38/0rHr6vyTj7KvPERHTQmLQ4mMF0Bo/uMxctr74gCIob3ya138P2W8NbqmtbGH5gAYL0DVIiEMo5PPV3uhO12VDHSfVoRfqxwJ2sYWBgGkNAHykUgJ4oag17fToFcagr0dfh8xeqG02mxJS0pSxiqTqmOSQurTEua59qVZV+wvg1i00244Xk= oat.stanford.edu
​3072 MD5:   07:37:fa:10:bf:9a:0e:81:fd:29:7b:91:03:ae:a5:57 oat.stanford.edu (RSA)
3072 SHA256:PZx/lsQHualxmRBRu+2wL12RKOO/u5nsqf9Ohntq7ak     oat.stanford.edu (RSA)
```

If you have any problems with host-key verification, such as getting a different fingerprint, [please let us know](mailto:research-computing-support@stanford.edu?subject=FarmShare%202%20Host%20Key%20Problem&body=Hello!%0A%0AI%20am%20having%20a%20problem%20with%20host-key%20verification%20on%20FarmShare%202.)!

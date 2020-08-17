---
title: Initial Setup
description: 
published: true
date: 2020-06-21T17:12:44.924Z
tags: 
editor: undefined
---

# New User
Start by creating a new user.

`adduser newuser`

Next provide the account root privileges.

`usermod -aG sudo newuser`

> User will now be able to utilise the "sudo" command
{.is-info}

> We currently have not disabled the root user.
{.is-warning}

# Create Private Key Pair
Now lets create the public and private key pair. The public key is located on the server, and the private key will be used to access the server from any device.

`ssh-keygen`

When asked where to create the file, utilise the default. You can choose whether or not to enter in a passphrase. 
> Having a passphrase means that you need both the private key and passphrase to gain access. It provides an additional layer of security.
{.is-info}

Once your keys have been created they will be located in /home/root/.ssh – there should be id_rsa (private key) and id_rsa.pub (public key) files in that directory.

Next we need to copy the newly created key to the new user’s account:

`ssh-copy-id newuser@[server IP]`

Choose ‘Yes’ when asked if you want to continue, and enter newusers’s password when prompted.

# Modify SSH Settings
Modify the SSH settings so that both root user access and password authentication are disabled. Start by editing the SSH configuration file:

`nano -w /etc/ssh/sshd_config`

Change the default SSH port from 22 to something non-standard.

Find the line that says:

`#Port 22`

And change it to:

`Port [different port number]`

> You can use any port number for your SSH connection, eg port 2222.
{.is-info}

Now scroll down until you find the line that says:

`PermitRootLogin yes`

Change ‘yes’ to ‘no’:

`PermitRootLogin no`

This disables root user login.  Next scroll down further and find:

`#PasswordAuthentication yes`

> Remove the # at the beginning.
{.is-info}

`PasswordAuthentication no`

This disables password based authentication. (Private key authentication should already be enabled by default, you can confirm this by ensuring that PubkeyAuthentication is set to ‘yes’ in the SSH config file).

Restart SSH with:

`systemctl reload sshd`

# Download Private Key File
Next download our private key file. Cat the contents of the id_rsa file:

`cat ~/.ssh/id_rsa`

Next, open up a text editor and paste the entire block of text into a blank file. Save this file in a secure location.

Once saved, you can delete the id_rsa file from the server (test connectivity first):

`rm ~/.ssh/id_rsa`

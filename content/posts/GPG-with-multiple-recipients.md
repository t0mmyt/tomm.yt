---
title: "GPG With Multiple Recipients"
date: 2015-03-22T15:20:49+01:00
tags: [ "gpg", "encryption" ]
---
This is how to store sensitive information (e.g. SSL key passphrases) in a GPG encrypted file with multiple recipients.  If you're using git to store the file (which you probably should be), always do a git pull before doing any changes. These files are binary and therefore merges are very difficult otherwise.
<!--more-->
# Setting up your GPG key

Almost every server and desktop distro already has GPG installed. If you want to do this on Windows then Google is your friend.

    $ gpg --gen-key

The following settings should be good to go:

*   RSA/RSA
*   Keysize 4096 (cpu is cheap, and the NSA/GCHQ have lots of it)
*   5y (5 years)

There is no harm in having multiple email addresses (UIDs) and/or mixing work/home.

Pick a sensible passphrase (see the following link but do not use correct horse battery staple). [https://xkcd.com/936/](https://xkcd.com/936/)

Back up your key somewhere sensible and don’t forget your passphrase (there’s no recovery option).

Find your key ID:

    $ gpg --fingerprint tom@example.com
    pub   4096R/18BCAD4F 2014-03-24 [expires: 2019-03-23]
          Key fingerprint = 47AF 7F1F 7902 3375 A928  D4E1 2093 6706 18BC AD4F
    uid                  Tom Taylor <tom@example.com>
    uid                  Tom Taylor <tom@example.net>
    sub   4096R/BEAE11DC 2014-03-24 [expires: 2019-03-23]

In the above example, the key ID is 18BCAD4F

Send it to the public key servers

    $ gpg --keyserver hkp://pgp.mit.edu --send-key KEYNAME

Then ideally get it signed by some of your colleagues and re-uploaded. **pius** is a really good tool for doing this easily.

Also, it's a good idea to generate a revokation certificate and store it with your gpg setup.

    $ gpg --output .gnupg/revoke.asc --gen-revoke KEYNAME

# Installing GPG plugin for vim

This is the easiest bit and will save you from a lot of the pain that comes with using GPG.

    $ mkdir ~/.vim/plugins
    $ wget -O ~/.vim/plugins/gnupg.vim http://www.vim.org/scripts/download_script.php?src_id=12200

This allows you to open an already encrypted file as if it were an ordinary text file if you have a gpg agent running. If not then you are prompted for your passphrase

# Saving a new encrypted file

First, make sure you have a copy of all of the public keys of the other people wou want to share information with. To get these, run

    $ gpg --recv-key THEIRKEYID

You can check what you have already got with

    $ gpg --list-keys

Save your file as a regular text file and then run the following command to encrypt

    $ gpg --encrypt --recipient PERSONAKEY --recipient PERSONBKEY --recipient PERSONCKEY file

Your new file will be called file.gpg. Now delete the original and store the gpg file in git! Revision control is very important.

# Working with an encrypted file

The aforementioned vim plugin makes this super easy. Just vim file.gpg and the plugin deals with all fo the work. Save as normal, commit and push.


# Setup GitHub

A guide to setup new GitHub GPG key on Windows 10/11 for *someone* that have same problem as me...

## Prerequisites

1. Some basic knowledge about installation.
2. [Notepad++.](https://notepad-plus-plus.org/downloads/ "Notepad++")
3. [Git SCM.](https://git-scm.com/install/windows "Git SCM")
4. [Gpg4win.](https://www.gpg4win.org/ "Gpg4win")
5. [GitHub Desktop.](https://desktop.github.com/download/ "GitHub Desktop")

## Step by step

### 1. Install the programs.

* Notepad++.
  * Please install as default.
* Git SCM.
  * Please choosing Notepad++ as default editor.
* Gpg4win.
  * Please choosing _**ALL**_ the options. Specially is _**Kleopatra**_.
* GitHub Desktop.
  * Please choosing and remember correct email for using lately.

### 2. Check for existing GPG keys.

* Open Git Bash.
* Using following command:
```
gpg --list-secret-keys --keyid-format=long
```
If there is an output like this:
```
gpg: directory '/c/Users/<YourUserName>/.gnupg' created
```
This is normal.  
Cause the first time you running Git Bash, it will create a folder.  
If the output show existing keys like `xxxxxxxx`, running this command to export it:
```
gpg --armor --export xxxxxxxx
```
With `xxxxxxxx` is the key.  
Then go to [].  
If the output show nothing, go to [3. Generate a new GPG key.](#3-generate-a-new-gpg-key)

### 3. Generate a new GPG key.

* Open Git Bash.
* Using following command:
```
gpg --full-generate-key
```
* At the prompt, specify the kind of key you want, or press Enter to accept the default (I am choosing `RSA and RSA`).
* At the prompt, specify the key size you want, or press Enter to accept the default (I am choosing `4096` bits).
* Enter the length of time the key should be valid. Press Enter to specify the default selection, indicating that the key doesn't expire. Unless you require an expiration date, we recommend accepting this default (I am choosing `0` for lifetime).
* Verify that your selections are correct (Press `y`).
* Enter your user ID information.
  * When asked to enter your email address, ensure that you enter the verified email address for your GitHub account. To keep your email address private, use your GitHub-provided no-reply email address (I choosing using `no-reply` email).
* Type a secure passphrase (Or not, if you don't want to type password every time using and are going to using this key to global sign).
* Using this command to recheck again:
```
gpg --list-secret-keys --keyid-format=long
```
* From the list of GPG keys, copy the long form of the GPG key ID you'd like to use. From this, i will use `xxxxxxxx` as example.
* Next, using this command to export the GPG key:
```
gpg --armor --export xxxxxxxx
```
* Please copy your GPG key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`.
* Then go to [].
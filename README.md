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
* If the output show existing keys like `xxxxxxxx`, running this command to export it:
```
gpg --armor --export xxxxxxxx
```
With `xxxxxxxx` is the key.  
* Please copy your GPG key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`.  
* Then go to [4. Add a GPG key to your GitHub account.](#4-add-a-gpg-key-to-your-github-account)  
* If the output show nothing, go to [3. Generate a new GPG key.](#3-generate-a-new-gpg-key)

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
* Then go to [4. Add a GPG key to your GitHub account.](#4-add-a-gpg-key-to-your-github-account)

### 4. Add a GPG key to your GitHub account.

* In the upper-right corner of any page on GitHub, click your profile picture, then click `Settings`.
* In the `Access` section of the sidebar, click `SSH and GPG keys`.
* Next to the `GPG keys` header, click `New GPG key`.
* In the `Title` field, type a name for your GPG key (Should be your name's device).
* In the `Key` field, paste the GPG key you copied when you [generated your GPG key](#3-generate-a-new-gpg-key).
* Click `Add GPG key`.
* If prompted, authenticate to your GitHub account to confirm the action.
* Then go to [5. Tell Git about your signing key.](#5-tell-git-about-your-signing-key)

### 5. Tell Git about your signing key.

* Open Git Bash.
* Using the following command:
```
git config --global --unset gpg.format
```
To unset the default GPG using in Git Bash.
* Next using this command to get again the GPG key:
```
gpg --list-secret-keys --keyid-format=long
```
And get the long form of the key ID you'd like to use.  
* Using this command to set global key using in commit:
```
git config --global user.signingkey xxxxxxxx
```
* Also, using these commands to sign all commits and tags by default:
```
git config --global commit.gpgsign true
```  
```
git config --global tag.gpgSign true
```
* Next, please using this command to export the key to a file:
```
gpg --export-secret-keys --armor xxxxxxxx > private-key.asc
```
* Then, open `Kleopatra`, in `Kleopatra` please add the private-key.asc (Can add by drop in the file, please check that the `Status` is `certified`)
* After that, set the GPG program using gpg4win:
```
git config --global gpg.program "C:/Program Files (x86)/GnuPG/bin/gpg.exe"
```
* Go to [6. Add tasks to Task Scheduler.](#6-add-tasks-to-task-scheduler)

### 6. Add tasks to Task Scheduler.

* __Keyboxd.__
  * Open Task Scheduler.
  * Click `Create Task`.
    * `General Tab`.
      * `Name`: `Start Keyboxd`.
      * Check `Run only when user is logged on`.
      * Check `Run with highest privileges`.
    * `Triggers Tab`.
      * Click `New`.
      * `Begin the task`: `At log on` or `At startup`.
      * Click `OK`.
    * `Actions Tab`.
      * Click `New`.
      * `Action`: `Start a program`.
      * `Program/script`: `gpgconf`.
      * `Add arguments (optional)`: `--launch keyboxd`.
      * Click `OK`.
    * `Conditions Tab`.
      * Uncheck `Start the task only if the computer is idle for:`.
      * Uncheck `Start the task only if the computer is on AC power`.
      * Uncheck `Wake the computer to run this task`.
      * Uncheck `Start only if the following network connection is available:`.
    * `Settings Tab`.
      * Check `Allow task to be run on demand`.
      * Uncheck `Run task as soon as possible after a scheduled start is missed`.
      * Check `If the task fails, restart every`: `1 minute`. And `Attempt to restart up to`: `3 times`.
      * Check `Stop the task if it runs longer than`: `3 days`.
      * Check `If the running task does not end when requested, force it to stop`.
      * Uncheck `If the task is not scheduled to run again, delete it after:`.
      * `If the task is already running, then the following rule applies`: `Do not start a new instance`.
  * Click `OK`.

* __GPG-Agent.__
    * Open Task Scheduler.
    * Click `Create Task`.
        * `General Tab`.
            * `Name`: `Start GPG-Agent`.
            * Check `Run only when user is logged on`.
            * Check `Run with highest privileges`.
        * `Triggers Tab`.
            * Click `New`.
            * `Begin the task`: `At log on` or `At startup`.
            * Click `OK`.
        * `Actions Tab`.
            * Click `New`.
            * `Action`: `Start a program`.
            * `Program/script`: `gpgconf`.
            * `Add arguments (optional)`: `--launch gpg-agent`.
            * Click `OK`.
        * `Conditions Tab`.
            * Uncheck `Start the task only if the computer is idle for:`.
            * Uncheck `Start the task only if the computer is on AC power`.
            * Uncheck `Wake the computer to run this task`.
            * Uncheck `Start only if the following network connection is available:`.
        * `Settings Tab`.
            * Check `Allow task to be run on demand`.
            * Uncheck `Run task as soon as possible after a scheduled start is missed`.
            * Check `If the task fails, restart every`: `1 minute`. And `Attempt to restart up to`: `3 times`.
            * Check `Stop the task if it runs longer than`: `3 days`.
            * Check `If the running task does not end when requested, force it to stop`.
            * Uncheck `If the task is not scheduled to run again, delete it after:`.
            * `If the task is already running, then the following rule applies`: `Do not start a new instance`.
    * Click `OK`.  

So this is the end of the guide.  
The following below will be some small notice.  

### P.S.1. Associate an email with your GPG key.

Your GPG key must be associated with a verified email that matches your committer identity.  
It can be your mail or the no-reply GitHub mail.  
Your mail can be `your-mail@your-mail` or `xxxxxxxx+your-name@users.noreply.github.com`.  
Can be seen in upper-right corner icon, `Settings` then `Emails`.  
Turn on ``Keep my email addresses private`` (If you want to use `no-reply`, if not, just turn it off).  
Also choosing right email when logged in GitHub Desktop.  
Or you can edit the email in existing key.
* Open Git Bash.
* Use the following command:
```
gpg --list-secret-keys --keyid-format=long
```
* Choosing the existing key (Eg. `xxxxxxxx`).
* Using following command:
```
gpg --edit-key xxxxxxxx
```
```
gpg> adduid
```
* Following the prompts:
  * `Real Name`.
  * `Email address`.
  * `Comment`.
  * `Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit?`.
* You can modify your entries by choosing `N`, `C`, or `E`.
* Enter `O` to confirm your selections.
* Enter your key's passphrase.
* Using this to save:
```
gpg> save
```

### P.S.2. About commit signature verification.

If you want GitHub to check the cryptographically verifiable commit, go to `Settings` then `SSH and GPG keys` then `Vigilant mode` and check `Flag unsigned commits as unverified`.  
It will display status for commits.
* Default statuses.
  * `Verified`: 	The commit is signed and the signature was successfully verified.
  * `Unverified`: The commit is signed but the signature could not be verified.
  * `No verification status`: The commit is not signed.
* Statuses with vigilant mode enabled.
  * `Verified`: The commit is signed, the signature was successfully verified, and the committer is the only author who has enabled vigilant mode.
  * `Partially verified`: The commit is signed, and the signature was successfully verified, but the commit has an author who: a) is not the committer and b) has enabled vigilant mode. In this case, the commit signature doesn't guarantee the consent of the author, so the commit is only partially verified.
  * `Unverified`: Any of the following is true: `The commit is signed but the signature could not be verified.`, `The commit is not signed and the committer has enabled vigilant mode.`, `The commit is not signed and an author has enabled vigilant mode.`.

### P.S.3. About sign commits and tags.
* About commits
```
git commit -S -m "YOUR_COMMIT_MESSAGE"
```
```
git push
```
* About tags
```
git tag -s MYTAG
```
```
git tag -v MYTAG
```
This is the end of P.S. guide.
# Survive with Github and Gitlab SSH keys

With a lot of doubt and research, is it possible to have GitHub and GitLab ssh keys on the same computer?

The answer is yes but it is not advisable. The best answer is that you should set one ssh key for Github and another one for Gitlab. The first thing to do is install Git if you haven't. Next, you should check for existing ssh-keys on your system:

---
### Config your identity

First configure global user name and email:
```
git config --global user.name "Your Name Here"
git config --global user.email your@email.com
```

This is will configure your identity for git. If you have to config local names, you have to open your repository and follow the commands:
```
git config user.name "Your Name Here"
git config user.email your@email.com
```

If you have to check the variables, just remove the final command, this is the command:

### Check your name
```
git config --global user.name
Your Name
```

### Check your e-mail
```
git config --global user.email
Your e-mail
```

## Next: Lists the files in your .ssh directory, if they exist

Open the Terminal.

The first step is check if you create the SSH Key for GitLab or GitHub. <br>
Enter ```ls -al ~/.ssh``` to see if existing SSH keys:
```
$ ls -al ~/.ssh
```

If you have SSH Key config, this will be the return for you:
```
id_rsa.pub
id_ecdsa.pub
id_ed25519.pub
```

By default, the filenames of the public keys are one of the following above.

If you don't have an existing public and private key pair, this you be return to you on the console:
```
No such file or directory
```

For create a new SSH Key paste the text below, substituting in your GitHub email address. <br>
```$ ssh-keygen -f ~/.ssh/id_ed25519_github -t ed25519 -C "your_email@example.com"```

This creates a new ssh key, using the provided email as a label.

### Copies the contents of the id_ed25519_hub.pub file to your clipboard

Command for Windows: ```$ clip < ~/.ssh/id_ed25519_github.pub``` <br>
Command for Mac: ```$ pbcopy < ~/.ssh/id_ed25519_github.pub```

Tip: If clip isn't working, you can locate the hidden .ssh folder, open the file in your favorite text editor, and copy it to your clipboard.

#### Copies the SSH Key local to your GitHub

This step will make GitHub recognize your local key

- Sign in to GitLab.
- In the upper-right corner of any page > profile photo > Settings.
- In the user settings sidebar > SSH and GPG keys > New SSH key or Add SSH key.
- In the "Title" field, add a descriptive label for the new key. For example, if you're using a personal Mac, you might call this key "Personal MacBook Air".
- Paste your key into the "Key" field.
- Click Add SSH key.
- If prompted, confirm your GitHub password.

## Generate a new SSH key for Gitlab

Open the Terminal.
Paste the text below, substituting in your GitLab email address.
```
$ ssh-keygen -f ~/.ssh/id_ed25519_gitlab -t ed25519 -C "your_email@example.com"
```

Copy the contents of your public key file:

Command for Windows: ```$ clip < ~/.ssh/id_ed25519_gitlab.pub``` <br>
Command for Mac: ```$ pbcopy < ~/.ssh/id_ed25519_gitlab.pub```

#### Copies the SSH Key local to your GitLab

This step will make GitLab recognize your local key

- GitLab > top right corner > avatar > settings.
- In left sidebar > SSH Keys.
- In the Key box, paste the contents of your public key. If you manually copied the key, make sure you copy the entire key, which starts with ssh-ed25519 or ssh-rsa, and may end with a comment.
- In the Title text box, type a description, like Work Laptop or Home Workstation.
   - Optional: _In the Expires at box, select an expiration date. (Introduced in GitLab 12.9.) The expiration date is informational only, and does not prevent you from using the key. However, administrators can view expiration dates and use them for guidance when deleting keys._
- Select Add key.

## Create file to reconize Github and Gitlab SSH keys

Go to the .ssh directory ```cd ~/.ssh```, create a file named "config" with the following command:
```
$ touch config
```
Now open the config file with the command:
```
$ nano config
```
Write the following lines inside the config file:
```
# GITHUB
Host github.com
   HostName github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_ed25519_github

# GITLAB
Host gitlab.com
   HostName gitlab.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_ed25519_gitlab
How to edit files in a terminal with nano?
How can I save the file?
```

### Maybe this commands saveyour day for nano edits

- F3: save without exiting. 
- Ctrl + X: will prompt you if you've made changes.
- Ctrl + X: quit the editor without saving the changes.
   - then N when it asks if you want to save.


## Configure SSH agent forwarding
Make sure your own SSH key is set up and working.

You can test whether the local key works by entering ssh -T git@github.com in the terminal:
```
ssh -T git@github.com
> Hi USERNAME! You've successfully authenticated, but GitHub does not provide
> shell access.
```

With GitLab:
```
ssh -T git@gitlab.com
> Welcome to GitLab, @USERNAME!
```
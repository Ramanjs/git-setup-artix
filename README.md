# Setup Git on Artix VM

This doc describes the process of setting up git on your terminal and use it to push/pull changes without having to authenticate everytime. We will be setting up an ssh key to login to our github account.

## 1. Install OpenSSH

To generate an ssh key, first install the openssh package:
```
sudo pacman -S openssh
```

## 2. Configure Git (optional but recommended)

Set up your name and email:
```
git config --global user.name "Your Name"
git config --global user.email "youremail@iiitd.ac.in"
```

Change the default branch for Git using this command:
```
git config --global init.defaultBranch main
```

To enable colorful output with git, type
```
git config --global color.ui auto
```

Verify that all the data has been entered correctly:
```
git config --get user.name
git config --get user.email
```

## 3. Generate an SSH key

```
ssh-keygen -t ed25519 -C "youremail@iiitd.ac.in"
```

Just press enter on all the questions until a key is generated.

## 4. Linking your key with Github

#### -> Using Github Website
On your Artix terminal type:
```
cat ~/.ssh/id_ed25519.pub
```

Then on the Github website, go to your account, **Settings > SSH and GPG Keys** and click on **New SSH Key.**
Give your key a title and and copy the output of the above command in the **Key** field.

Click on **Add SSH key** to save your new key.

#### -> Using Artix Terminal

For this you need the `github-cli` package available on the `community` arch repository. So if you don't have arch repositories enables go ahead and [do that first.](https://wiki.artixlinux.org/Main/Repositories)

Install `github-cli` on your os:
```
sudo pacman -S github-cli
```

Now, go to the Github website and under **Settings > Developer settings > Personal access tokens** create a new token. Save this token in the file `token.txt` and then type the following command to link your local SSH key with your Github account:
```
gh auth login --with-token < token.txt
```

Incase the above command does not work (it probably won't work), you'll have to do a manual setup and type in your access token:
```
gh auth login
```

## 5. Testing your SSH connection 

Connect to Github on ssh using:
```
ssh -T git@github.com
```

You may see a warning like this:
```
> The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> RSA key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
> Are you sure you want to continue connecting (yes/no)?
```

Type `yes` and press enter.

Hopefully the connection will be successfull and you'll get this message:
```
> Hi username! You've successfully authenticated, but GitHub does not
> provide shell access.
```

If you get an error instead don't @ me, you're on your own. [Here's something that might help tho.](https://docs.github.com/en/authentication/troubleshooting-ssh/error-permission-denied-publickey)

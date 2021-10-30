# Manage multiple GitHub accounts on Mac

As a developer you might have to play around office work and sometimes need to work upon pet projects that lead you to work with different GitHub accounts on your machine.

## Lets learn how to setup that by example with 7 fire shots ;)

I've 2 different active GitHub accounts,

1. **Office Work:** office@email.com
2. **Personal Work:** personal@email.com

### 1. Generate SSH keys for each GitHub accounts

```
# Run within terminal

> cd ~/.ssh
> ssh-keygen -t rsa -C "personal@email.com" -f "github-personal"
> ssh-keygen -t rsa -C "office@email.com" -f "github-office"
```

The `-C` option is a comment to help identify the key.

The `-f` option specifies the file name for the key pair. My opinionated advice for file name is `github-{GitHub username}`

### 2. Verify newly generated SSH keys

```
# Run within terminal

> cd ~/.ssh
> ls -lart
```

You will see 4 files got generated as

```
github-personal
github-personal.pub
github-office
github-office.pub
```

### 3. Add public SSH keys to respective GitHub accounts

Setup personal GitHub account with public keys with given steps:

1. Copy generated public SSH key into clipboard

```
# Run within terminal

> pbcopy < ~/.ssh/github-personal.pub
```

2. Login into personal GitHub account then goto **profile settings** section.
3. Click on **SSH and GPG keys**.
4. Click on **New SSH key** button.
5. Paste the public ssh key in the text area copied earlier at step-1.
6. Click on Add SSH key to add your key and enter your password if it is asked.

**_Note: Go through the same steps to add your office's public SSH key._**

### 4. Create a configuration file to add two keys

1. Create SSH config file

```
# Run within terminal

> cd ~/.ssh
> touch config
```

2. Paste given content into config file _(use file editor of your choice)_

```
# Personal account
Host github.com-personal
   HostName github.com
   User git
   IdentityFile ~/.ssh/github-personal

# Office account
Host github.com-office
   HostName github.com
   User git
   IdentityFile ~/.ssh/github-office
```

3. Create git configuration files

- We need to create a directory that will host `.gitconfig` file which has definitions like `name`, `email` we want to use for any repository we clone/push through office GitHub account.

```
# Run within terminal

> mkdir ~/Documents/office
> touch .gitconfig
```

> Paste given content into `.gitconfig` file.

```
[user]
    email = office@email.com
```

- We need to create one global `.gitconfig` for our personal GitHub profile

```
# Run within terminal

> cd ~/.ssh
> touch .gitconfig
```

> Paste given content into `.gitconfig` file.

```
[user]
    name = Your Name
    email = personal@email.com

[includeIf "gitdir:~/Documents/office/"]
    path = ~/Documents/office/.gitconfig
```

### 5. Add the SSH keys to your SSH-agent

In order to use SSH we had created earlier we need to add them into SSH agent.

```
> ssh-add -K ~/.ssh/github-personal
> ssh-add -K ~/.ssh/github-office
```

You only need the `-K` option on a mac.

### 6. List out SSH keys added within SSH agent

```
ssh-add -l
```

You will see the list of previously added SSH keys. Now, we're almost done!

### 7. Clone your repository & start working upon your favourite project

1. Get into office folder

```
# Run within terminal

> cd ~/Documents/office/
```

2. Clone repository

```
> git clone git@github.com-office:USERNAME/my-repo.git
```

Keep enjoying working on your office projetcs within `office` directory & outside it you can play around with your personal projects.

I hope you enjoyed this journey. Cheers!

:star_struck:

<!--
.. title: how use multiple ssh keys with github
.. slug: how-use-multiple-ssh-keys-with-git
.. date: 2025-07-06 17:15:20 UTC+02:00
.. tags: git,ssh,github,windows,win
.. category: git
.. link: 
.. description: 
.. type: text
-->
Assuming that you have already configured one key to access GitHub using SSH.

### Step 1: Create Additional SSH Key

Put an additional key in the SSH config directory, e.g.:

**Windows:**
```
%USERPROFILE%/.ssh/some-name/id_ed25519
```

**Linux/macOS:**
```
~/.ssh/some-name/id_ed25519
```

### Step 2: Configure SSH Config

Now you need to edit (or create) SSH config under:

**Windows:**
```
%USERPROFILE%/.ssh/config
```

**Linux/macOS:**
```
~/.ssh/config
```

Add additional config:
```
Host github-some-new-label
    HostName github.com
    User git
    IdentityFile c:\Users\USERNAME\.ssh\some-name\id_ed25519
    IdentitiesOnly yes
```

### Caution
`IdentityFile` - must be full path to SSH key 

### Notice

`IdentitiesOnly yes` is a crucial SSH configuration option that tells SSH to only use the specific key file you've specified and ignore all other keys.

#### Without `IdentitiesOnly yes`
SSH tries multiple keys in this order

1. Keys loaded in SSH agent 
2. Default keys (`~/.ssh/id_rsa`, `~/.ssh/id_ed25519`, etc)
3. Your specified key file

GitHub sees the first key that works and authenticates with that account

#### With `IdentitiesOnly yes`
- SSH only uses the exact key file you specified
- It ignores SSH agent keys and default keys
- GitHub only sees your intended key

### Step 3: Configure Repository

Go to repo that should use new key.

Optionally set new git username and git email for given repo:

```shell
git config user.name "git username"
git config user.email "your.email@example.com"
```

Update remote to use new GitHub config:
```shell
git remote set-url origin git@github-some-new-label:github-user/project1.git
```

Where:
- `github-some-new-label` - is the same as label after `Host` in `.ssh/config` -> `Host github-some-new-label`
- `github-user` is GitHub user of appropriate repo; i.e., if https url is `https://github.com/code-drill/django-app-template.git` -> then ssh link will be: `git@github-code-drill:code-drill/django-app-template.git`.

### Step 4: Test Configuration

Test the SSH connection:
```shell
ssh -T git@github-some-new-label
```

You should see:
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### Step 5: Push Changes

Now you can push using the correct SSH key:
```shell
git push origin main
```
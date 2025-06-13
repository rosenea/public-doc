# Git

## 1 Installation Instructions

### 1.1 Windows Instructions

Open the terminal and run the following command:

```powershell
winget install --id Git.Git -e
```

### 1.2 Linux Instructions

#### 1.2.1 Using `apt` package manager

Open the terminal and run the following command:

```shell
sudo apt install -y git
```

#### 1.2.2 Using `pacman` package manager

Open the terminal and run the following command:

```shell
sudo pacman -Syu git
```

## 2 Configuration Instructions

### 2.1 WINDOWS -- Add Git to the `PATH` Environment Variable

> **NOTE**
>
> Sometimes, changes to `PATH` are only applied following a reboot. If the change does not work, you may need to reboot before attempting again.

If `git` is not available from the terminal (can verify using `git --help`), the you need to add the following path to the **System Environment Variable** `PATH`:

`C:\Program Files\Git\cmd`

Adding this path will allow you to access the following from the terminal:

* `git`
* [`git-lfs`](./Git-LFS.md)
* `gitk`

### 2.2 Attribute Configuration

#### 2.2.1 **REQUIRED:** Configure the global Git user name and email address

```shell
git config --global user.name "User Name"
git config --global user.email "user.name@email.com"
```

#### 2.2.2 **RECOMMENDED:** Configure Git to not automatically adjust the line endings

> **WARNING**
>
> If you are making this change **after** you have checked out a project, you **must** clone that repository from scratch **after** applying this change. Otherwise the change cannot take affect.

```shell
git config --global core.autocrlf false
```

#### 2.2.3 **OPTIONAL:** Configure Git to use `notepad.exe` (or another editor) for commit messages

```shell
git config --global core.editor "'C:\Windows\notepad.exe'"
```

### 2.3 Start your SSH Agent

To facilitate cloning projects from a Git repository using SSH keys you need to ensure that your local SSH Agent is running.

#### 2.3.1 Windows Instructions

To check if your SSH Agent is running, open the Admin Terminal (`Win`+`X` `A`) and run the following command:

```powershell
Get-Service ssh-agent | Format-List -Property Name,Status,StartupType
```

The output of this command should match:

```
Name        : ssh-agent
Status      : Running
StartupType : Automatic
```

If the output **does not** match the above output, run the following commands:

```powershell
Set-Service ssh-agent -StartupType Automatic
Start-Service ssh-agent
Get-Service ssh-agent
```

This will ensure the SSH Agent is started and will automatically start on login.

#### 2.3.2 Linux Instructions

Open the terminal and run the following command:

```shell
eval `ssh-agent`
```

### 2.4 Generate your SSH Public Key

Before you generate a new SSH key, you should check whether you have any SSH keys added to the `ssh-agent`. This is done by opening the terminal and running the following command:

```shell
ssh-add -l
```

If this command outputs an entry, then you can [test the SSH conection](TODO). Otherwise, this means that your agent is not managing any private keys. If you have a private key in your Bitwarden, [follow these steps to restore the key](#241-restore-bitwarden-ssh-key). Otherwise, you need to [generate a new SSH key](TODO).

#### 2.4.1 Restore Bitwarden SSH key

To restore the key from Bitwarden.
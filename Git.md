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

If this command outputs an entry, then you can [test the SSH conection](#247-test-ssh-connections). Otherwise, this means that your agent is not managing any private keys. If you have a private key in your Bitwarden, [follow these steps to restore the key](#241-restore-bitwarden-ssh-key). Otherwise, you need to [generate a new SSH key](#242-generate-new-key).

#### 2.4.1 Restore Bitwarden SSH key

To restore the key from Bitwarden, you need to start by copying the private key to the clipboard, open the terminal and run the following commands:

```powershell
mkdir $env:USERPROFILE\.ssh
Get-Clipboard | Out-File -FilePath $Env:USERPROFILE\.ssh\id_ed25519
```

Now that the file has been restored, you need to [add the key to the SSH Agent](#243-add-private-key-to-ssh-agent).

#### 2.4.2 Generate New Key

To use key-based authentication, you first need to generate public/private key pairs. We will use `ssh-keygen` to generate key files. To generate the required files, open the terminal and run the following command (*remember to change the comment*):

```shell
ssh-keygen -t ed25519 -C "comment"
```

The output from the command should look like the following lines:

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (<path to key here>):
```

At the prompt, you can select `Enter` to accept the default file path (these files will be cleaned later). Next, you're prompted to use a passphrase to encrypt your private key files.

> **WARNING**
>
> Setting a passphrase will cause compatibility issues with Docker, Containers, and WSL. While this is not generally recommended, we will be relying on different security measures to ensure this file is protected.

Click `Enter` when prompted to use a passphrase. The output should resemble the following:

```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /path/to/.ssh/id_ed25519
Your public key has been saved in /path/to/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:n1m8u9fPC7leUo2TnZWNrm0G3QUUndsRhX7DW1cRMO0 username@hostname
The key's randomart image is:
+--[ED25519 256]--+
|             +*BB|
|              o*=|
|             .+.O|
|           . o.EX|
|        S   + *+O|
|         . + =oo |
|          + o++o |
|             +*..|
|            += .=|
+----[SHA256]-----+
```

Now you have a public/private ED25519 key pair in the specified location. The `.pub` file is the public key, and the file without an extension is the private key:

```
total 8
-rw------- 1 username username 419 Jun 13 11:26 id_ed25519
-rw-r--r-- 1 username username 109 Jun 13 11:26 id_ed25519.pub
```

#### 2.4.3 Add Private Key to SSH Agent

The private key file is the equivalent of a password and should be protected the same way you protect your password. To load the private key file into `ssh-agent` open the terminal and run the following command(s):

**For Windows**

```powershell
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

**For Linux**

```shell
chmod 400 $HOME/.ssh/id_ed25519
ssh-add $HOME/.ssh/id_ed25519
```

After you add the private key to the `ssh-agent` service, the `ssh-agent` service automatically retrieves the local private key and passes it to the SSH client. At this point, if you haven't done so yet, [backup the private key in Bitwarden](#244-backup-private-key-in-bitwarden). If already backed up, you can [remove the private key](#245-remove-private-key).

#### 2.4.4 Backup Private Key in Bitwarden

Copy the content of the key to the clipboard using the following command:

**For Windows**

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519
```

**For Linux**

```shell
cat $HOME\.ssh\id_ed25519 | xclip -sel clip
```

Now you can add a new SSH key in Bitwarden and click the import from clipboard button next to the private SSH key. This will automatically create the public key. So you don't need to worry.

Now that the key is backed up, you can [remove the private key](#245-remove-private-key).

#### 2.4.5 Remove Private Key

Now that you have the private key managed by your `ssh-agent` and backed up in Bitwarden, you can remove the private key. This is done by running the following command:

**For Windows**

```powershell
rm -f $env:USERPROFILE\.ssh\id_ed25519
```

**For Linux**

```shell
rm -f $HOME/.ssh/id_ed25519
```

With the private key removed, you need to either [add the public key to your account(s)](#246-add-public-key-to-your-accounts), or [test the SSH connection(s)](#247-test-ssh-connections).

#### 2.4.6 Add public key to your account(s)

No matter the account (i.e., GitHub, Bitbucket, Gitlab), the process requires you to add the public key. To do this, you need to load the contents of the public key into your clipboard. This is done by running the following command:

**For Windows**

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub
```

**For Linux**

```shell
cat $HOME/.ssh/id_ed25519.pub | xclip -sel clip
```

Now, you will just need to use `Ctrl`+`v` to paste the key contents into the web form(s). Once you have added all the keys, you can finally [test your SSH connection(s)](#247-test-ssh-connections).

#### 2.4.7 Test SSH Connection(s)

At this point, the last thing we need to do is test our SSH connection(s). This is done by opening the terminal and running the following command(s):

```shell
# For GitHub
ssh -T git@github.com

# For BitBucket
ssh -T git@bitbucket.org

# For Gitlab
ssh -T git@gitlab.com
```

## 3 Troubleshooting

### 3.1 error: failed retrieving file `'####.db'`

To resolve this issue, make sure that you manually select only the needed mirrors. This is done by running the following command:

```shell
sudo pacman-mirrors -i
```

In the resulting window, select only the United States mirrors. Following this, the issue should be resolved after 1-2 executions of:

```shell
sudo pacman -Syu
```
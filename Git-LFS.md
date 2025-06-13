# Git LFS

## 1 Installation Instructions

### 1.1. Windows Instructions

Git LFS is installed when [Git](./Git.md) is installed on Windows. The only real "installation" requirement is to complete the step for [adding Git to the `PATH` Environment Variable](./Git.md#21-windows----add-git-to-the-path-environment-variable).

### 1.2 Linux Instructions

#### 1.2.1 Using `apt` package manager

Open the terminal and run the following command:

```shell
sudo apt install -y git-lfs
```

#### 1.2.2 Using `pacman` package manager

Open the terminal and run the following command:

```shell
sudo pacman -Syu git-lfs
```

## 2 Configuration Instructions

The only "configuration" instruction for Git LFS is ensuring that it is initialized. This is done by running the following command from the terminal:

```shell
git lfs install
```

If done correctly, your should see:

```
Git LFS initialized.
```
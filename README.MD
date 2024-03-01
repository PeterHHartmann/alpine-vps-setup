# Alpine VPS Setup

> Disclaimer: This project is intended only for my own personal use

This project is a custom quick setup for Alpine Linux 3.19
with all my personally preferred installs and configs for a development environment

## 🔶 Prerequisites / Notes

Some things to remember when setting up VPS initially

### ◼️ Creating SSH Keys

Remember to create keys in .ssh directory

```bash
ls ~/.ssh
```

SSH keygen using Ed25519 and 100 KDF (Key Derivation Function) rounds

```bash
ssh-keygen -t ed25519 -a 100
```

### 🔸 Alpine VPS of Choice

I prefer using a cloud instance provided by Linode as it is fairly priced with minimal setup and config needed.
These are my go to choices:

> - Choose a Distribution:
>   - Images: Alpine 3.19
> - Region: Europe, Frankfurt, DE
> - Linode Plan: Under _Shared CPU_ select Nanode 1GB, $5 monthly, 1GB RAM, >1 CPU, 25GB storage
> - Linode Label: \<_vps instance name_>
> - Root Password: _set a good, long password - keep it safe!_
> - SSH Keys: _include generated SSH public key (.pub)_
> - Optional Add-ons - Backup: _check this if you want Linode to back up your VPS - this is recommended_

### 🔸 Prepare SSH locally

Edit SSH config (_~/.ssh/config_) with current

```bash
Host <your hostname of choice>
    HostName <ip address of VPS instance>
    ServerAliveInterval 20
    TCPKeepAlive no
    (optional) LocalForward <local port> 127.0.0.1:<remote port>
```

## 🔶 Installation

Connect to your Virtual Private Server

```bash
ssh root@alpine-dev
```

To get this repository we must first install `git`:

```bash
apk add git
```

Next, we can clone the repository:

```bash
git clone https://github.com/PeterHHartmann/alpine-vps-setup.git
```

ensure that install script is executable.

```bash
chmod +x ./alpine-vps-setup.git/src/install-script
```

The script needs to know your username before running install.

```bash
export NEWUSER=peter
```

The script needs to know your new hostname before running install.

```bash
export NEWHOSTNAME=peter-vpc
```

Now we are ready to run the installation script:

```bash
./alpine-vps-setup/src/install
```

> You are now going to set a password. Make sure to choose a secure password and write it down. Note: Try to avoid characters like `!#` in your password. The characters may give you a problem logging in, and locked you out. If so, rebuild the vps again. You may just want to use the suggested password.

The script should end with a message telling you to activate your user.

> Make sure to make a note of your password. You might get completely locked out of your Development Server if you lose this password. Also, keep it safe!

Do that, and reboot the server as instructed.

Your Development Server will then be ready after the reboot, which usually takes two or three minutes.

You can now access your newly created user with SSH:

```bash
ssh peter@alpine-dev
```

## 🔶 Extras - Improve DX With Dotfiles

The installation is a bit rough out-of-the-box.
You can improve your developer experience by adding a few configuration files for the shell and the `vim` editor.

> You should only perform these steps after the reboot instructed above.

First we must clone the repository once more for your new user:

```bash
git clone https://github.com/PeterHHartmann/alpine-vps-setup.git
```

Next, we must unzip the configuration files:

```bash
unzip alpine-vps-setup/src/dotfiles.zip
```

Now exit from `alpine-dev` and re-connect to have the configuration take effect.

Once you are reconnected, run `vim`:

```bash
vim
```

Press `ENTER` to acknowledge the error, then wait for the installs to run and press `ENTER` again.

Press `ESC` to make sure you are in `NORMAL` mode, and run the command `:PlugInstall`.

You can now close the plug-installer pressing `ESC` and `:bd`, and quit `vim` pressing `ESC` and `:q`.

Your `alpine-dev` server is now ready.

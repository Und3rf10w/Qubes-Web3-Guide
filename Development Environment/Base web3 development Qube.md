# Setting up the base Web 3 development TemplateVM

We will use our core [`debian-11-minimal` TemplateVM](/Qubes%20Web3%20Development%20Environment/Core%20Environment/Installing%20Debian%2011%20minimal%20template.md) to be the base of our developer TemplateVM.

Begin, in `dom0`, with creating our TemplateVM:

```bash
qvm-clone debian-11-min-web3 debian-11-min-web3-development
```


## Installing Initial Packages
Transitioning to our `debian-11-min-web3-development` TemplateVM, we will begin by installing essential tools:


```bash
sudo apt install p7zip-full chromium curl firefox-esr python3 build-essential ca-certificates gnupg lsb-release
```


## NodeJS
As documented in [Node's documentation](https://vscodium.com/#install), we have to install Node's apt repo, and then install the package. The correct way to do this is to download the script from a dispVM, then download whatever is needed in the script, transfer it from the dispVM to this templateVM, then install `nodejs` via apt.


### Easier Installation
The EASIER (read: less secure and not recommended) way to do this is to go to `dom0` and:

```bash
qvm-prefs debian-11-min-web3-development netvm sys-firewall
```

Go to the `debian-11-min-web3-development` TemplateVM and run:

``` bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo bash -
sudo apt install -y nodejs
```

Finally, return to `dom0` and run:

```bash
qvm-prefs --default debian-11-min-web3-development netvm
```

## Docker
To install `docker` and `docker-compose`, we're going to use the [official Docker installation instructions].

The correct way to do this is to download the individual keys from a dispVM and copy them over, then manually follow the provided instructions.

### Easier Installation
The EASIER (read: less secure and not recommended) way to do this is to go to `dom0` and:

```bash
qvm-prefs debian-11-min-web3-development netvm sys-firewall
```

Go to the `debian-11-min-web3-development` TemplateVM and run:

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Finally, return to `dom0` and run:

```bash
qvm-prefs --default debian-11-min-web3-development netvm
```


## Rust
Per official [Rust documentation](https://github.com/rust-lang/rustup/issues/2383), rust installation should be done on a per user basis, and thus will not be installed in this TemplateVM, but rather in every appVM.


# Resources
- [Docker Debian Setup](https://docs.docker.com/engine/install/debian/#set-up-the-repository)
- [Node Debian Setup](https://github.com/nodesource/distributions/blob/master/README.md#debinstall)

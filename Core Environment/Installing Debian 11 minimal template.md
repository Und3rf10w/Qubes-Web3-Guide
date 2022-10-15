# Initial Installation of the debian-11-minimal TemplateVM

This Qube TemplateVM, `debian-11-min-web3` will be the base VM that all other TemplateVMs will be spawned from. 

# Installing base debian-11-minimal TemplateVM
Prior to setting up our web3 base templateVM, we'll have to set up our initial debian-11-minimal TemplateVM.

Open `dom0`, and run:

```bash
sudo qubes-dom0-update qubes-template-debian-11-minimal
qvm-run -u root debian-11-minimal xterm
```

This will open an `xterm`  for the `debian-11-minimal` TemplateVM with a dispVM. In this window run:

```bash
sudo apt update
sudo apt install qubes-core-agent-passwordless-root qubes-vm-dependencies qubes-vm-recommended locales-all 
sudo poweroff
```

You now have a useful `debian-11-minimal` template.

# Creating our base Web3 TemplateVM
Begin, in `dom0`, with creating our TemplateVM:

```bash
qvm-clone debian-11-minimal debian-11-min-web3
```

Transitioning to our `debian-11-min-web3` TemplateVM, we will begin by installing some initially required packages:

```bash
sudo apt install -y qubes-snapd-helper snapd
```

This set of packages will ensure that any future TemplateVMs spawned from this TemplateVM will have anything and everything we will need.

Once package installation is completed, shut down the `debian-11-min-web3` TemplateVM:

```bash
sudo poweroff
```
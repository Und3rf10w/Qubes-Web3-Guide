# Initial Installation of the debian-11-minimal TemplateVM
==#todo: Explain the inital setup of the debian-11-minimal TemplateVM, ensuring you highlight that you have to install `snapd`, and `qubes-snapd-helper`==

This Qube TemplateVM, `debian-11-min-web3` will be the base VM that all other TemplateVMs will be spawned from. 


# Creating our base Web3 TemplateVM
Begin, in `dom0`, with creating our TemplateVM:

```bash
qvm-clone debian-11-minimal debian-11-min-web3
```

Transitioning to our `debian-11-min-web3` TemplateVM, we will begin by installing some initially required packages:

```bash
sudo apt install -y qubes-snapd-helper snapd qubes-vm-dependencies qubes-vm-recommended
```

This set of packages will ensure that any future TemplateVMs spawned from this TemplateVM will have anything and everything we will need.

Once package installation is completed, shut down the `debian-11-min-web3` TemplateVM:

```bash
sudo poweroff
```
# Overview
This section describes the `split-bitcoin` feature of Qubes.

Basically, look here, as this has already been done and well documented: https://github.com/Qubes-Community/Contents/blob/master/docs/security/split-bitcoin.md

# Installation
Based on our setup, just a few commands need to be changed, but for the most part it's going to be very similar to [what already exists and has well documented](https://github.com/Qubes-Community/Contents/blob/master/docs/security/split-bitcoin.md). A few minor tweaks need to be made to be adopted to our environment.

This documentation was written for `Electrum-4.3.1`, you should adjust the commands as appropriate.
## Prerequisites
- You should already have completed [Installing Debian 11 minimal template](Core%20Environment/Installing%20Debian%2011%20minimal%20template.md), as templates will be based off of this

## Creating bitcoin TemplateVM
On `dom0`:

```bash
qvm-clone debian-11-minimal debian-11-min-bitcoin
qvm-prefs debian-11-min-bitcoin netvm sys-firewall
```

On `debian-11-min-bitcoin`:

```bash
sudo apt install python3-pyqt5 libsecp256k1-0 python3-cryptography python3-setuptools python3-pip virtualenvwrapper
```

We'll install electrum using `pip`, but we'll need to keep the install package in our home directory to make this easy. On the same TemplateVM:

```bash
sudo wget -O /opt/Electrum.tar.gz https://download.electrum.org/4.3.1/Electrum-4.3.1.tar.gz
# You should REALLY verify the signature before continuing
sudo poweroff  # Shutdown the TemplateVM
```

Go back to `dom0`:
```bash
qvm-prefs --default debian-11-min-bitcoin netvm
```

### Creating offline wallet and offline wallet AppVM Qube

### Make the AppVM
Now, continuing in `dom0`, we'll create our `web3-bitcoin-offline` AppVM Qube:

```bash
qvm-create -t debian-11-min-bitcoin -l black web3-bitcoin-offline
qvm-prefs web3-bitcoin-offline netvm none
```

### Make the offline wallet
Next, we'll create the offline wallet itself that we'll use. Keep in mind that this is basically just a copy of [the official documentation](https://electrum.readthedocs.io/en/latest/coldstorage.html#create-an-offline-wallet) on the date of authoring this article. Review the documentation prior to continuing to ensure you're aware of any possible changes and adapt accordingly.

Open a terminal on `web3-bitcoin-offline`, and run `electrum`.

```bash
sudo python3 -m pip install /opt/Electrum.tar.gz  # We'll install this for all users instead of just the local user, creating a folder in 
```

Open electrum with `electrum`.

Create a new wallet using file -> new

Once created, copy the master key of the wallet to you clipboard via Wallet -> Information

### Create the watching-only Qube
On `dom0`, run:

```bash
qvm-create -t debian-11-min-bitcoin -l blue web3-bitcoin-watching
qvm-prefs web3-bitcoin-watching sys-whonix  # Set the NetVM to however you want to connect
```

Open a terminal in you new `web3-bitcoin-watching` AppVM and run:

```bash
sudo python3 -m pip install /opt/Electrum.tar.gz
electrum  # to open electrum
```

Now electrum will be opened, depending on your threat model, you may want to consider enabling Electrum's auto updates on the AppVM.

Paste the private key for the wallet into this AppVM's clipboard.

Open `electrum`, then follow the official instructions for [creating a watching-only version of your wallet].

# Spending using this setup
When you want to spend from your offline wallet, you can refer to [the official documentation](https://electrum.readthedocs.io/en/latest/coldstorage.html#create-an-unsigned-transaction) for instructions Qube AppVMs
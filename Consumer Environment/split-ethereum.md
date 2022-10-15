# Overview
This section describes a `split-ethereum` setup modeled after the well-documented [Qubes split-bitcoin](https://github.com/Qubes-Community/Contents/blob/master/docs/security/split-bitcoin.md) setup.

# Installation
Ensure you fulfill the prerequisites prior to continuing.

## Prerequisites
- Complete [Creating our base Web3 TemplateVM](Core%20Environment/Installing%20Debian%2011%20minimal%20template.md#Creating%20our%20base%20Web3%20TemplateVM)

## Creating Ethereum TemplateVM
On `dom0`:

```sh
qvm-clone debian-11-min-web3 debian-11-min-web3-ethereum
```

Start `debian-11-min-web3-ethereum`.

Open a dispVM and download [the lastest MyCrypto Desktop Release](https://github.com/MyCryptoHQ/MyCrypto/releases/tag/1.7.17).

Open the terminal on the dispVM and run:

```sh
qvm-copy linux-x86-64_*MyCrypto.AppImage
```

Select the `debian-11-min-web3-ethereum` TemplateVM as the target.

Open a terminal on the `debian-11-min-web3-ethereum` TemplateVM, and run:

```shell
sudo cp ~/QubesIncoming/disp*/*.AppImage /opt/
sudo chmod +x /opt/*.AppImage
sudo apt install libdbus-glib-1-2
```

Power off the `debian-11-min-web3-ethereum` TemplateVM.

# Creating the Ethereum AppVMs
There are two separate approaches one can use to manage Ethereum identities. In this context, an identity is defined as the associated wallet and all actions associated with the particular wallet. All activity associated with a given address is assumed to be able to be "traced" back to that particular wallet, creating an identity.

- All in one wallet approach
	- With this method, you create separate "interaction" AppVMs for each unique identity, and one offline AppVM to manage all of the associated wallets for each identity.
	- This approach is easier from a management and usage perspective, but the convenience provided comes at the risk of making it easier to accidentally cross identities.
	- This approach is suggested for most users unless your threat model requires strict maintenance of separated identities
- Wallet and User pair approach
	- With this method, you create a pair of AppVMs for each unique identity. One offline AppVM to manage the identity's wallet, and one online AppVM to do the interactions associated with the identity.
	- This approach is more costly from a resources and management perspective, but is more secure at the trade-off of convince.
	- You should opt to use this method, as it minimizes the available attack surface.

Regardless of the method you choose, the following steps are applicable to both.

## Creating the offline Ethereum wallet AppVM
1. Create an AppVM from the `debian-11-min-web3-ethereum` TemplateVM. We'll refer to this as the `web3-eth-wallet` AppVM. Ensure the `Networking` option is set to `(none)`
2. Open a terminal in the `web3-eth-wallet` AppVM
3. Launch `MyCrypto` by running `/opt/*.AppImage`
4. A dialog should appear asking if you'd like to integrate the app image with your system. Select `Yes`.
5. Close `MyCrypto`
6. In the Qube manager, open the Qube settings for the `web3-eth-wallet` AppVM.
7. Select the Applications tab
8. Select `Refresh applications`
9. Once completed, move `MyCrypto` from Available to Selected.
10. Close the settings window.

### Creating a New Wallet in MyCrypto
Now we'll create the wallet for the identity this AppVM will use in MyCrypto.

Start with opening the `MyCrypto` application in the `web3-eth-wallet` AppVm.

1. Select "Create New Wallet"
2. Click `Generate a Wallet`
3. Select `Mnemonic Phrase` (unless you want to use Keystore File, but we'll choose this for compatability)
4. Save your seed phrase and Confirm that you've saved it.
5. Select the order of your phrase then click `Confirm Phrase`
6. Select `Go to Account`
7. At the bottom, select Mnemonic Phrase
8. Enter your Mnemonic Phrase

Your wallet is now ready to use for Signing and Verifying Messages.

When you open MyCrypto, you'll have to reenter your Mneomic Phrase to use your wallet.
# VSCodium
The VSCodium installation here assumes that you want to set up a unique VSCodium that has access to the VSCode extensions.

## Risk Tolerance
There are three options for setting up VSCodium depending on your risk appetite. ==The `snap` installation method is recommended==, otherwise your VSCodium TemplateVM and core `debian-11-min-web3-development` TemplateVM will not be in sync, which may lead to potential issues.

You can ignore the VSCode extension store setup, but it may lead to a less optimal experience at the trade-off of increase security and/or privacy.

This model assumes that the sources of VSCodium are free of back-doors. If that doesn't work for you, compile from source.

# Installation Guide
Depending on your use case(s), you may want to use multiple methoasdfds (e.g. the `snap` method, and one of the `apt` methods).

1. (**RECOMMENDED**) [Install from `snap`](https://snapcraft.io/codium)
	1. Create a new appVM from the `debian-11-min-web3-development` TemplateVM (`web3-dev-core-codium`)
	2. Inside this new appVM, execute `snap install codium`
3. (*RECOMMENDED**) (New TemplateVM) Install from community provided apt sources:
	- As described in [Codium's official documentation](https://vscodium.com/#install), you also have the option of installing a codium from a [community repo for major releases](https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo), or a [community repo for hourly releases](https://vscodium.c7.ee/)
	- Recommended if you want to install the latest VSCodium in sync with releases
4. (New TemplateVM) Install from debian apt sources:
	- A bit more annoying as you'd have to create a new TemplateVM and base appVMs off of that
	- Not recommended due to lagging updates


Using method 1 (snap), you can have a base VSCodium environment that you can clone additional appVMs from. Using methods 2 or 3, you will have a TemplateVM for VSCodium appVMs that you can use to create dispVMs or additional appVMs from.

# Installation from snap
==#todo: document this==

# Installation from official apt sources
==**It is HIGHLY recommended you follow these steps at least once**==

1. In `dom0`, create a new TemplateVM from your [`debian-11-min-web3-development` TemplateVM](Development%20Environment/Base%20web3%20development%20Qube.md) to create a new `debian-11-min-web3-dev-codium` TemplateVM:
```bash
qvm-clone debian-11-min-web3-development debian-11-min-web3-codium
```
2. Open a dispVM, then run:
```bash
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg
qvm-copy pub.gpg  # CHOOSE: debian-11-min-web3-codium
```
3. Return to your codium template and run:
```bash
cat ~/QubesIncoming/$dispVM_name/pub.gpg | gpg --dearmor | sudo dd of=/usr/share/keyrings/vscodium-archive-keyring.gpg
rm -rf ~/QubesIncoming/*  # Keeps your template clean
echo 'deb [ signed-by=/usr/share/keyrings/vscodium-archive-keyring.gpg ] https://download.vscodium.com/debs vscodium main' \
    | sudo tee /etc/apt/sources.list.d/vscodium.list
sudo apt update
sudo apt install codium
```  
4. Shutdown your `debian-11-min-web3-dev-codium` TemplateVM
5. Return to `dom0`, run:
```bash
qvm-clone debian-11-min-web3-dev-codium debian-11-min-codium-base
```

At this point, you have a fresh TemplateVM, `debian-11-min-codium-base`, that you can use as a base for new VSCodium Template and AppVMs. It is recommend you no longer make changes to the `debian-11-min-codium-base` TemplateVM, so you always have a fresh VSCodium TemplateVM to revert to.

## apt Based Installation Usage Guide

### Pre-installing extensions into TemplateVM
==#todo: document the much more secure method to install extensions==

Reasons to do this method:
- You want to have an inventory of TemplateVMs with pre-installed extension sets
- Easy to maintain a different set of extensions for rapidly switching between 
- Want to be able to just change the underlying TemplateVM 

If you want to have a TemplateVM that can create AppVMs with extensions already installed and ready to deploy, you will have to install them in the TemplateVM, then put them in `/etc/skel`.

This is not the most secure method, but it's easy enough for the layperson to do. The proper way to do this is much more involved.

1. Open `dom0` and provide networking to your TemplateVM:
```bash
qvm-prefs debian-11-min-web3-codium netvm sys-firewall
```
2. Open `codium` on the TemplateVM and install your extensions
3. Close `codium` on the TemplateVM
4. In `dom0`, disable networking on the TemplateVM:
```
qvm-prefs --defaults debian-11-min-web3-codium netvm
```
5. In the TemplateVM, copy your `.vscode-oss/extensions` directory to `/etc/skel/`:
```bash
sudo mkdir /etc/skel/.vscode-oss/
sudo cp -r {~,/etc/skel}/.vscode-oss/extensions/
```
6. Shutdown the TemplateVM
7. 
Now, when you have an AppVM you want to use Codium with this set of extensions installed, just set its TemplateVM to point to your created TemplateVM

## Additional Tips for apt based VSCodium TemplateVMs
### Use official VSCode Extensions
==#todo: document this==
If you'd rather use the official VS Code extensions, instead of the open-vsx ones, we can [follow the guide here](https://github.com/VSCodium/vscodium/blob/master/DOCS.md#extensions--marketplace)

Edit `/usr/share/codium/resources/app/product.json` appropriately to match on your TemplateVMs:

```json
{
  "extensionsGallery": {
    "serviceUrl": "https://marketplace.visualstudio.com/_apis/public/gallery",
    "cacheUrl": "https://vscode.blob.core.windows.net/gallery/index",
    "itemUrl": "https://marketplace.visualstudio.com/items",
    "controlUrl": "",
    "recommendationsUrl": ""
  },
  "extensionAllowedProposedApi": [
    "ms-vscode-remote.vscode-remote-extensionpack"
  ]
}
```




## Completing install 
Once everything is ready, return to `dom0`, run:
```
qvm-create -t debian-11-min-web3-dev-codium --label yellow -C AppVM web3-codium
```


# Suggested VSCode Extensions

# Syncing your configuration across Qubes
### Syncing your configuration with from TemplateVMs in an apt setup
==#todo: document this==

### Syncing your configuration with AppVMs in a snap setup
==#todo: document this==

# Resources
- [VSCodium installation guide](https://snapcraft.io/codium)

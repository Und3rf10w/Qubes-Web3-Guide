# Overview
This set of documentation describes my web 3 development and usage environments using [Qubes](https://www.qubes-os.org/) 4.1.1.

[Click here to skip straight to the Setup Guide](#Setup%20Guide)

This projects serves to document my effort, and may act as a guide for for individuals or groups wanting to use or develop on web 3 in a secure manner using the Qubes OS and principles from [Qubes](https://www.qubes-os.org/doc/getting-started/), without the need for the use of a physical hardware wallet.

## Intended Audience
This document assumes the reader already has a working familiarity with Web 3 and Qubes OS. This document also assumes that the reader has a sufficient platform to run the Qubes environment on, and owns at least one Yubikey. Finally, this document assumes the reader has a interest in security, and privacy as it applies to Web 3.

## Disclaimer
While secure and private computing is a personal passion, that by no means makes me an expert, and I make no guarantees. I have taken every reasonable effort to create a secure environment and workflow based off of the culmination of my knowledge, including operational security practices based off of my theoretical threat model. 

As one review's this document, it is critical to always consider:
> ==_MY_ threat model model is **NOT** _YOUR_ threat model.==

This document ignores certain supply chain risks, such as compromise of the underlying hardware Qubes is operating on, compromise of the integrity of the user's Yubikey, and Xen hypervisor exploits. Essentially, if you're being targeted using these types of attacks, you likely have bigger problems that this model can't assist with.

Suggestions for improvements or identifications of vulnerabilities for the proposed architecture are welcomed to be submitted via issues.


## Donations
Donations are accepted for via either the Github Sponsors program, or at the following cryptocurrency addresses:

- BTC: `<addme>`
- ETH: `<addme>`
- SOL: `<addme>`
- XMR: `<addme>`


# Security Principles
## Threat Model Basics
This projects attempts to create an environment to be used for two types of users:
- Web 3 developers
	- Groups or individuals creating applications in the Web 3 ecosystem
	- Potential use cases:
		- Smart Contract Development
- Web 3 consumers
	- Groups or individuals participating in the Web 3 ecosystems
	- Potential usecases:
		- Smart Contract Usage
		- DeFi usage
		- NFT usage

From a consumer standpoint, this project attempts to mitigate some of the risks associated with threats such as cryptocurrency phishing scams, code compromise of the machine where transactions are being conducted, and physical attacks.

From a developer standpoint, this project attempts to mitigate some of the risks associated with threats such as infrastructure compromise, and physical attacks. This environment also attempts to provide simple ways of segmenting development or production infrastructure.

For the sake of simplicity and cost (and the fact that it doesn't particularly offer significant benefit in this setup), hardware wallets are ignored in this set-up.

# Environments
To facilitate the proposed security model, Qubes are segmented into three environments, defined as:

- Core Environment
	- Critical Qubes that facilitate the operation of other the environments.
	- Contains Core appVM Qubes, network-providing system appVM Qubes, and TemplateVM Qubes that core environment Qubes are based off of
- Development Environment
	- Qubes that are used to provide a reasonably-secure environment intended for creators, developers, and/or security researchers within the Web 3 ecosystem
	- Contains development related appVM Qubes, network-providing system appVM Qubes, and the associated TemplateVM Qubes.
- Consumer Environment
	- Qubes that are used to provide a reasonably-secure environment intended for Web 3 consumers.
	- Contains wallet and browsing related appVM Qubes, network-providing system appVM Qubes, and the associated TemplateVM Qubes.

## Core Environment
- [Installing Debian 11 minimal template](Core%20Environment/Installing%20Debian%2011%20minimal%20template.md)

## Development Environment
- [Setting Up a VSCodium environment for Web3 Development](Development%20Environment/Setting%20Up%20VSCodium%20environment%20for%20Web3%20Development.md)


## Consumer Environment



# Setup Guide

## Core Setup
1. [Installing Debian 11 minimal template](Core%20Environment/Installing%20Debian%2011%20minimal%20template.md)


## Development Setup
1. [Install the base web3 development Qube](Development%20Environment/Base%20web3%20development%20Qube.md)
2. [Set Up a VSCodium environment for Web3 Development](Development%20Environment/Setting%20Up%20VSCodium%20environment%20for%20Web3%20Development.md)
	- Ensure that you follow the apt installation method to end up with a base `debian-11-min-codium-base` TemplateVM

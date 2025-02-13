# Ledger Cosmos

## Using SCRT with Ledger <a href="#using-scrt-with-ledger" id="using-scrt-with-ledger"></a>

Note: This guide is for Ledger Nano S but according to community members it also works for Ledger Nano X.

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* This guide assumes you have a verified, genuine Ledger Nano S device.
* If you don't, or you using your Ledger device for the first time, you should check Ledger's [Getting Started](https://support.ledger.com/hc/en-us/sections/360001415213-Getting-started) guide.
* We also advise you to check your Ledger's genuineness and upgrade your firmware to the newest one available (`v1.6.0+`).
* Have a machine with [Ledger Live](https://www.ledger.com/ledger-live) installed.
* Have the latest version of our latest binaries installed. You can get it [here](https://github.com/scrtlabs/SecretNetwork/releases/latest).

### Install Cosmos Ledger App <a href="#install-cosmos-ledger-app" id="install-cosmos-ledger-app"></a>

* Open Ledger Live and go to Settings (gear icon on the top right corner): ![](https://raw.githubusercontent.com/cosmos/ledger-cosmos/master/docs/img/cosmos\_app1.png)
* Enable developer mode:\
  ![](https://raw.githubusercontent.com/cosmos/ledger-cosmos/master/docs/img/cosmos\_app2.png)
* Now go to Manager and search "Cosmos": ![](https://raw.githubusercontent.com/cosmos/ledger-cosmos/master/docs/img/cosmos\_app3.png)
* Our binaries require Cosmos App Version `1.5.1` (if you only see a lower version available, like `1.0.0`, then you need to upgrade your Ledger firmware).
* Hit "Install" and wait for the process to complete.

_Ref: https://github.com/cosmos/ledger-cosmos_

### Common commands <a href="#common-commands" id="common-commands"></a>

These are some basic examples of commands you can use with your Ledger. You may notice that most commands stay the same, you just need to add the `--ledger` flag.

**Note: To run these commands below, or any command that requires signing with your Ledger device, you need your Ledger to be opened on the Cosmos App:**\
![](https://miro.medium.com/max/1536/1\*Xfi5\_ScAiFn6rr9YBjgFFw.jpeg) _Ref: https://medium.com/cryptium-cosmos/how-to-store-your-cosmos-atoms-on-your-ledger-and-delegate-with-the-command-line-929eb29705f_

#### Fix Connection Issues <a href="#fix-connection-issues" id="fix-connection-issues"></a>

* Prepare your Linux host to work with ledger

Some users may not have their ledger recognized by their Linux host. To fix this issue implement the fix for connection issues on Linux from the [ledger support page](https://support.ledger.com/hc/en-us/articles/115005165269-Connection-issues-with-Windows-or-Linux)

```
wget -q -O - https://raw.githubusercontent.com/LedgerHQ/udev-rules/master/add_udev_rules.sh | sudo bash
```

* MacOS

You will need at least MacOS 10.14 Mojave, which introduced the Security feature of allowing Full Disk Access, which Ledger Live needs in order to enable the `--ledger` flag in `secretcli`. Refer to the MacOS section in the [ledger support page](https://support.ledger.com/hc/en-us/articles/115005165269-Connection-issues-with-Windows-or-Linux).

### Keplr <a href="#keplr" id="keplr"></a>

We recommend using [Keplr Wallet](https://keplr.xyz/) as the interface for using Ledger with the Secret Network. To use, simply install the extension, and select "Import Keplr" when adding a new account

![image](https://user-images.githubusercontent.com/16579705/114312311-ee925100-9afa-11eb-9d89-1a2c78281fc3.png)

### SecretCLI <a href="#secretcli" id="secretcli"></a>

For a more advanced user, it is possible to interface with the CLI utility, SecretCLI with a Ledger device. You can get it [here](https://github.com/scrtlabs/SecretNetwork/releases/latest)

#### Create an account <a href="#create-an-account" id="create-an-account"></a>

> Note: You can use any number you'd like for your account number. Be sure to remember the number you used, so you can recover if needed.

```
secretcli keys add <account name> --ledger --account <account number on your Ledger> --legacy-hd-path
```

⚠️⚠️⚠️

**Please backup the mnemonics!**

> Note: Ledger only supports a BIP-44 HD path of `44'/118'/{account}'/0/{index}`, while Secret Network wallets will use `44'/529'/{account}'/0/{index}` by default. This means if you wish to export the keys to a different wallet, you must select 118 as the coin type when importing the mnemonics

#### Display your account address <a href="#display-your-account-address" id="display-your-account-address"></a>

```
secretcli keys show -a <account name>
```

#### Add an account to `secretcli` that already exists on your Ledger <a href="#add-an-account-to-secretcli-that-already-exists-on-your-ledger" id="add-an-account-to-secretcli-that-already-exists-on-your-ledger"></a>

_You'll use this when you, say, using a different machine._

```
secretcli keys add <account name> --ledger --account <account number on your Ledger> --recover --legacy-hd-path
```

**Note! If you run the above command without the `--ledger` flag, the CLI will prompt you to enter your BIP39 mnemonic, which is your Ledger recovery phrase. YOU DO NOT WANT TO DO THIS. This will essentially save your private key locally.**

_Note: the commands below assume that you run them on the same machine where you have an Secret Network node running. However, if you need to connect to a remote Secret Network node (on the cloud) while you interact with your Ledger wallet locally, you will need to append the following to each command below:_

```
--node http://node.domain:26657
```

#### Send tokens <a href="#send-tokens" id="send-tokens"></a>

```
secretcli tx send <account name or address> <to_address> <amount> --ledger 
```

#### Delegate SCRT to a validator <a href="#delegate-scrt-to-a-validator" id="delegate-scrt-to-a-validator"></a>

```
secretcli tx staking delegate <validator address> <amount to bond> --from <account key> --ledger
```

#### Collect rewards and commission <a href="#collect-rewards-and-commission" id="collect-rewards-and-commission"></a>

```
secretcli tx distribution withdraw-all-rewards --from <account name> --gas auto --commission --ledger
```

#### Vote on proposals <a href="#vote-on-proposals" id="vote-on-proposals"></a>

```
secretcli tx gov vote <proposal-id> <vote> --from <account name> --ledger
```

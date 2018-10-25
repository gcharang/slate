# Settings, Parameters, Maintenance

## Accessing the Coin Daemon Remotely

> Contents of a KMD .conf file:

```
rpcuser=myusername
rpcpassword=myrpcpassword
server=1
rpcport=7771
addnode=78.47.196.146
addnode=5.9.102.210
addnode=178.63.69.164
addnode=88.198.65.74
addnode=5.9.122.241
addnode=144.76.94.3
```

To access a coin daemon remotely -- for example, via a `curl` command in the shell -- the user will need to obtain the `rpcuser`, `rpcpassword`, and `rpcport` from the `.conf` file of the relevant coin daemon.

Assuming the default installation location, the `.conf` file can be found by exploring the following directories:

`MacOS: ~/Library/Application Support/Komodo`

`Windows: C:\Users\myusername\AppData\Roaming\Komodo\`

`GNU/Linux: ~/.komodo`

Within that directory there are also subdirectories containing all KMD-compatible coin daemon `.conf` files.

## Manually Deleting Blockchain Data

Sometimes it is necessary to manually delete all blockchain data. This should automatically trigger a full resync of the blockchain.

Users should exercise caution not to delete the `wallet.dat` file during this procedure. We recommend that the user make frequent backups of the `wallet.dat` file, especially before deleting files from the application directory.

To erase all synced blockchain data, the following files should be deleted from the `.komodo` folder:

`blocks`

`chainstate`

`notarisations`

`komodostate`

`komodostate.ind`

**These files can typically be found in the default file locations:**

`MacOS: ~/Library/Application Support/Komodo`

`Windows: C:\Users\myusername\AppData\Roaming\Komodo\`

`GNU/Linux: ~/.komodo`

## Intro to Parameters and Settings

The following is an abbreviated list of runtime parameters and settings that can be initiated in a [coin daemon's .conf file](## Accessing the Coin Daemon Remotely).

These commands largely derive from the upstream Bitcoin software, `bitcoind`. (Komodo is a fork of Zcash, and Zcash is a privacy-centric fork of Bitcoin. Therefore essentially all commands available in both Bitcoin and Zcash are available in Komodo.)

To see additional runtime parameters not included here, please visit [the relevant Bitcoin wiki page](https://en.bitcoin.it/wiki/Running_Bitcoin).

## addressindex

> Using addressindex as a runtime parameter:

```
komodod -addressindex=1
```

> Using addressindex as a default value in the coin's .conf file:

```
  addressindex=1
```

`addressindex` instructs a KMD-based coin daemon to maintain an index of all addresses and balances.

The user should [manually delete the blockchain data](## Manually Deleting Blockchain Data) before initiating this parameter.

<aside class="notice">
[The `-reindex` parameter](### reindex) is not a viable alternative method for re-syncing the blockchain in this circumstance.
</aside>

`addressindex` is enabled by default on any asset chain that utilizes the CryptoConditions (CC) smart-contract protocol.

## txindex

`txindex` instructs a KMD-based coin daemon to track every transaction made on the relevant blockchain.

`txindex` is enabled by default on all KMD-based coin daemons, and is utilized in delayed Proof of Work (dPoW), JUMBLR, and the CryptoConditions (CC) smart-contract protocol. Disabling the feature will cause a normal KMD-based coin daemon to malfunction.

## reindex

> Using reindex as a runtime parameter:

```
  komodod -reindex
```

`reindex` instructs the daemon to re-index the currently synced blockchain data.

<aside class="notice">
Depending on the size and state of the chain you are re-indexing, this parameter may prolong the daemon launch time.
</aside>

## timestampindex

> Using timestampindex as a runtime parameter:

```
  ./komodod -timestampindex=1
```

> Using timestampindex as a default value in the coin's .conf file:

```
  timestampindex=1
```

`timestampindex` instructs a KMD-based coin daemon to maintain a timestamp index for all blockhashes.

The user should manually delete the blockchain data before initiating this parameter ===link to manual deletion instructions===.

<aside class="notice">
[The `-reindex` parameter](### reindex) is NOT a viable alternative method for re-syncing the blockchain in this circumstance.
</aside>

## spentindex

> Using spentindex as a runtime parameter:

```
  komodod -spentindex=1
```

> Using spentindex as a default value in the coin's `.conf` file:

```
  spentindex=1
```

`spentindex` instructs a KMD-based coin daemon to maintain a full index of all spent transactions (txids).

The user should [manually delete the blockchain data](## Manually Deleting Blockchain Data) before initiating this parameter.

`spentindex` is enabled by default on any asset chain that utilizes the CryptoConditions (CC) smart contract protocol.

<aside class="notice">
[The `-reindex` parameter](### reindex) is NOT a viable alternative method for re-syncing the blockchain in this circumstance.
</aside>

## regtest

> Using regtest as a runtime parameter:

```
komodod -ac_name=TEST -regtest
```

> Using regtest as a default value in the coin's .conf file:

```
regtest=0
```

`regtest` instructs the coin daemon to run a regression test network. Typically, the user will create a disposable asset chain for these purposes. The asset-chain coin supply parameter is not required in this instance.

(A regression-test network is a useful tool for rapid trial and testing. Please reach out to our team if you are curious to implement this tool in your workflow and are unfamiliar with how it is done.)

## bantime

> Using bantime as a runtime parameter:

```
  komodod -bantime=100000
```

> Using bantime as a default value in the coin's .conf file:

```
  bantime=100000
```

`bantime` sets the default number of seconds for a ban initiated during the daemon's session. The default is 86400.

## mempooltxinputlimit

** DEPRECATED **

`mempooltxinputlimit` is a runtime parameter inherited from Zcash. The functionality it facilitated is now enabled by default, and therefore the parameter is deprecated. Please see [the Zcash documentation for more information](https://blog.z.cash/new-release-1-1-0/).

## proxy

> Using proxy as a runtime parameter:

```
komodod -proxy=127.0.0.1:9050
```

> Using proxy as a default value in the coin's .conf file:

```
proxy=127.0.0.1:9050
```

`proxy` allows the user to connect via a `SOCKS5` proxy.

## bind

> Using bind as a runtime parameter:

```
  komodod -bind=127.0.0.1:9050
```

> Using bind as a default value in the coin's `.conf` file:

```
  bind=127.0.0.1:9050
```

`bind` instructs the coin daemon to bind to a given address and always listen on it.

Use `[host]:port` notation for IPv6.

## whitebind

> Using whitebind as a runtime parameter:

```
  komodod -whitebind=127.0.0.1:9050
```

> Using whitebind as a default value in the coin's `.conf` file:

```
  whitebind=127.0.0.1:9050
```

`whitelist` binds the daemon to a given address and whitelists peers connecting to it.

Use [host]:port notation for IPv6

## addnode

> Using addnode as a default value in the coin's .conf file:

```
  addnode=69.164.218.197
```

`addnode` tells the daemon which nodes are trusted to act as seed nodes. After connecting to a node via `addnode`, the trusted node will send your node the list of all nodes that it is connected to, and your node will then connect to these additional nodes until [the max limit](## maxconnections) is reached.

This contrasts from [the `connect` runtime parameter](## connect), as the latter does not attempt to connect your node to additional nodes.

If you are behind a firewall or are having issues connecting to the network, `addnode` is a stronger option.

On the other hand, if you want to connect only to designated and trusted nodes, `connect` is a stronger option.

If you run multiple nodes that are connected via a LAN, it is not necessary for each node to open multiple connections. Instead, use `connect` to connect all to one primary node, and then use `addnode` on the primary node to connect to the network.

The p2p port must not be blocked by a firewall. If the computers do not have public IP addresses, you will need to port-forward the p2p port on both computers and append the forwarded port to the IP.

For example:

`./komodod -ac_name=EXAMPLECHAIN -ac_supply=1000000 -addnode=<IP of the second node>:8096`

## connect

> Using connect as a default value in the coin's .conf file:

```
connect=69.164.218.197
```

`connect` connects the `komodod` server to a trusted peer node, but not to request or add any additional nodes.

Please refer to [the `addnode` parameter](## addnode) entry for more information.

## gen

> Using gen as a runtime parameter:

```
komodod -gen
```

> Using gen as a default value in the coin's .conf file:

```
gen=0
```

`gen` instructs the daemon to attempt to generate new blocks, and thereby mine new coins.

See also [`setgenerate`](## setgenerate).

## listen

> Using listen as a runtime parameter:

```
komodod -listen=1
```

> Using listen as a default value in the coin's `.conf` file:

```
listen=1
```

`listen` instructs the daemon to listen for RPC calls on the network. It is enabled by default, except when `connect` is used.

## maxconnections

`maxconnections` sets the maximum number of inbound and outbound connections.

> Using maxconnections as a runtime parameter:

```
komodod -maxconnections=NUMBER
```

> Using maxconnections as a default value in the coin's .conf file:

```
maxconnections=NUMBER
```

## server

> Using server as a runtime parameter:

```
  komodod -server=1
```

> Using server as a default value in the coin's .conf file:

```
  server=1
```

`server` instructs the daemon to accept json-rpc commands. It is enabled by default.

## rpcbind

> Using rpcbind as a runtime parameter:

```
komodod -rpcbind=127.0.0.1:9704
```

> Using rpcbind as a default value in the coin's .conf file:

```
  rpcbind=127.0.0.1:9704
```

`rpcbind` instructs the daemon to listen for json-rpc connections.

Use [host]:port notation for IPv6.

This option can be specified multiple times.

The default setting is to bind to all interfaces.

## rpcclienttimeout

> Using rpcclienttimeout as a runtime parameter:

```
komodod -rpcclienttimeout=SECONDS
```

> Using rpcclienttimeout as a default value in the coin's .conf file:

```
rpcclientttimeout=SECONDS
```

`rpcclienttimeout` indicates the number of seconds to wait for an rpc command to complete before killing the process.

## rpcallowip

> Using rpcallowip as a default value in the coin's .conf file:

```
  rpcallowip=10.1.1.34/255.255.255.0
  rpcallowip=1.2.3.4/24
  rpcallowip=2001:db8:85a3:0:0:8a2e:370:7334/96
```

`rpcallowip` tells the daemon which ip addresses are acceptable for receiving RPC commands.

By default, only RPC connections from localhost are allowed.

Specify as many `rpcallowip=` settings as you like to allow connections from other hosts, either as a single IPv4/IPv6 or with a subnet specification.

<aside class="notice">
Opening up the RPC port to hosts outside your local trusted network is NOT RECOMMENDED. Because the rpcpassword is transmitted over the network unencrypted, and also because anyone that can authenticate on the RPC port can steal your keys and take over the account running `komodod`.
</aside>

[For more information click here](https://github.com/zcash/zcash/issues/1497).

## rpcport

> Using rpcport as a default value in the coin's `.conf` file:

```
  rpcport=8232
```

`rpcport` tells the daemon to listen for RPC connections on the indicated TCP port.

## rpcconnect

> Using rpcconnect as a default value in the coin's `.conf` file:

```
rpcconnect=127.0.0.1
```

`rpcconnect` allows the user to connect to `komodod` and send RPC commands from a host. By default, it is set to localhost. We DO NOT RECOMMEND that the average user set this value to anything other than the localhost, as it can grant access to a foreign party, who are then able to take control over `komodod` and all funds in your `wallet.dat` file.

## sendfreetransactions

> Using sendfreetransactions as a default value in the coin's .conf file:

```
  sendfreetransactions=0
```

`sendfreetransactions` instructs the daemon to send transactions as zero-fee transactions if possible. The default value is 0.

## genproclimit

> Using genproclimit as a default value in the coin's .conf file:

```
  genproclimit=1
```

`genproclimit` sets the number of threads to be used for mining. To initiate all cores, use the value `-1`.

## keypool

> Using keypool as a default value in the coin's .conf file:

```
keypool=100
```

`keypool` instructs the daemon to pre-generate a certain number of public/private key pairs. This can facilitate `wallet.dat` backups being valid for both prior transactions and several dozen future transactions.

## rewind

> Using rewind as a runtime parameter:

```
komodod -rewind=777777
```

`rewind` rewinds the chain to specific block height. This is useful for creating snapshots at a given block height.

## stopat

> Using stopat as a runtime parameter:

```
  komodod -stopat=1000000
```

`stopat` stops the chain at a specific block height. This is useful for creating snapshots at a given block height.

## pubkey

> Using pubkey as a default value in the coin's `.conf` file:

```
pubkey=027dc7b5cfb5efca96674b45e9fda18df069d040b9fd9ff32c35df56005e330392
```

> Using pubkey as a startup parameter:

```
-pubkey=027dc7b5cfb5efca96674b45e9fda18df069d040b9fd9ff32c35df56005e330392
```

`pubkey` sets an address to use as a change address for all transactions. This value must be set to a 33 byte pubkey. All mined coins will also be sent to this address. We recommend that the user ensure they own the relevant `privkey` of their chosen `pubkey`, lest their funds be sent to a `pubkey` they do not own or control.

The `pubkey` parameter is required for all CryptoConditions (CC) smart-contract enabled chains. All smart-contract transactions will utilize the `pubkey` as an integral property.

## exchange

> Using exchange as a default value in the coin's .conf file:

```
exchange=1
```

`exchange` forfeits all user rewards to miners. Set this to explicitly not claim user rewards.

## donation

> Using donation as a default value in the coin's .conf file:

```
donation=027dc7b5cfb5efca96674b45e9fda18df069d040b9fd9ff32c35df56005e330392
```

`donation` donates all user rewards to a specific address. This value must be set to a 33 byte pubkey.

## exportdir

> Using exportdir as a default value in the coin's `.conf` file:

```
  exportdir=/home/myusername/mydirectory
```

`exportdir` tells the coin daemon where to store your export directory.

# lolMiner 1.20

## Using the dual & splitting modes

### Use case A: Mine ZIL&ETH or ZIL&ETC on different pools

Usually when mining ZIL you need to mine ETH on the same pool or you need to rely on a pool proxy forwarding mechanism implemented by the pool. The first case restricts restricts your mining to a single pool while the latter might have the disadvantage of a worse ETH mining latency or pool forwarding instabilities. lolMiner 1.20 and up allow to bypass the situation by adding a second stratum connection that will pick up your ETH (or ETC) shares and bring them directly to the pool you like, while the ZIL shares will be send during the ZIL shard epochs to the ZIL pool.  

To configure this follow the following steps

a) Configure your ZIL mining as normal. In case you want to use ETC+ZIL select ETCHASH as algorithm parameter. The configuration needs to be complete, that means if you stop after this point you should use normal ZIL+ETH or ZIL+ETC dual mining as you are used to.
b) Add the parameter --dualmode zil --dualstratum *ETHWALLET*.*ETHWORKER*@*ETHPOOL*:*ETHPORT* to your command line arguments or your extra user parameters. Replace here the elements in *ETHWALLET*, *ETHWORKER*, *ETHPOOL* and *ETHPORT* with your desired ETH mining credentials. Note that <ETHSTRATUM> understands prefixes like "tls://" to activate ssl on the additional stratum connection.

Now the miner will create both connections on startup, but will mine the ETH (or ETC) shares on the extra connection, which can be different to the first one, which will only be used when mining ZIL. Note that the parameter will automatically enabling the ZIL cache mode on your 6 & 8G cards. 

### Use case B: Mine ETH on 8G cards while mining ETC on 4G cards

Usually miners allow using only one algorithm at a time. With lolMiner 1.20 the miner starts supporting to create two connections to your favorite pools and mine two algorithms within the same miner instance. Concretely this mode was build to mine ETCHASH on some GPUS while others stay on ETH. 

To configure this follow the following steps:

a) Configure your ETH mining as normal, no further settings are required.
b) Add the parameters --dualmode etc --dualstratum *ETCWALLET*.*ETCWORKER*@*ETCPOOL*:*ETCPORT* to your command line arguments or your extra user parameters. Replace here the elements in *ETCWALLET*, *ETCWORKER*, *ETCPOOL* and *ETCPORT* with your desired ETC mining credentials. Note that <ETCSTRATUM> understands prefixes like "tls://" to activate ssl on the additional stratum connection.
c) (Optionally) You can use --dualdevices to select those devices that shall mine ETC instead of ETH. The default is that all 3G and 4G cards will be pointed to the secondary algorithm, while the other cards remain on the primary one. The parameter takes a comma separated list of numbers, which needs to be a subset of the devices running. For example the combination of --devices 0,1,2,4,5 --dualdevices 4,5 will cause Cards 0,1 and 2 to mine the first algorithm, cards 4 and 5 the second algorithm and card 3 to be skipped from mining. 

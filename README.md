# lolMiner

A git repository for lolMiner release versions

Helpful information in : https://github.com/Lolliedieb/lolMiner-releases/wiki

## Supported Algorithm

| Algorithm  | Fee % |
| ------------- | ------------- |
| BeamHash I | 1.0  | 
| BeamHash II | 1.0  |
| BeamHash III | 1.0  |
| Cuckoo 29 | 2.0  |
| CuckarooD 29 | 2.0 |
| CuckarooM 29 | 2.0 | 
| Cuckaroo 30 CTXC | 2.5 |
| Cuckatoo 31 | 2.0 |
| Cuckatoo 32 | 2.0 |
| Cuckaroo 29-32 | 1.0 |
| Cuckaroo 29-40 | 1.0 |
| Equihash 144/5 | 1.0 |     
| Equihash 192/7 | 1.0 |
| Equihash 210/9 | 1.0 |
| Etchash | 0.7  |
| Ethash | 0.7 |
| ZelHash | 1.0 |

## Recent Changelog:

### lolMiner 1.27

- Added verify routine for Ethash dag epochs 400 to 450. In case the miner will detect defect entries, the CPU will try to fix this. Mining will be paused until the repair is completed. Use --disable-dag-verify to disable the verify & repair mechanism routine.
- Re-worked default Ethash kernels for Pascal GPUs - improved their performance
- Added Ethash kernels for Fermi and Kepler GPUs. Most of them will only work for small epoch Eth forks.
- Nvidia cards on Ethash now pause when the stratum reports no current work (e.g. when connection was lost).
- Added a split DAG mode for Nvidia GPUs in case that the memory allocation fails on the primary kernels. This will be a bit slower, but improve compatibility, especially for 5G GPUs. Use --mode s to force it.
- Added parameter --cclk to fix the core clock of Nvidia Turing and newer GPUs without using external tools. Use a comma separated list to give different values to cards, use * to skip over cards. Needs super user or administrator privileges to work. See a detailed description here: https://github.com/Lolliedieb/lolMiner-releases/wiki/Fix-Clock-Nvidia-for-Cuda-by-lolMiner-(English)
- Added new dualmode zilEx. This works like --dualmode zil but with the ability to use --dualdevices to exclude GPUs from switching to ZIL. They will continue on the secondary connection and do not switch.
- Added new dualmode eth. This will allow to point different GPUs to different pools.
- Added parameter --statsformat to use custom format for the bigger statistic box. The expected values are either compact or default or extended or a comma separated list of values. Use --help-format to get a list with accepted entries. The list is also documented at https://github.com/Lolliedieb/lolMiner-releases/wiki/Stats-Format-(English)
- Added reading of current core and memory clocks for AMD and Nvidia GPUs

_Fixes_

Fixed a stratum error, that caused the "all shares stale" bug when too many reconnect attempts in a row did fail
Fixed a crash on Nvidia GPUs when mixing ethproxy and Nicehash stratum modes in dualmodes.
Fixed zombie tune values not applied when using json format for configuring
Fixed displayed names of RX 6000 generation of cards and RTX 3060 in 460.x drivers.

_Deprecation notice_

This will be the last release that allows to use the Zombie-Mode in Windows. This is because the current implementation will start throwing invalid shares at some point and also going too slow to be worth using it any more.
Furthermore Cuckaroo-29 with 48 cycle length - originally planned for Italocoin - got removed because of the lack of use.

### lolMiner 1.26

- Slightly improved performance of Ethash on Pascal / Turing & Ampere GPUs (about +0.1 - 0.2 mh per card)
- Further reduced internal latency in Ethash Cuda back end (less stale shares & CPU load)
- Added experimental Grin-C32 kernel for Radeon 6700

_Fixes_
- Fixed a bug in Cuda back-end to crash with a segfault on Epoch change (introduced in 1.25)
- Fixed a bug that Nvidia-GPUs did not start when Cuda Toolkit was not installed (It reported "No OpenCL devices found ..." - which was complete nonsense)
- Fixed some (rare) potential faults in Beam stratum
- Windows: Fixed message about Light Cache build time was missing (introduced in 1.25)


### lolMiner 1.25 goes CUDA!
Added a real Cuda back-end for better Nvidia GPU support on Ethash. Features:
- Supports Maxwell to Ampere GPU generations.
- Two different mining kernels. Use --mode a (faster) --mode b (better energy efficiency) to select between the two. The selection can be done per card via a comma separated list. In mixed system select 'a' for skipping over the AMD cards. 
- Both kernel modes need less energy and perform better then in 1.24a
- Reduced internal latency for less stale shares
- Reduced CPU load when mining with Nvidia cards
- lolMiner works now without OpenCL driver installed
- In case of mixed rigs AMD GPUs will use OpenCL while Nvidia cards use Cuda
- ZIL cache feature fully supported (and stable)
- Temperature stop & Zombie mode is currently not supported

_Further Changes (over 1.24a)_
- Added Ethash, Beam Hash III, Grin Cuckatoo 32 and Cortex kernels for RX 6700
- The Ethash stratum interface will now try to run up to three attempts of reconnecting before switching the stratum mode

_Bug fixes_
- Fixed "Warning: index out of bounds" error when switching from ETHPROXY to ETHV1 stratum mode. This might solve problems with some pools on connection loss. 

_Recommendations for Cuda backend_
- Recommendation: When using lolMiner on Nvidia cards only use "--watchdog exit" mode and run the miner in a script that will automatically restart it on closing. 
- For ideal efficiency fix the core clock, do not use the offset functionality. Recommended values for selected GPUs:

| GPU        | Range       | 
| ------------- |:-------------:| 
| 2070 | 1000 - 1050 |
| 2080 | 1110 - 1160 |
| 3060 (1) | 1070 - 1120 |
| 3060ti | 1300 - 1350 |
| 3070 | 750 - 800 |
| 3080 | 1010 - 1060 |

(1) Using Windows and Nvidia Driver 470.05 Beta

### lolMiner 1.24a
lolMiner 1.22 - 1.24 are **Linux only** releases that targets improvements of the performance of the zombie mode in the Linux specific code. Therefore the yesterday released version **1.21 will remain the recent release for Windows**. Miners that do not have a card using the zombie mode can safely ignore this update - it will behave identical to 1.21.
- Added (tunable) zombie mode kernels for R9 290(x) and  R9 295 GPUs - on a popular request.

_Bug Fixes_
- Fixed a bug, that often caused the amdgpu driver to report a VM_CONTEXT1_PROTECTION_FAULT_STATUS on startup
- Fixed defect shares and wrong reported has hrate when started with fixed --zombie-tune parameters directly
- Fixed a bug with Baffin (RX 450,460, 550, 560) and Tonga (R9 380(X) ) GPUs showing too high hashrate and producing invalids in 1.23 zombie mode.
- Fixed a bug with ETC mining not starting up when more then two 4G GPUs  in 1.23.

_Personal release notes_
I received quite some requests with problems about Nvidia cards and also if I can add in zombie mode kernels for 4G Nvidias and RX 5500. I need to say I tried, but there are some hurdles that prevented it. I will do better Nvidia codes in the future, but preparing it takes time. 
That said the Navi cards somehow to not like to zombie tuning at all, that is why they only feature the standard zombie mode. Currently I also can not recommend mixing them into rigs where RX 4xx and 5xx cards run in zombie mode, because that seems to cause stuck systems from time to time.  Mixing with 8G cards and do normal mining seems not to be an issue though. 


### lolMiner 1.23
lolMiner 1.22 & 1.23 are **Linux only** releases that targets improvements of the performance of the zombie mode in the Linux specific code. Therefore the yesterday released version **1.21 will remain the recent release for Windows**. Miners that do not have a card using the zombie mode can safely ignore this update - it will behave identical to 1.21.

### lolMiner 1.22
lolMiner 1.22 is a **Linux only** release that targets improvements of the performance of the zombie mode in the Linux specific code. Therefore the yesterday released version **1.21 will remain the recent release for Windows**. Miners that do not have a card using the zombie mode can safely ignore this update - it will behave identical to 1.21.

- Significantly improved the performance of zombie mode on RX 400 and RX 500 GPUs in Linux, especially for low zombie tune values between 0 and 4 and rather high epochs. Performance increases by 7-11% on epoch 393 (--4g-alloc-size 4080 on a RX 580. 4G) and 15-20% on epoch 400. Re-tuning using the auto-tune is recommended. Also this version might draw a bit more power, but with approximately same total efficiency.

### lolMiner 1.21
- slightly improved the performance of Linux zombie mode on Polaris GPUs on medium tune stages (needs re-tuning from previous settings)
- increased range of accepted zombie tune parameter for GPUs with high interconnect bandwidth
- slightly decreased GPU load of Polaris GPUs during DAG build
- Added more control about handling cards that are detected to be non-working any more. Use parameter **--watchdog off/exit/script** to turn off any action, exit the miner with a specific exit code or to run an external script. See detail description on the 1.21 release page
- Nvidia cards that experienced a OpenCL driver error (e.g. "CL_OUT_OF_RESOURCES" will now also trigger the watchdog with the configured effect.
- The **--ethstratum** parameter can now take two options separated by a ',' to give different options in case the dual or split mining mode is used.
- The dns resolving and the connection attempt can now timeout (after 10 seconds each) and will re-try to connect afterwards. This fixes an issue when a pool went offline and the following connection attempt takes indefinitely much time. Each timeout event contributes to the counter that will trigger switching to fail-over pools.
- New option **--apihost** (default 0.0.0.0) which controls to which host address the api binds. Use 127.0.0.1 to restrict api access to only your computer, 0.0.0.0 is equivalent to everyone can access when rig is reachable on the used apiport. IPV6 ip addresses should be supported, but is untested. 


_Fixes_
- Fixed a issue that might cause the rig to drop to 0 hash rate on epoch changes - including changes with activated ZIL caching
- Fixed the pool hash rate reporting not working correctly in dual & split stratum modes
- Fixed the dual stratum connection not picking up the correct worker name when --worker is used
- Fixed miner not loading Ethash / Etchash kernels on Tahiti and Hawaii GPUs when using older then end 2017 drivers. 



_Note on Watchdog use_
There are different reasons why a card might crash and drop to 0 mh/s or g/s or sol/s. Often this happens when the card is slightly too much undervolted, but other problems like heat are possible. Additionally the OpenCL driver of Nvidia cards sometimes crashes with a CL_OUT_OF_RESOURCE error - this is rather a software then a hardware thing and will be fixed soon. 
Anyways: Once a card is crashed some cards - mostly AMD cards - need a system reboot to get the faults card working again. Other cards - mostly Nvidia - just need a closing of the miner program - a few seconds wait time - and then are fine to get going again.
Therefore the crashed card detection now allows three different options to proceed with a crashed card or driver:

**--watchdog off**
This will do nothing except for printing a message. If only a single card did crash and not the whole driver this means the other cards will continue mining.

**--watchdog exit**
This will close the miner with a exit code of 42. This can be picked up by the .sh or .bat script that did start the miner (an example is provided in mine_eth.sh and mine_eth.bat) so the miner will restart after some seconds of pause. This is recommended option for Nvidia cards.

**--watchdog script** 
With this option the miner will call an external script (default path is current working directory and there emergency.sh / .bat), which can be configured with --watchdogscript. The moment the script is called the miner itself will exit. The script needs to take care about rebooting the rig or informing the OS what to do. Since this was the default behavior in previous versions it also is the default. In case the script can not be found, an error will be printed and the miner will continue as with --watchdog off.

### lolMiner 1.20
- Significantly improved **Ethash** mining speed on **R9 390** (+6 mh/s on stock settings compared to 1.19) and **Etchash** speed on **R9 290**. 
- Added new **split & dual mining** options. This allows more freedom or better latency and stability on ETH+ZIL dual mining as well as split mining, i.e. let some cards mine ETH while other (3 and 4G) cards mine ETC. Read instructions on usage here: 
https://github.com/Lolliedieb/lolMiner-releases/blob/master/dual_and_split_mining.md
- The archives for ZIL example files now contain examples how to bypass the ZIL pools. Also an example configuration for ETH / ETC card split is provided.

_Bug Fixes_
- Fixed a bug with 4G cards crash on mining ETC when trying to falsely enter zombie-tune. 
- Fixed R9 380 cards not start mining Beam
- Fixed "Address already in use" API bug in Linux (that incidentally got introduced in 1.19)

_About the split mining_
There are two new splitting modes. Read here for configuration: 
https://github.com/Lolliedieb/lolMiner-releases/blob/master/dual_and_split_mining.md

a) For ETH+ZIL or ETC+ZIL:  
Usually when mining ZIL you need to mine ETH on the same pool or you need to rely on a pool proxy forwarding mechanism implemented by the pool. The first case restricts restricts your mining to a single pool while the latter might have the disadvantage of a worse ETH mining latency or pool forwarding instabilities. lolMiner 1.20 and up allow to bypass the situation by adding a second stratum connection that will pick up your ETH (or ETC) shares and bring them directly to the pool you like, while the ZIL shares will be send during the ZIL shard epochs to the ZIL pool.

b) For mining an other algorithm with your 4G cards:
Usually miners allow using only one algorithm at a time. With lolMiner 1.20 the miner starts supporting to create two connections to your favorite pools and mine two algorithms within the same miner instance. Concretely this mode was build to mine ETCHASH on some GPUS while others stay on ETH.

![grafik](https://user-images.githubusercontent.com/40234439/105478542-98540580-5ca3-11eb-9347-76aa7e85b78a.png)


### lolMiner 1.19
- Added automatic tuning mode for --zombie-tune. This is **default on**, so just run the miner with --4g-alloc-size set only to run the zombie mode automatic tuning. At the end it will report the configuration in case you want to use the configuration again. 
You can also exclude cards from tuning or set their value manually, e.g. --zombie-tune 2,auto,0,auto will run the automatic tuning on the 2nd and 4th GPU while using fixed number 2 for first card and 0 for the 3rd one.
The tuning will need about 30 seconds per card in the rig to show first results. The next two phases take about 1 minute per card and followed by a approximately 1.5 minutes fine tune phase. 

- Ethash stratum connection will now reconnect after three pool rejected shares in a row that did pass own CPU verify before. This solves issues with unstable proxy forwarding e.g. in some ZIL pools. Also helps to get quicker to a failover pool if configured.

_Fixed bugs_
- Miner did not start up when "DEVICES" was configured in as a vector in json file, e.g. in some ETHOS configurations. #110 


### lolMiner 1.18a
- Improved linux zombie mode power draw & speed Polaris GPUs (R9 380, RX Fury, RX 4x0 and RX 5x0). Depending on configuration, the zombie mode now uses 0.5 to 1W less energy and is 0.2 to 0.4 mh/s faster.
- Added --zombie-tune <number> parameter for Polaris GPUs. This will increase the performance of zombie mode (further up on the general improvement) by an other 5-15%, depending on parameter and epoch (later epochs profit more). Default value is 0 (off), for most cards the value of 2 is optimal. If you see cards getting slower then before, set to 0 or 1. Note: you either can give one value for the whole rig or provide a comma separated list for each card individually. Cards not running zombie mode ignore the parameter.
- The parameter --4g-alloc-size can now also be set for each card individually
- Slight rework of Beam Hash III back end. Improves poolside hash rate by approx 0.2 to 0.3% - displayed hashrate and power consume kept equal. 
- Added a 4G_Ethash_Linux_Readme.txt file to the Linux release, giving guidance how to configure for ideal zombie mode performance.
See online version: https://github.com/Lolliedieb/lolMiner-releases/blob/master/4G_Ethash_Linux_Readme.md

_Bug fixes_
- Fixed: segmentation fault when the dns resolve of a pool fails #109 
- Fixed: miner does not restart after connection loss. #108 
- Applied potential fix for "address or port already in use" bug.

_Compatibility note:_
The new zombie-tune parameter has only been tested with amdgpu-pro 20.30 and 20.40. Other drivers might cause issues. 
Also make sure your mining rig is configured to use PCIe-gen 2 connection to your GPUs.

_Example effect of --zombie-tune parameter on the hash rate in Zombie Mode_
Note: it may be needed to tune each card individually. Tune value of 2 works for most cards, but some do not like the mode, especially when on PCIe-gen1 riser. 
![ztune](https://user-images.githubusercontent.com/40234439/103769382-6d6f7d80-5024-11eb-9022-c63a40651672.png)


### lolMiner 1.17
- Significantly reduced Ethash power draw on Navi GPUs, Slightly improved performance of 6800 (XT) / 6900
- Added Cuckoo-29, Cuckaroo-30 CTX, Cuckatoo-31 (MWC) and Cuckatoo-32 (Grin) for RX 6800 family of GPUs
- Reduced number of stale shares on Cortex algorithm. This will result in a minimally lower displayed hash rate, but higher pool side hash.
- Added a basic temperature protection mechanism. See notes below for usage.
- Added parameter **--singlethread** to work with Ethash and Equihash algorithm. This will disable the 2nd mining thread and slightly reduce performance of the involved cards. Use this option to reduce stumbles when a card does graphic output in parallel. Use **--singlethread** (equivalent to **--singlethread -1**) to enable single thread mode for all GPUs, use **--singlethread <gpu id>** to set the mode for one single card.
- Added reading of junction temperature on AMD GPUs.
- The API is now bound to the local host, causing less issues with firewalls.
- Windows: use **--computemode** to automatically enable the compute mode on all detected AMD GPUs. Requires higher privileges and a driver restart, see example files.
- lolMiner.exe Windows executable is now digitally signed.

_Fixed bugs:_
- Ethash Ethproxy stratum mode some times loosing worker name.
- Beam Hash III not starting up in Linux on RX 5000 & RX 6000 series card on amdgpu-pro 20.45 driver.
- Ethash & Beam not starting up on Radeon R9 380 
- Ethash not starting up on some 6G Nvidia cards
- Ethash mining frequently trying to start up a card if there was an error during mining. 
- "DEVICES" parameter not working when configuring the miner from json file.
 
_Known issues:_
- ETC mining is currently not working for Nvidia GTX cards with 3G of memory. 
- On some Linux kernels there is a memory leak in the GPU driver component that effects lolMiner quite hard when mining with Navi cards. Keep system updated. (Note that this bug also affected earlier versions of lolMiner - so if it worked fine in last version it can be expected to do so further.)

_Basic temperature management / overheating protection. _
Use **--tstop <number>** to stop any mining operation on a GPU at the given temperature. Use **--tstart <number>** to allow a restart of the card below a lower temperature. Further you can use **--tmode edge/junction/memory** to apply the scheme to edge (chip), junction (hotspot) or memory temperature. If a GPU does not have the required sensors the chip temperature will be used as a back up - if no sensors are available at all the parameters will be ignored. 

Note that at the moment the miner has no fan control module and also no throttling to keep a target temperature. This may be included in a future version. Thus you should put the limit high enough so the operation system or the driver has a chance to ramp up the fan speed itself. Currently tstop is supposed to be a overheat protection to prevent hardware damage in extreme cases, e.g. broken fans.

### lolMiner 1.16a
- Fixed performance regression on Nvidia cards
- Added support of Ethash and Beam Hash III for RX 6000 generation of GPUs
- All supported algorithms now show the share difficulty and have best share statistics. 
- New feature: use **--rebuild-defect _n_** to trigger a rebuild of DAG if a GPU produced _n_ defect shares on the current one. Default is 3, use 0 to deactivate this feature. 
- New feature: Use **--workmulti _n_** to modify the amount of Ethash work a GPU does per batch. Higher values will cause less CPU load but more stale shares, lower values will give less stale shares but higher CPU load. Performance may vary for different values. Default is 128.  
- New feature: if Ethash pool disconnects within 2 seconds from connection attempt (immediate reconnect), then the other stratum mode is tested to login.
- New feature: AMD Vega and newer cards now display memory temperature in statistics & api (only visible if there is at least one such GPU in the rig). 
- Default ethstratum changed from ETHV1 to ETHPROXY for better pool compatibility. 
- Stratum pool addresses now understand "stratum+tcp://", "stratum+ssl://" and "ssl://" prefixes (turning on or of ssl / tls automatically) for better compatibility with existing configurations.   
- Slightly reduced CPU load when mining Ethash
- New coloring scheme with more friendly colors. For terminals that do not have rgb colors (e.g. shellinabox) use **--basecolor** to restrict to a simpler set. Use **--nocolor** to deactivate coloring completely. 
- Fixed bug: Cards may crash when switching from ZIL cache back to normal mining.
- Fixed bug: Wavy hashrate - especially for rigs with many AMD Navi GPUs.
- Fixed bug: (Linux:) Watchdog not called when a GPU is stuck & extremely high CPU load on crashed GPU. (1)
- Fixed bug: Hashrate reporting not working on some pools (e.g. sparkpool)
- Fixed bug: Miner can crash after trying to reconnect to same pool over 5 minutes. 
- Fixed bug: Miner crashes when mixing TLS and non-TLS pools for fail-over. 

(1) Note on watchdog use: When the watchdog script is called the miner will stop working on the other cards. If this is not wished use --disablewatchdog. Please make sure the script can be executed with the current user rights / does password-less operations. 

### lolMiner 1.15
- Fixed bug: Miner causing invalid shares on 4G cards on some systems (mostly Linux)
- Fixed bug: Miner hangs up when changing epoch when using the ZIL cache feature
- Fixed bug: Miner sometimes produces invalid shares when a new job with different epoch arrives while the miner is currently creating the DAG file for an earlier job.
- Fixed bug: Miner not calling the default emergency scripts when a GPU was hung up (it only worked with custom scripts)
- Improved Ethash efficiency on Nvidia GPUs
- ZIL cache now can be used by cards with less then 8G when enough memory is available (e.g. 6G cards or when mining e.g. ETP + ZIL)
- General stability improvements (resolved many potential miner hangs up causes)
- Changed color scheme in Windows for Ethash mining (new jobs are now white, accepted shares green)

### lolMiner 1.14
- Added Ethash Zombie mode for 4G Nvidia GPUs. Use --4g-alloc-size to calibrate the number of MBytes the GPUs are allowed to use. 
- Fixed a segmentation fault on Nvidia & mixed rigs when starting Ethash mining

### lolMiner 1.13
- Ethash: Reduced power draw significantly on  non-zombie mode for Rx Fury & Rx 470 - 590, slight reduction for Vega &  Navi
- Ethash: Slightly improved performance on Vega, Navi and Nvidia GPUs. Significantly improved performance on R9 390. (1)
- Added (Linux) Zombie mode for RX 5300XT & RX 5500 4G cards. Windows users can try it by using "--win4galloc off --4g-alloc-size 4008". (Vary the last number to find out sweet spot)
- Added ETCHash support for Radeon HD 79x0 / R9 280 (X) & RX 5300 3G. On Linux will be good for ETCHash till epoch ~250 (about July 2022) 
- Added caching of last 5 used light caches. This will reduce the switching time for Nicehash & ZIL dual mining significantly. 
- Added support for extranonce subscription on  EthereumStratum/1.0.0 (Nicehash) format - this will stop the miner from frequently reconnecting to Nicehash
- Added detection of pool not accepting worker name in <wallet.workerName> format when using ETHPROXY stratum. Miner will reconnect with worker name copied into --worker in this case. 
- Added experimental workaround for mining epoch 385+ with RX 470 - 590 and Linux kernel 5.6.x: Note this fix will deactivate the ZIL cache ability and force the miner to create DAG a bit slower. Deactivate it with --disableLinux56fix . Other Linux kernel versions and other GPUs are unchanged.
- Fixed bug: "conversion of data to type "b" failed" when using ETHPROXY stratum mode on some pools.
- Fixed potential issue causing GPUs to freeze when a GPU needs to reboot, e.g. epoch change or connection loss.
- Fixed benchmark mode for ETCHash. Use --benchmark ETCHASH --benchepoch 390 to benchmark performance post fork.
- Fixed benchmark mode not starting up when called from json type configuration.

(1)  (its still not perfect, but way better)


### lolMiner 1.12
- Added support for ETCHash (Ethereum Classic dag size reduction planned for end November). Use --algo ETCHASH to activate it. (See note on release page).
- Reworked Ethash codes for late epochs on Windows. See 4G_Windows_Readme.txt for configuring it.
- Slightly improved Ethash efficiency for GCN 3 (R9 Fury, 470 - 590) & Navi cards
- Added experimental support for Ethash on Nvidia GPUs (See note on release page)
- Added new parameter: --4g-alloc-size <MByte> to define the memory allowed for Ethash on 4G cards. Maxing out will give more epochs of mining & better Zombie mode performance, lower values may improve compatibility. Suggested values: Linux: 4076, Windows 4008 - 4024
- Added new parameter: --worker <Name> to set the worker in ETHPROXY stratum mode (improves pool compatibility)
- Overall new Ethash host size back end - hopefully improving stability of mining
- Fixed bug: Zombie mode generates defect shares in Windows
- Fixed bug: Logs were not written when "LOG" : 1 was set in json style config file



### lolMiner 1.0

- lolMiner got a restructure how to configure it and also features a 2nd, more simple config file format.
  Use lolMiner -h to get a list of new supported parameters or visit the new [online manual](https://github.com/Lolliedieb/lolMiner-releases/wiki)
- Added optimizes solvers for Beam Hash III for AMD & Nvidia cards. Use *--coin BEAM* to auto switch from BeamHash II to BeamHash III on fork (approx June 28th, requires 8G card) or select Beam Hash III solver manually with *--algo BEAM-III* (requires 6G card)
- Added performance improved (+ >10%) GRIN-C29M solver for 8G GPUs
- Added Cuckaroo-30 solver to mine Cortex Ai (*--coin CTXC* or *--algo C30CTX*) for all 8G and higher GPUs
- Added support for non-integer difficulty on Grin
- Reactivated support for Beam Hash I including support for personalization strings. 
- AMD Navi does now work on all supported algorithms
- Removed Grin Auto-Switcher (C31 is obsolete now and the switcher would not work on next Grin fork)
- Removed support for MNX (Minex Coin project is dead / abandoned by developers)  
- Added temperature, consumption and fan speed readings in API and long statistics
- Internal bug fixes

### lolMiner 0.9.8
- Improved GRIN-C29M solver, Better performance (+7-12% depending on card) and smaller memory use (fits 6G now)
- Added support for Radeon 5500 (XT) and 5600 (XT) on all Grin algorithms 
- Added BEAM support for Radeon 5500, 5600 and 5700 series (Needing 19.30 and newer driver)
- Fixed driver incompatibilities with some newer driver versions (e.g. Linux 19.50) 
- Renamed GRIN-AD29 to MWC-C29D to mine cuckarood-29
- Added a small penalty to C32 in auto switcher - it seems the solver has not perfect fidelity and the penalty will make it switch a bit later to cover this. Will hopefully be fixed in next version
- Removed 14 cycle fidelity from overview - new C29M solver does not output it properly.

### lolMiner 0.9.6
- Significant improvement on GRIN-C29M performance (+6-7% on 580 and Vega cards, +10% on Navi)
- Significant improvement on GRIN-C31 and GRIN-C32 solver (~ +6% on all AMD cards)
- Added a 16G GRIN-C32 solver (Approx 20% faster on Radeon VII, Vega FE and 570 16G)

### lolMiner 0.9.5
- Added support for Grin CuckarooM-29 (hard fork on Jan 16th) on 8G AMD GPUs, use --coin GRIN-C29M to mine with it.
- Improved C31 performance on 8G cards by ~5% (Windows: relatively to 0.9.3, Linux: relatively to 0.9.4)
- Navi on Windows now runs same kernels as in Linux
- Slightly lowered energy use of C31 / C32 solver
- Added aliases GRIN-C31 and GRIN-C32 for the Cuckatoo solvers (the old names still exist and continue to work)

### lolMiner 0.9.4
- New GRIN-AT31 performance code for Vega (+7%) and Navi (+12%). Requires amdgpu-pro 18.50 or newer or ROCm 2.10 driver
- Experimental support for Cuckatoo-32 (use --coin GRIN-AT32) on 8G AMD cards (see further notes on releases page)
- Windows release postponed due to incompatibilities with the new performance codes. 

### lolMiner 0.9.3
- Extended GRIN-AT31 compatibility to older drivers (18.x +) 
- Improved GRIN-AT31 performance on ROCm (RX 470/480/570/580/Vega/VII)
- Introduced early job cancellation for GRIN-AT31+ (improves hash on pool side, see further release notes on releases page)
- Deeply reworked kernel scheduler
- Fixed GRIN-AT31 kernel bugs (improving stability and fidelity)
- Fixed Bug: Vega FE loading 8G instead of 16G GRIN-AT31 solver in Windows
- Fixed Bug: Watchdog did not call right file in Windows
- Added --disablewatchdog 1 parameter to disable the 0 sol/s  /  0 g/s detection

### lolMiner 0.9.2
- Significant performance improvement of GRIN-AT31 on 8 / 16G cards (+5% on Polaris & Vega, +10 on Navi)
- Experimental support for GRIN-AT31 and Polaris, Vega and VII using AMD ROCm drivers
- Added range checks to GRIN-AT31 code (improves better stability) 
- Added function to call external watchdog scripts in case a GPU fails during mining (see release notes)

### Usage of Watchdog Script
In case the miner detects no action of a GPU for at least a minute it will call the included scripts "reboot.sh" (Linux) or "reboot.bat" (Windows) and display a warning message in red. Afterwards the counters are reset. The scripts can be used to trigger a reboot of the rig or to call any other watchdog actions. The miner itself will take no further action and continue operation on the remaining cards.


### lolMiner 0.9.1
- Added GRIN-AT31 solver for 16G AMD cards (Better performance on Radeon Vega FE, Radeon VII and Sapphire RX 570 16G) 
- Updated GRIN-AT31 solver for 4G AMD cards (Better performance on Fiji based GPUs, Polaris 10 4G)
- Fixed a bug causing too low pool hash on GRIN-AT31
- Added experimental GRIN-AT31 support for AMD Navi (8G), ~~AMD Fiji (4G)~~ and AMD Hawaii (4G / 8G) GPUs

Edit: support for Fiji was broken, A fix can be found here: 
https://github.com/Lolliedieb/lolMiner-preview/releases/tag/0.9.1hotfix

### lolMiner 0.9
- Significant performance improvement for GRIN-AT31 on 8G AMD cards (+22% on Polaris to 30% on Radeon VII)
- Disabled 16G solver for GRIN-AT31 (the 8G is faster at the moment)
- Reduced Grin stale shares
- In command line lolMiner now accepts --pool address:port pattern
- Fixed a bug with the API crashing when accessed by Chrome based browsers
- Fixed a bug in EXCC stratum not passing number of submitted shares to the API


### lolMiner 0.8.7
- Added support for Beam Hash II on older AMD cards (R 9 200 / 300 4 & 8G cards;  R9 280(X), HD 79x0 with 3G). Note that the auto-switcher is only working with at least 4G, for the 3G cards please use --coin BEAM-II manually on fork height.
- Fixed an issue with Genesis Network (GENX) missing in lolMiner 0.8.x

### lolMiner 0.8.6
- Added support the BEAM hard fork on block 321321 (approx Aug15th).
- Fixed a bug with the BEAM stratum back end in case of formatted job descriptions (e.g. new Nicehash platform)

### lolMiner 0.8.4
- Fixed a 0 sol/s issue for Zelcash
- Reduced Zelcash memory usage to 2.9 GBytes
- Fixed a bug with the stratum for 125/4, 144/5, 192/7 and 96/5 in case the pool sends a very low job id.

### lolMiner 0.8.3
- Added 16 GByte solver for Cuckatoo-31 (GRIN). Better performance and less energy use for Radeon VII, Radeon Pro Duo Polaris, Vega Frontier and RX 570 16GByte

### lolMiner 0.8.2
- Added support for ZelHash (EquihashR 125/4/0) for the hard fork (July 2nd)

### lolMiner 0.8.1
- Added support for NiceHash on Grin
- Fixed a bug with some pool software (e.g. grinmint.com)
- Reduced number of stale shares submitted 

### lolMiner 0.8
- Added support for Grin (Cuckatoo-31) for 4G (Slean) and 8G cards. The parameter is "--coin GRIN-AT31"
- Grin cycle finding is completely done on GPU, the miner has almost 0 CPU load (on AMD, Nvidia has some due to OpenCL back end)
- Stratum bug fixes (NiceHash) for 144/5, 192/7 and  96/5
- Fixed AUTO192_7 configuration
- Improved general stability 
- Added a distinct 1G / 3&4G / 6&8G kernel for mining MNX (Windows, Linux had this in 0.7.1)


### lolMiner 0.7
- Added support for Beam (BEAM, modified Equihash 150/5), only tuned for AMD cards
- Added TLS support for stratum. The default is off for all coins except Beam but on for Beam. This adds a new parameter --tls to control TLS on / off (see manual on usage)
- Some bugfixes and reworks in the stratum code. It is more stable now
- Complete rework of the mining back end. Lower CPU load for AMD graphic cards.
- GPU sorting changed. Its now sorted by PCIE bus address. This addresses are also shown at startup and in API.
- Integrated all kernel files to the executable
- Lowered fee of ALL algorithms / coins to 1%
- Added --help parameter (needs formating)
- API update interval is now fixed to 10 seconds
- API now smooths the performance data over approx one minute 

Coin specific changes:
- Removed workbatch parameter for MNX, its obsolete now
- Removed 96/5 Nvidia specific kernel
- Changed Safecoin (SAFE) from Equihash 144/5 to 192/7
- Added Vidulum (VDL), Equihash 192/7

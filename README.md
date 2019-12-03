# lolMiner

A git repository for lolMiner release versions

## Recent Changelog:

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

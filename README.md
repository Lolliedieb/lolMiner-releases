# lolMiner

A git repository for lolMiner release versions

## Recent Changelog:

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

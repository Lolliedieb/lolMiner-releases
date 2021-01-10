# lolMiner 1.19

## How to mine Ethash with your 4G cards.

There are two parameters that have an influence on your 4G mining speed using the so called Zombie Mode with your 4G card.

### Parameter 1: --4g-alloc-size 
This parameter can either be given one value that applied for all GPUs in your system or it can be a comma seperated list of values. Note that all cards that are equipped with more then 4G memory will ignore this parameter. A value of 0 is just the lolMiner default. When providing a list of card individual values do not skip those cards that have more then 4G of memory (just set them to 0).

The higher this parameter is choosen the more memory for a partial dag lolMiner is allowed to use. Thus the speed on your 4G cards will increase significantly. 
Given a recent driver (e.g. amdgpu-pro 20.30 or 20.40) and the right Linux kernel (best is 5.4 at the moment), you can almost always set a value of 4078, on many rigs even 4080. Older drivers or other Linux kernels may require significantly lower values and will perform slower.

Usually values that work fine, should also be good on higher epochs, so this is a one time tuning effort. 


### Parameter 2: --zombie-tune
This parameter will allow the miner to use additional system resources to speed up the zombie mode on high epochs. Generally setting a value around 2 works best for most systems that have connected their GPUs via PCIe gen2 x1 risers. The tune requires some extra communication of the host with the GPUs and so may struggle on a gen1 link connection. 

Per default lolMiner will run a auto tune to find the ideal configuration of your rig. This will totally take approximately 2 minutes per GPU in the system. At the end the miner will output the result of the automatic tuning so it can be replicated in future runs. Note that a new epoch may need to re-run the 
tuning for perfect results.

As for the --4g-alloc-size all cards that are equipped with more then 4G memory will ignore this parameter. When providing a list of card individual values do not skip those cards that have more then 4G of memory (just set them to 0). 

The tune can be deactivated by setting it to 0. Note that each card may need an individual tuning, so it is best to enable it card by card. Values that are set too high will reduce the speed measurably. Use 0 in case the rig shows uncommon instabilities with the tune enabled.

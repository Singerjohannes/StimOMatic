# Instructions to install the Psychophysics display demo for StimOMatic. #

This package has two components:  
1) Simple psychophysics toolbox script (Matlab) that displays stimuli conditional on commands received from the online analysis of StimOMatic.  
2) Python communication server that receives commands sent by StimOMatic.

## Requirements: ##
These apply to the system where psychophysics toolbox should be used to display stimuli.

- Matlab
- Runnning Psychophysics toolbox version 3 (PTB3).
- Python 2.7.3.


## Install instructions: ##

### 1) Python

The Python TCP Server is in `python/tcpServerMmap/tcpServerMmap.py`  

The default for the shared file is set as `c:/temp/varstoreNew.dat`. The default TCP Port used is 9999.  
Change these settings as required.  

Then run `tcpServerMmap.py`. It should say `Press any key to start...`.  
Press any key and it should say `Ready to receive data...`.  

This completes the Python part. Press `Ctrl-C` to close the program properly.

### 2) Matlab

In the Matlab running on the display system (psychophysics toolbox), execute `setpath_StimOMatic.m` to initialize all paths.  
Modify `experimentScenes1_testConditional.m` accordingly (paths in first few lines).

This script will display stimuli conditional on the power of ongoing oscillations. Also, it will send TTLs to the acqusition
system via the parallel port. Disable this by removing the calls to `sendTTLwithLog.m`

Also, set which screen(s) should be updated. The screens variable specifies how many screens should be updated. Set to 1 unless you have a multi-screen setup and want to use several for displaying visual stimuli.  
The default is that only one screen is updated. Drag matlab to the second screen so you can see the cmd line while the experiment runs.
Also drag the python script that receives the data to the same screen so you can observe the data arriving.

## Usage instructions: ##

1. In Stimomatic, put the IP address of the system where `tcpServerMmap.py` is running into the field `PTB Sys IP`.

2. Add the `Realtime Control LFP` plugin. Adjust settings accordingly. Whenever the threshold is crossed for the channel specified, the control plugin will send a command to the system identified in `PTY Sys IP`. 
The `tcpServerMmap.py` script will acknowledge this with `received data: X`.

3. Start `experimentScenes1_testConditional.m`
Press any key to start the experiment. Press `s` to abort.

This demo script waits for two thresholds, a low one and a high one (conditional sequence experiment as described in the paper).
For demo purposes, use the `power hilbert thresh` method in the control plugin in stimomatic.
Set `Th#1` to a high positive value and `Th#2` to a low value. A negative value in `Th#2` means you want to trigger when the signal is below this threshold. 


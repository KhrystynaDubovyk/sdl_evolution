# SDL behavior in case of LOW_VOLTAGE event

* Proposal: [SDL-0184](184-Resumption-during-Low-Voltage.md)
* Author: [Alexander Kutsan](https://github.com/LuxoftAKutsan)
* Status: **In review**
* Impacted Platforms: [Core / Web / RPC]

## Introduction

This proposal defines SDL behavior related to resumption data saving and restoring in low voltage event.  
A low voltage scenario is triggered when the battery voltage hits below a certain predefined threshold set by the system.  
In case of such an event, the EMMC is turned off and all read/write operations are unavailable.  
After the voltage level is restored all operations are resumed.

## Motivation

Implement logic that will allow SDL to save and resume aplication data after battery charge is restored within the same ignition cycle or in the next ignition cycle if SDL was shut down during a LOW_VOLTAGE event.  
When battery voltage hits below the predefined threshold set by the system (e.g.7v) SDL will "freeze" all operations until it will be switched off or resumed.

## Proposed solution

Since SDL operations are to be halted including receiving and processing of normal RPC messages from HMI, it is proposed to use last correct saved applicaton data before LOW VOLTAGE event occured. 
In case SDL receives a "LOW_VOLTAGE" signal, SDL must stop any read/write activities until SDL receives a "WAKE_UP" signal or new ignition cycle.  

During LOW_VOLTAGE state the following behavior is proposed:
* SDL persists resumption related data stored before receiving a "LOW_VOLTAGE" signal

SDL resumes its regular work after receiving a "WAKE_UP" signal:
* If "LOW_VOLTAGE" was received at the moment of writing to policies database, SDL and Policies Manager must keep policies database correct and working. After "WAKE_UP" policy database reflects the last known correct state.
* If "LOW_VOLTAGE" was received at the moment of saving applications state to file system, SDL must keep resumption storage correct and working. After "WAKE_UP" resumption storage reflects the last known correct state.

* In case SDL received "LOW_VOLTAGE" signal from HMI, then "WAKE_UP" signal does not come and SDL shuts down, 
  there will be no possibility in new ignition cycle to recognize that LOW_VOLTAGE event occured in previous ignition cycle. 
* After LOW_VOLTAGE event - SDL has no possibility to store any data for the next ignition cycle.
  Therefore in next ignition cycle SDL has possibility to resume all related app data(including HMI Level) from latest save 
  before LOW VOLTAGE event.

NOTE: SDL can not guarantee correct policy data base and resumption data saving in case LOW VOLTAGE event triggers restrictions to file system from OS side.

**Resumption during LOW VOLTAGE event:** 

*LIMITED HMI Level resumption:*
 
- Any application that in LIMITED HMILevel unexpectedly disconnects 
- during the time frame of 30 sec (inclusive) before "LOW_VOLTAGE" signal from HMI
- and then registers during 30 sec. after "WAKE_UP" signal in the same ignition cycle
- and there is no other application currently in LIMITED,
  must be resumed to LIMITED HMILevel by SDL as well as related app data should be resumed
  
*FULL HMI Level resumption:*
 
- Any application that in FULL HMILevel unexpectedly disconnects 
- during the time frame of 30 sec (inclusive) before "LOW_VOLTAGE" signal from HMI
- and then registers during 30 sec. after "WAKE_UP" signal in the same ignition cycle
- and there is no other application currently in FULL,
  must be resumed to FULL HMILevel by SDL as well as related app data should be resumed
  
If application in any HMI Level unexpectedly disconnects 
- out of time frame of 30sec before "LOW_VOLTAGE" signal from HMI
- and then registers after "WAKE_UP" signal during 30secs in the same ignition cycle
  only related app data should be resumed without HMI Level
  
If application in any HMI Level unexpectedly disconnects 
- during time frame of 30sec before "LOW_VOLTAGE" signal from HMI
- and then registers after 30 secs passed after "WAKE_UP" signal in the same ignition cycle
  only related app data should be resumed without HMI Level
 
*In case SDL received "LOW_VOLTAGE" signal from HMI, then "WAKE_UP" signal does not come and SDL shuts down:*
  
  If app registers during first 30 secs after IGNITION ON: 
- SDL resumes FULL or LIMITED (depends on app HMI Level which was active during last save of 
  resumption state to file system before LowVoltage - up to 10 seconds ( configurable by ini file).
  only if currently there is no other application in the same HMI Levels as required to resume   
- If app registers after 30 secs passed after IGNITION_OFF - SDL resumes app data without HMI Level  


## Details of implementation  


## Potential downsides  


## Alternatives considered  

 



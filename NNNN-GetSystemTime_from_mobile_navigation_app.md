
# Get System Time from mobile GPS source

* Proposal: [SDL-NNNN](NNNN-GetSystemTime_from_mobile_navigation_app.md)
* Author: [Andriy Byzhynar](https://github.com/AByzhynar)
* Status: **In review**
* Impacted Platforms: [Core]

## Introduction

Get real system time from GPS source located in mobile device if Head unit GPS does not work by some reason


## Motivation

This will allow to start secured Navigation service even head unit GPS is down by some reason. 


Resolved Problems with the approach :

    SDL requires real system time (based on current time zone which is provided by GPS) 
    for security certificate validation in order to start secured service
    In current implementation SDL can start non-secured services ONLY if head unit GPS 
    does not work/unavailable by some reason

## Proposed solution

Proposed to extend app service `NAVIGATION` with GetSystemTime RPC

 
## Potential downsides

 - SDL must trust mobile information received from mobile device. 
   This may lead to situation when security certificate received from mobile for establishing secure connection is actually expired(or not yet valid) 
   but SDL consider it as valid due to wrong/false time received from mobile source.

## Impact on existing code

 Will require handling of additional GetSystemTime request/response
 
## Alternatives considered

Assuming that head unit has its own independent access to internet, 
SDL can use NTP [Network Time Protocol](https://en.wikipedia.org/wiki/Network_Time_Protocol) for getting system time and processing secure connection.

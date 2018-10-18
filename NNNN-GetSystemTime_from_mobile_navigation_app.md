
# Send app permissions within registration

* Proposal: [SDL-NNNN](NNNN-GetSystemTime_from_mobile_navigation_app.md)
* Author: [Andriy Byzhynar](https://github.com/AByzhynar)
* Status: **In review**
* Impacted Platforms: [Core]

## Introduction

Get real system time from GPS source located in mobile device if Head unit GPS does not work by some reason


## Motivation

This will allow to start secured Navigation service even head unit GPS is down by some reason. 


Resolved Problems with the approach :

    SDL requires real system time (based on current time zone which is provided by GPS) for security certificate validation in       order to start secured service
    In current implementation SDL can start non-secured services ONLY if head unit GPS does not work/unavailable by some reason

## Proposed solution

Proposed to extend app service `NAVIGATION` with GetSystemTime RPC

 
## Potential downsides

 - N/A

## Impact on existing code

 Will require handling of additional GetSystemTime request/response


## Alternatives considered
 

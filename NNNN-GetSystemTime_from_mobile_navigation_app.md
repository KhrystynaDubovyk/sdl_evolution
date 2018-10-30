
# Get System Time from mobile GPS source

* Proposal: [SDL-NNNN](NNNN-GetSystemTime_from_mobile_navigation_app.md)
* Author: [Andriy Byzhynar](https://github.com/AByzhynar)
* Status: **In review**
* Impacted Platforms: [Core]

## Introduction

Get real system time from GPS source located in mobile device (`NAVIGATION` app service) if head unit can not provide system time by some reason


## Motivation

This will allow to start secured Navigation streaming even head unit system time is down by some reason at this time. 


Resolved Problems with the approach :

    SDL requires real system time (based on current time zone which is provided by head unit system) 
    for security certificate validation in order to start secured service
    In current implementation SDL can start non-secured services ONLY if head unit can not provide system time by some reason

## Proposed solution

Proposed to extend app service `NAVIGATION` with GetSystemTime RPC

## Additions to MOBILE_API

GetSystemTime request\response

```xml
<function name="GetSystemTime" messagetype="request">
    <description>Request from SDL to Navigation Service to obtain current UTC time.</description>
</function>
<function name="GetSystemTime" messagetype="response">
    <param name="systemTime" type="Common.DateTime" mandatory="true">
      <description>Current UTC system time</description>
    </param>
</function>
```
 
## Potential downsides

 - SDL must trust information received from mobile device. 
   This may lead to situation when security certificate received from mobile for establishing secure connection is actually expired (or not yet valid) 
   but SDL consider it as valid due to wrong/false time received from mobile source.
   Therefore during `NAVIGATION` service registration - it is required to get system time from head unit to start service securely.
   So `GetSystemTime` should be sent to mobile only encrypted.

## Impact on existing code

 Will require handling of additional GetSystemTime request/response
 
## Alternatives considered

Assuming that head unit has its own independent access to internet, 
SDL can use [Network Time Protocol](https://en.wikipedia.org/wiki/Network_Time_Protocol) (NTP) for getting system time and processing secure connection.

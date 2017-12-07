# HMI result codes usage at at the individual RPC level

* Proposal: [SDL-NNNN](NNNN-filename.md)
* Author: 
* Status: **Awaiting review**
* Impacted Platforms: [RPC]

## Introduction

This proposal is to reach overall awareness and undertandig of the process of HMI result codes creation, storing and updates.

## Motivation

Currently resultCodes defined in HMI API.xml via the `Result` enum are not used at the individual RPC level.

## Proposed solution
To list expected HMI result codes on API XML same as for mobile API.  
To list expected HMI result codes in hmi_integration_guidelines for each HMI response.

## Potential downsides
None

## Impact on existing code
### HMI API responses processed by SDL
1. Interface name="BasicCommunication" 
```
<function name="GetCapabilities" messagetype="response">
      <param name="capabilities" type="Common.ButtonCapabilities" array="true" minsize="1" maxsize="100" mandatory="true">
        <description>Response must provide the names of available buttons and their capabilities.See ButtonCapabilities</description>
      </param>
       <param name="presetBankCapabilities" type="Common.PresetBankCapabilities" mandatory="false">
        <description>Must be returned if the platform supports custom on-screen Presets</description>
      </param>
    </function>
```

```
<function name="ButtonPress" messagetype="response">
    </function>
```

```
 <function name="UpdateAppList" messagetype="response">
    </function>
```
```
<function name="UpdateDeviceList" messagetype="response">
    </function>
```

```
<function name="AllowDeviceToConnect" messagetype="response">
      <param name="allow" type="Boolean" mandatory="true"/>
    </function>
```

```
 <function name="ActivateApp" messagetype="response">
    </function>
```

```
 <function name="MixingAudioSupported" messagetype="response">
      <description>If no response received SDL supposes that mixing audio is not supported</description>
      <param name="attenuatedSupported" type="Boolean" mandatory="true">
        <description>Must be true if supported</description>
      </param>
    </function>
```

```
<function name="DialNumber" messagetype="response">
    </function>
```

```
 <function name="SystemRequest" messagetype="response">
    </function>
 ```
 
 ```
  <function name="PolicyUpdate" messagetype="response">
    </function>
```

```
 <function name="GetSystemInfo" messagetype="response">
    <param name="ccpu_version" type="String" maxlength="500" mandatory="true">
      <description>Software version of the module</description>
    </param>
    <param name="language" type="Common.Language" mandatory="true">
      <description>ISO 639-1 combined with ISO 3166 alpha-2 country code (i.e. en-us)</description>
    </param>
    <param name="wersCountryCode" type="String" maxlength="500" mandatory="true">
      <description>Country code from the Ford system WERS (i.e.WAEGB).</description>
    </param>
  </function>
  ```
  
  ```
   <function name="DecryptCertificate" messagetype="response">
         <description>SUCCESS - in case the certificate is decrypted and placed to the same file from request.</description>
   </function>
   ```
2. Interface name="SDL" 
```
  <function name="ActivateApp" messagetype="response">
    <param name="isSDLAllowed" type="Boolean" mandatory="true" scope="internal"/>
    <param name="device" type="Common.DeviceInfo" mandatory="false" scope="internal">
      <description>If isSDLAllowed is false, consent for sending PT through specified device is required.</description>
    </param>
    <param name="isPermissionsConsentNeeded" type="Boolean" mandatory="true"/>
    <param name="isAppPermissionsRevoked" type="Boolean" mandatory="true"/>
    <param name="appRevokedPermissions" type="Common.PermissionItem" array="true" minsize="1" maxsize="100" mandatory="false">
        <description>If app permissions were reduced (isAppPermissionsRevoked == true), then this array specifies list of removed permissions. </description>
      </param>
    <param name="isAppRevoked" type="Boolean" mandatory="true"/>
    <param name="priority" type="Common.AppPriority" mandatory="false">
        <description>Send to HMI so that it can coordinate order of requests/notifications correspondingly.</description>
      </param>
  </function>
```

```
  <function name="GetUserFriendlyMessage" messagetype="response">
      <param name="messages" type="Common.UserFriendlyMessage" array="true" minsize="1" maxsize="100" mandatory="false">
        <description>If no message was found in PT for specified message code and for HMI current or specified language, this parameter will be omitted.</description>
      </param>
    </function>
```

```
 <function name="GetListOfPermissions" messagetype="response">
      <param name="allowedFunctions" type="Common.PermissionItem" mandatory="true" array="true" minsize="0" maxsize="100">
        <description>If no permissions were specified for application the array will come empty.</description>
      </param>
      <param name="externalConsentStatus" type="Common.ExternalConsentStatus" mandatory="true" array="true" minsize="0" maxsize="100">
        <description>External User Consent Settings (defined by entityType and entityID) status: enabled/disabled. If empty array is returned, SDL does not have any stored status.</description>
      </param>
    </function>
```

```
 <function name="UpdateSDL" messagetype="response">
      <description>Specify result: no update needed, update was successful/unsuccessful etc</description>
      <param name="result" type="Common.UpdateResult" mandatory="true"/>
    </function>
```

```
  <function name="GetStatusUpdate" messagetype="response">
      <param name="status" type="Common.UpdateResult" mandatory="true"/>
    </function>
```

```
 <function name="GetURLS" messagetype="response" scope="internal">
      <param name="urls" type="Common.ServiceInfo" array="true" mandatory="false" minsize="1" maxsize="100"/>
    </function>
```

### HMI API responses transferred by SDL from HMI to mobile application
1. Interface name="Buttons" 
```
    <function name="GetCapabilities" messagetype="response">
      <param name="capabilities" type="Common.ButtonCapabilities" array="true" minsize="1" maxsize="100" mandatory="true">
        <description>Response must provide the names of available buttons and their capabilities.See ButtonCapabilities</description>
      </param>
      <param name="presetBankCapabilities" type="Common.PresetBankCapabilities" mandatory="false">
        <description>Must be returned if the platform supports custom on-screen Presets</description>
      </param>
    </function>
```

```
 <function name="ButtonPress" messagetype="response">
    </function>
```
2. Interface name="VR"
```
<function name="IsReady" messagetype="response">
    <param name="available" type="Boolean" mandatory="true">
      <description>Must be true if VR is present and ready to communicate with SDL.</description>
    </param>
  </function>
```

```
<function name="AddCommand" messagetype="response">
  </function>
```

```
<function name="DeleteCommand" messagetype="response">
  </function>
```

```
<function name="PerformInteraction" messagetype="response">
    <param name="choiceID" type="Integer" minvalue="0" maxvalue="2000000000" mandatory="false">
      <description>
        ID of the choice that was selected in response to PerformInteraction.
      </description>
    </param>
  </function>
  ```
  
```
 <function name="ChangeRegistration" messagetype="response">
  </function>
```

```
<function name="GetSupportedLanguages" messagetype="response">
    <param name="languages" type="Common.Language" mandatory="true" array="true" minsize="1" maxsize="100">
      <description>List of languages supported in VR.</description>
    </param>
  </function>
```

```
  <function name="GetLanguage" messagetype="response">
    <param name="language" type="Common.Language" mandatory="true"/>
  </function>
```

```
<function name="GetCapabilities" messagetype="response">
    <param name="vrCapabilities" type="Common.VrCapabilities" minsize="1" maxsize="100" array="true" mandatory="false">
      <description>Types of input recognized by VR module.</description>
    </param>
  </function>
```
 
## Alternatives considered
N/A

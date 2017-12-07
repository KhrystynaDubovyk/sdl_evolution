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
`param name="code" type="Integer" mandatory="true"` should be added to each `messagetype="response"`

### HMI API responses processed by SDL
**1. Interface name="BasicCommunication"** 
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
**2. Interface name="SDL"**
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

### HMI API responses transferred by SDL to mobile application
**1. Interface name="Buttons"** 
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
**2. Interface name="VR"**
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

**3. Interface name="TTS"**  
```
<function name="GetCapabilities" messagetype="response">
    <param name="speechCapabilities" type="Common.SpeechCapabilities" minsize="1" maxsize="5" array="true" mandatory="true">
      <description>See SpeechCapabilities</description>
    </param>
    <param name="prerecordedSpeechCapabilities" type="Common.PrerecordedSpeech" minsize="1" maxsize="5" array="true" mandatory="true">
      <description>See PrerecordedSpeech</description>
    </param>
  </function>
```

```
<function name="IsReady" messagetype="response">
    <param name="available" type="Boolean" mandatory="true">
      <description>Must be true if TTS is present and ready to communicate with SDL.</description>
    </param>
  </function>
```

```
<function name="Speak" messagetype="response">
    <description>Provides information about success of operation.</description>
  </function>
```

```
<function name="StopSpeaking" messagetype="response">
  </function>
```

```
 <function name="ChangeRegistration" messagetype="response">
  </function>
```

```
<function name="GetSupportedLanguages" messagetype="response">
    <param name="languages" type="Common.Language" mandatory="true" array="true" minsize="1" maxsize="100">
      <description>List of languages supported in TTS.</description>
    </param>
  </function>
```

```
 <function name="GetLanguage" messagetype="response">
    <param name="language" type="Common.Language" mandatory="true"/>
  </function>
```

```
<function name="SetGlobalProperties" messagetype="response">
  </function>
```

**4. Interface name="UI"**
```
<function name="Alert" messagetype="response">
    <param name="tryAgainTime" type="Integer" mandatory="false" minvalue="0" maxvalue="2000000000">
      <description>Amount of time (in milliseconds) that SDL must wait before resending an alert. Must be provided if another system event or overlay currently has a higher priority than this alert.</description>
    </param>
  </function>
```

```
<function name="Show" messagetype="response">
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
<function name="AddSubMenu" messagetype="response">
  </function>
```

```
 <function name="DeleteSubMenu" messagetype="response">
  </function>
```

```
<function name="PerformInteraction" messagetype="response">
    <param name="choiceID" type="Integer" minvalue="0" maxvalue="2000000000" mandatory="false">
      <description>ID of the choice that was selected in response to PerformInteraction.</description>
    </param>
    <param name="manualTextEntry" type="String" minlength="0" maxlength="500" mandatory="false">
      <description>
            Manually entered text selection, e.g. through keyboard
            Can be returned in lieu of choiceID, depending on trigger source
      </description>
    </param>
  </function>
```

```
<function name="SetMediaClockTimer" messagetype="response">
  </function>
```

```
<function name="SetGlobalProperties" messagetype="response">
  </function>
```

```
<function name="GetCapabilities" messagetype="response">
    <param name="displayCapabilities" type="Common.DisplayCapabilities" mandatory="true">
      <description>Information about the capabilities of the display: its type, text field supported, etc. See DisplayCapabilities. </description>
    </param>
    <param name="audioPassThruCapabilities" type="Common.AudioPassThruCapabilities" mandatory="true"/>
    <param name="hmiZoneCapabilities" type="Common.HmiZoneCapabilities" mandatory="true"/>
    <param name="softButtonCapabilities" type="Common.SoftButtonCapabilities" minsize="1" maxsize="100" array="true" mandatory="false">
      <description>Must be returned if the platform supports on-screen SoftButtons.</description>
    </param>
    <param name="hmiCapabilities" type="Common.HMICapabilities" mandatory="false">
      <description>Specifies the HMI’s capabilities. See HMICapabilities.</description>
    </param>
    <param name="systemCapabilities" type="Common.SystemCapabilities" mandatory="false">
      <description>Specifies system capabilities. See SystemCapabilities</description>
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
      <description>List of languages supported in UI.</description>
    </param>
  </function>
```

```
 <function name="GetLanguage" messagetype="response">
    <param name="language" type="Common.Language" mandatory="true"/>
  </function>
```

```
<function name="SetAppIcon" messagetype="response">
  </function>
```

```
<function name="SetDisplayLayout" messagetype="response">
    <param name="displayCapabilities" type="Common.DisplayCapabilities" mandatory="false">
      <description>See DisplayCapabilities</description>
    </param>
    <param name="buttonCapabilities" type="Common.ButtonCapabilities" minsize="1" maxsize="100" array="true" mandatory="false">
      <description>See ButtonCapabilities</description >
    </param>
    <param name="softButtonCapabilities" type="Common.SoftButtonCapabilities" minsize="1" maxsize="100" array="true" mandatory="false">
      <description>If returned, the platform supports on-screen SoftButtons; see SoftButtonCapabilities.</description >
    </param>
    <param name="presetBankCapabilities" type="Common.PresetBankCapabilities" mandatory="false">
      <description>If returned, the platform supports custom on-screen Presets; see PresetBankCapabilities.</description >
    </param>
  </function>
```

```
 <function name="ShowCustomForm" messagetype="response">
    <param name="info" type="String" maxlength="1000" mandatory="false" platform="documentation">
      <description>Provides additional human readable info regarding the result.</description>
    </param>
  </function>
```

```
<function name="Slider" messagetype="response">
    <param name="sliderPosition" type="Integer" minvalue="1" maxvalue="26" mandatory="false">
      <description>Current slider position. Must be returned when the user has clicked the ‘Save’ or ‘Canceled’ button or by the timeout </description>
    </param>
  </function>
```

```
<function name="ScrollableMessage" messagetype="response">
  </function>
```

```
<function name="PerformAudioPassThru" messagetype="response">
  </function>
```

```
<function name="EndAudioPassThru" messagetype="response">
  </function>
```

```
<function name="IsReady" messagetype="response">
    <param name="available" type="Boolean" mandatory="true">
      <description>Must be true if UI is present and ready to communicate with SDL.</description>
    </param>
  </function>
```

```
 <function name="ClosePopUp" messagetype="response">
    <description>Provides the result of operation.</description>
  </function>
```
```
<function name="SendHapticData" messagetype="response">
  </function>
```

**5. Interface name="Navigation"**
```
 <function name="IsReady" messagetype="response">
    <param name="available" type="Boolean" mandatory="true">
      <description>Must be true if Navigation is present and ready to communicate with SDL.</description>
    </param>
  </function>
```

```
<function name="SendLocation" messagetype="response" >
  </function>
```

```
<function name="ShowConstantTBT" messagetype="response">
  </function>
```

```
<function name="AlertManeuver" messagetype="response">
  </function>
```

```
  <function name="UpdateTurnList" messagetype="response">
  </function>
```

```
 <function name="SetVideoConfig" messagetype="response">
    <description>
      Response from HMI to SDL whether the configuration is accepted.
      In a negative response, a list of rejected parameters are supplied.
    </description>
    <param name="rejectedParams" type="String" array="true" minsize="1" maxsize="1000" mandatory="false">
      <description>
        List of params of VideoConfig struct which are not accepted by HMI, e.g. "protocol" and "codec".
        This param exists only when the response is negative.
      </description>
    </param>
  </function>
```

```
  <function name="StartStream" messagetype="response">
  </function>
```

```
  <function name="StopStream" messagetype="response">
  </function>
```

```
  <function name="StartAudioStream" messagetype="response">
  </function>
```

```
  <function name="StopAudioStream" messagetype="response">
  </function>
```

```
 <function name="GetWayPoints" functionID="GetWayPointsID" messagetype="response">
    <param name="appID" type="Integer" mandatory="true">
      <description>ID of the application.</description>
    </param>
    <param name="wayPoints" type="Common.LocationDetails" mandatory="false" array="true" minsize="1" maxsize="10">
      <description>See LocationDetails</description>
    </param>
  </function>
```

```
  <function name="SubscribeWayPoints" functionID="SubscribeWayPointsID" messagetype="response">
  </function>
```

```
  <function name="UnsubscribeWayPoints" functionID="UnsubscribeWayPointsID" messagetype="response">
  </function>
```

**6. Interface name="VehicleInfo"**
```
 <function name="IsReady" messagetype="response">
    <param name="available" type="Boolean" mandatory="true">
      <description>Must be true if vehicle data modules are present and ready to communicate with SDL.</description>
    </param>
  </function>
```

```
  <function name="GetVehicleType" messagetype="response">
    <param name="vehicleType" type="Common.VehicleType" mandatory="true"/>
  </function>
```

```
  <function name="ReadDID" messagetype="response">
    <param name="didResult" type="Common.DIDResult" minsize="0" maxsize="1000" array="true" mandatory="false">
      <description>Array of requested DID results (with data if available).</description>
    </param>
  </function>
```

```
 <function name="GetDTCs" messagetype="response">
    <param name="ecuHeader" type="Integer" minvalue="0" maxvalue="65535" mandatory="true">
      <description>2 byte ECU Header for DTC response (as defined in VHR_Layout_Specification_DTCs.pdf)</description>
    </param>
    <param name="dtc" type="String" mandatory="false" minsize="1" maxsize="15" maxlength="10" array="true">
      <description>
        Array of all reported DTCs on module. Each DTC is represented with 4 bytes:
        3 bytes for data
        1 byte for status
      </description>
    </param>
  </function>
```

```
<function name="DiagnosticMessage" messagetype="response">
    <param name="messageDataResult" type="Integer" minvalue="0" maxvalue="255" minsize="1" maxsize="65535" array="true" mandatory="true">
      <description>
        Array of bytes comprising CAN message result.
      </description>
    </param>
  </function>
```

```
 <function name="SubscribeVehicleData" messagetype="response">
    <param name="gps" type="Common.VehicleDataResult" mandatory="false">
      <description>See GPSData</description>
    </param>
    <param name="speed" type="Common.VehicleDataResult" mandatory="false">
      <description>The vehicle speed in kilometers per hour</description>
    </param>
    <param name="rpm" type="Common.VehicleDataResult" mandatory="false">
      <description>The number of revolutions per minute of the engine</description>
    </param>
    <param name="fuelLevel" type="Common.VehicleDataResult" mandatory="false">
      <description>The fuel level in the tank (percentage)</description>
    </param>
    <param name="fuelLevel_State" type="Common.VehicleDataResult" mandatory="false">
      <description>The fuel level state</description>
    </param>
    <param name="instantFuelConsumption" type="Common.VehicleDataResult" mandatory="false">
      <description>The instantaneous fuel consumption in microlitres</description>
    </param>
    <param name="externalTemperature" type="Common.VehicleDataResult" mandatory="false">
      <description>The external temperature in degrees celsius.</description>
    </param>
    <param name="prndl" type="Common.VehicleDataResult" mandatory="false">
      <description>See PRNDL</description>
    </param>
    <param name="tirePressure" type="Common.VehicleDataResult" mandatory="false">
      <description>See TireStatus</description>
    </param>
    <param name="odometer" type="Common.VehicleDataResult" mandatory="false">
      <description>Odometer in km</description>
    </param>
    <param name="beltStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the seat belts</description>
    </param>
    <param name="bodyInformation" type="Common.VehicleDataResult" mandatory="false">
      <description>The body information including power modes</description>
    </param>
    <param name="deviceStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The device status including signal and battery strength</description>
    </param>
    <param name="driverBraking" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the brake pedal</description>
    </param>
    <param name="wiperStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the wipers</description>
    </param>
    <param name="headLampStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>Status of the head lamps</description>
    </param>
    <param name="engineTorque" type="Common.VehicleDataResult" mandatory="false">
      <description>Torque value for engine (in Nm) on non-diesel variants</description>
    </param>
    <param name="accPedalPosition" type="Common.VehicleDataResult" mandatory="false">
      <description>Accelerator pedal position (percentage depressed)</description>
    </param>
    <param name="steeringWheelAngle" type="Common.VehicleDataResult" mandatory="false">
      <description>Current angle of the steering wheel (in deg)</description>
    </param>
        <!-- Ford Specific Data Items -->
    <param name="eCallInfo" type="Common.VehicleDataResult" mandatory="false">
      <description>Emergency Call notification and confirmation data</description>
    </param>
    <param name="airbagStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the air bags</description>
    </param>
    <param name="emergencyEvent" type="Common.VehicleDataResult" mandatory="false">
      <description>Information related to an emergency event (and if it occurred)</description>
    </param>
    <param name="clusterModes" type="Common.VehicleDataResult" mandatory="false">
      <description>The status modes of the cluster</description>
    </param>
    <param name="myKey" type="Common.VehicleDataResult" mandatory="false">
      <description>Information related to the MyKey feature</description>
    </param>
        <!-- / Ford Specific Data Items -->
  </function>
 ```
 
 ```
 <function name="UnsubscribeVehicleData" messagetype="response">
    <param name="gps" type="Common.VehicleDataResult" mandatory="false">
      <description>See GPSData</description>
    </param>
    <param name="speed" type="Common.VehicleDataResult" mandatory="false">
      <description>The vehicle speed in kilometers per hour</description>
    </param>
    <param name="rpm" type="Common.VehicleDataResult" mandatory="false">
      <description>The number of revolutions per minute of the engine</description>
    </param>
    <param name="fuelLevel" type="Common.VehicleDataResult" mandatory="false">
      <description>The fuel level in the tank (percentage)</description>
    </param>
    <param name="fuelLevel_State" type="Common.VehicleDataResult" mandatory="false">
      <description>The fuel level state</description>
    </param>
    <param name="instantFuelConsumption" type="Common.VehicleDataResult" mandatory="false">
      <description>The instantaneous fuel consumption in microlitres</description>
    </param>
    <param name="externalTemperature" type="Common.VehicleDataResult" mandatory="false">
      <description>The external temperature in degrees celsius</description>
    </param>
    <param name="prndl" type="Common.VehicleDataResult" mandatory="false">
      <description>See PRNDL</description>
    </param>
    <param name="tirePressure" type="Common.VehicleDataResult" mandatory="false">
      <description>See TireStatus</description>
    </param>
    <param name="odometer" type="Common.VehicleDataResult" mandatory="false">
      <description>Odometer in km</description>
    </param>
    <param name="beltStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the seat belts</description>
    </param>
    <param name="bodyInformation" type="Common.VehicleDataResult" mandatory="false">
      <description>The body information including power modes</description>
    </param>
    <param name="deviceStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The device status including signal and battery strength</description>
    </param>
    <param name="driverBraking" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the brake pedal</description>
    </param>
    <param name="wiperStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the wipers</description>
    </param>
    <param name="headLampStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>Status of the head lamps</description>
    </param>
    <param name="engineTorque" type="Common.VehicleDataResult" mandatory="false">
      <description>Torque value for engine (in Nm) on non-diesel variants</description>
    </param>
    <param name="accPedalPosition" type="Common.VehicleDataResult" mandatory="false">
      <description>Accelerator pedal position (percentage depressed)</description>
    </param>
    <param name="steeringWheelAngle" type="Common.VehicleDataResult" mandatory="false">
      <description>Current angle of the steering wheel (in deg)</description>
    </param>
        <!-- Ford Specific Data Items -->
    <param name="eCallInfo" type="Common.VehicleDataResult" mandatory="false">
      <description>Emergency Call notification and confirmation data</description>
    </param>
    <param name="airbagStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the air bags</description>
    </param>
    <param name="emergencyEvent" type="Common.VehicleDataResult" mandatory="false">
      <description>Information related to an emergency event (and if it occurred)</description>
    </param>
    <param name="clusterModes" type="Common.VehicleDataResult" mandatory="false">
      <description>The status modes of the cluster</description>
    </param>
    <param name="myKey" type="Common.VehicleDataResult" mandatory="false">
      <description>Information related to the MyKey feature</description>
    </param>
        <!-- / Ford Specific Data Items -->
  </function>
```

```
 <function name="GetVehicleData" messagetype="response">
    <param name="gps" type="Common.GPSData" mandatory="false">
      <description>See GPSData</description>
    </param>
    <param name="speed" type="Float" minvalue="0" maxvalue="700" mandatory="false">
      <description>The vehicle speed in kilometers per hour</description>
    </param>
    <param name="rpm" type="Integer" minvalue="0" maxvalue="20000" mandatory="false">
      <description>The number of revolutions per minute of the engine</description>
    </param>
    <param name="fuelLevel" type="Float" minvalue="-6" maxvalue="106" mandatory="false">
      <description>The fuel level in the tank (percentage)</description>
    </param>
    <param name="fuelLevel_State" type="Common.ComponentVolumeStatus" mandatory="false">
      <description>The fuel level state</description>
    </param>
    <param name="instantFuelConsumption" type="Float" minvalue="0" maxvalue="25575" mandatory="false">
      <description>The instantaneous fuel consumption in microlitres</description>
    </param>
    <param name="externalTemperature" type="Float" minvalue="-40" maxvalue="100" mandatory="false">
      <description>The external temperature in degrees celsius</description>
    </param>
    <param name="vin" type="String" maxlength="17" mandatory="false">
      <description>Vehicle identification number</description>
    </param>
    <param name="prndl" type="Common.PRNDL" mandatory="false">
      <description>See PRNDL</description>
    </param>
    <param name="tirePressure" type="Common.TireStatus" mandatory="false">
      <description>See TireStatus</description>
    </param>
    <param name="odometer" type="Integer" minvalue="0" maxvalue="17000000" mandatory="false">
      <description>Odometer in km</description>
    </param>
    <param name="beltStatus" type="Common.BeltStatus" mandatory="false">
      <description>The status of the seat belts</description>
    </param>
    <param name="bodyInformation" type="Common.BodyInformation" mandatory="false">
      <description>The body information including power modes</description>
    </param>
    <param name="deviceStatus" type="Common.DeviceStatus" mandatory="false">
      <description>The device status including signal and battery strength</description>
    </param>
    <param name="driverBraking" type="Common.VehicleDataEventStatus" mandatory="false">
      <description>The status of the brake pedal</description>
    </param>
    <param name="wiperStatus" type="Common.WiperStatus" mandatory="false">
      <description>The status of the wipers</description>
    </param>
    <param name="headLampStatus" type="Common.HeadLampStatus" mandatory="false">
      <description>Status of the head lamps</description>
    </param>
    <param name="engineTorque" type="Float" minvalue="-1000" maxvalue="2000" mandatory="false">
      <description>Torque value for engine (in Nm) on non-diesel variants</description>
    </param>
    <param name="accPedalPosition" type="Float" minvalue="0" maxvalue="100" mandatory="false">
      <description>Accelerator pedal position (percentage depressed)</description>
    </param>
    <param name="steeringWheelAngle" type="Float" minvalue="-2000" maxvalue="2000" mandatory="false">
      <description>Current angle of the steering wheel (in deg)</description>
    </param>
    <param name="eCallInfo" type="Common.ECallInfo" mandatory="false">
      <description>Emergency Call notification and confirmation data</description>
    </param>
    <param name="airbagStatus" type="Common.AirbagStatus" mandatory="false">
      <description>The status of the air bags</description>
    </param>
    <param name="emergencyEvent" type="Common.EmergencyEvent" mandatory="false">
      <description>Information related to an emergency event (and if it occurred)</description>
    </param>
    <param name="clusterModeStatus" type="Common.ClusterModeStatus" mandatory="false">
      <description>The status modes of the cluster</description>
    </param>
    <param name="myKey" type="Common.MyKey" mandatory="false">
      <description>Information related to the MyKey feature</description>
    </param>
  </function>
```

**7. Interface name="RC"**
```
 <function name="IsReady" messagetype="response">
    <param name="available" type="Boolean" mandatory="true">
      <description>Must be true if vehicle RC modules are present and ready to communicate with SDL.</description>
    </param>
  </function>
```

```
 <function name="GetCapabilities" messagetype="response">
    <param name="remoteControlCapability" type="Common.RemoteControlCapabilities" mandatory="false">
      <description>See RemoteControlCapabilities, all available RC modules and buttons shall be returned.</description>
    </param>
  </function>
```

```
  <function name="SetInteriorVehicleData" messagetype="response">
    <description>Used to set the values of one zone and one data type within that zone</description>
    <param name="moduleData" type="Common.ModuleData" mandatory="true" >
    </param>
  </function>
```

```
<function name="GetInteriorVehicleData" messagetype="response">
  <param name="moduleData" type="Common.ModuleData" mandatory="true" >
  </param>
  <param name="isSubscribed" type="Boolean" mandatory="false" >
    <description>Is a conditional-mandatory parameter: must be returned in case "subscribe" parameter was present in the related request.
    if "true" - the "moduleType" from request is successfully subscribed and  the head unit will send onInteriorVehicleData notifications for the moduleDescription.
    if "false" - the "moduleType" from request is either unsubscribed or failed to subscribe.</description>
  </param>
</function>
```

```
 <function name="GetInteriorVehicleDataConsent" messagetype="response">
    <param name="allowed" type="Boolean" mandatory="true">
      <description>"true" - if the driver grants the permission for controlling to the named app;
      "false" - in case the driver denies the permission for controlling to the named app.</description>
    </param>
</function>
```

 
## Alternatives considered
N/A

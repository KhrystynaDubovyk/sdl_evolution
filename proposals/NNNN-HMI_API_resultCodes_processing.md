# HMI result codes usage at the individual RPC level

* Proposal: [SDL-NNNN](NNNN-filename.md)
* Author: [Khrystyna Dubovyk](https://github.com/KhrystynaDubovyk)
* Status: **Awaiting review**
* Impacted Platforms: [RPC]

## Introduction

This proposal is to reach overall awareness and understanding of the process of HMI result codes creation, storing and updates.

## Motivation

Currently resultCodes defined in HMI API.xml via the `Result` enum are not used at the individual RPC level.  
In order to mitigate possible misundertandings and discrepancy HMI result codes should be listed out clearly.

## Proposed solution
To list expected HMI result codes on API XML same as for mobile API.  
To list expected HMI result codes in hmi_integration_guidelines for each HMI response.

## Potential downsides
None

## Impact on existing code
`param name="code" type="Result" mandatory="true"` should be added to each `messagetype="response"`  

```  
</function>
    <function name="UpdateAppList" messagetype="response">
   <param name="resultCode" type="Result" platform="documentation" mandatory="true">
            <description>See Result</description>
            <element name="SUCCESS"/>
            <element name="INVALID_DATA"/>
            ...
    </param> 
</function>
```

### Impacted RPCs  
#### **HMI API responses processed by SDL**  
**Interface name="BasicCommunication"**
1. [UpdateAppList](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2771)
2. [UpdateDeviceList](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2779)
3. [AllowDeviceToConnect](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2797)
4. [ActivateApp](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2824)
5. [MixingAudioSupported](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2903)  
6. [DialNumber](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2918)  
7. [SystemRequest](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2964)  
8. [PolicyUpdate](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2988)  
9. [GetSystemInfo](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3041)  
10. [DecryptCertificate](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3065)  


**Interface name="SDL"**  
11. [ActivateApp](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4730)  
12. [GetUserFriendlyMessage](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4755)  
13. [GetListOfPermissions](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4797)  
14. [UpdateSDL](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4851)  
15. [GetStatusUpdate](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4859)  
16. [GetURLS](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4880)  

#### **HMI API responses transferred by SDL to mobile application**  
**Interface name="Buttons"**  
17. [GetCapabilities](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2669)  
18. [ButtonPress](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L2691)  

**Interface name="VR"**  
19. [IsReady](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3084)  
20. [AddCommand](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3115)  
21. [DeleteCommand](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3132)  
22. [PerformInteraction](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3165)  
23. [ChangeRegistration](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3197)  
24. [GetSupportedLanguages](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3208)  
25. [GetLanguage](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3216)  
26. [GetCapabilities](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3222)  

**Interface name="TTS"**  
27. [GetCapabilities](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3234)  
28. [IsReady](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3251)  
29. [Speak](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3271)  
30. [StopSpeaking](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3277)  
31. [ChangeRegistration](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3298)  
32. [GetSupportedLanguages](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3309)  
33. [GetLanguage](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3317)  
34. [SetGlobalProperties](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3339)  

**Interface name="UI"**  
35. [Alert](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3377)  
36. [Show](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3419)  
37. [AddCommand](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3437)  
38. [DeleteCommand](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3448)  
39. [AddSubMenu](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3462)  
40. [DeleteSubMenu](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3473)  
41. [PerformInteraction](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3500)  
42. [SetMediaClockTimer](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3532)  
43. [SetGlobalProperties](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3556)  
44. [GetCapabilities](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3582)  
45. [ChangeRegistration](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3624)  
46. [GetSupportedLanguages](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3635)  
47. [GetLanguage](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3643)  
48. [SetAppIcon](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3661)  
49. [SetDisplayLayout](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3674)  
50. [ShowCustomForm](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3716)  
51. [Slider](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3768)  
52. [ScrollableMessage](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3789)  
53. [PerformAudioPassThru](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3810)  
54. [EndAudioPassThru](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3815)  
55. [IsReady](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3820)  
56. [ClosePopUp](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3831)  
57. [SendHapticData](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3863)  

**Interface name="Navigation"**  
58. [IsReady](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3872)  
59. [SendLocation](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3913)  
60. [ShowConstantTBT](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3950)  
61. [AlertManeuver](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3961)  
62. [UpdateTurnList](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3974)  
63. [SetVideoConfig](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L3991)  
64. [StartStream](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4012)  
65. [StopStream](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4020)  
66. [StartAudioStream](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4031)  
67. [StopAudioStream](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4039)  
68. [GetWayPoints](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4062)  
69. [SubscribeWayPoints](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4081)  
70. [UnsubscribeWayPoints](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4088)  

**Interface name="VehicleInfo"**  
71. [IsReady](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4097)  
72. [GetVehicleType](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4105)  
73. [ReadDID](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4120)  
74. [GetDTCs](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4137)  
75. [DiagnosticMessage](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4166)  
76. [SubscribeVehicleData](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4255)  
77. [UnsubscribeVehicleData](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4413)  
78. [GetVehicleData](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4567)  

**Interface name="RC"**  
79. [IsReady](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4894)  
80. [GetCapabilities](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4903)  
81. [SetInteriorVehicleData](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4918)  
82. [GetInteriorVehicleData](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4936)  
83. [GetInteriorVehicleDataConsent](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/HMI_API.xml#L4957)


## Alternatives considered
N/A

## Introduction

This document contains a list of compatible smartphones that have been tested for receiving the Direct Remote ID signals from an Unmanned Aircraft (UA) or add-on device following the European and US standards.

The current specification of both the ASD-STAN prEN4709-02 (EU version) and the ASTM F3411-19 (US version) standards rely on wireless protocols in the unlicensed spectrum to broadcast the desired identification and telemetry data from UAs to ground observers.
Particularly, they define transport methods over Bluetooth 4 Legacy Advertising, Bluetooth 5 Long Range (Advertising Extensions over Coded PHY S8), WiFi Beacon and WiFi Neighbor Awareness Network (NAN).
The main reason for choosing these wireless technologies is that they meet the requirement of being supported on ordinary mobile devices.

Neither of the above standards cover the receiver side of the Direct Remote ID.
This document provides a quick overview of the compatibility of community tested smartphones with the specified broadcast Remote ID technologies.

The current stage of the prEN4709-02 (per January 2021) is a finished draft at CEN Enquiry, and the final version will be published in the upcoming months.
To obtain a copy of the ASTM Remote ID standard, please visit this [link](https://www.astm.org/Standards/F3411.htm).

**Disclaimer:** The list of tested devices is not exhaustive and comes without any guarantees.
It might contain some errors and misleading information.
There might even be some changes via SW updates from the smartphone manufacturers that can either improve or degrade the receiving capabilities.
If you find any incomplete, inconsistent, or wrong information, please feel free to open a GitHub issue.

## Testing Methodology

To determine if your smartphone can receive the Direct Remote ID messages, a simple test methodology is described below.
Testing is now defined only for Android phones, since iOS devices up until version 14 do not support Bluetooth 5 Long Range, WiFi NAN and the situation about WiFi Beacon support is unclear.
Testing the Bluetooth 4 Legacy Advertising support is irrelevant, since all existing Android and iOS models support it and no devices have been found to behave unexpectedly.

Please note that there should also be an advanced testing methodology, covering some hidden pitfalls of receiver implementations.
One of the examples is the Bluetooth Long Range feature, since it is an optional part of the Bluetooth 5.0 specification.
Some Android phones claim to support Long Range via the Android feature flags, but in reality, they only support advertising from the phone to the others, not reading the Remote ID information (scanning).
Another known pitfall for Bluetooth is that even if the phone supports scanning the Long Range advertisements, a power-saving feature will cause the loss of the majority of the Remote ID messages.
The work on the advanced testing methodology is now in progress.

### Bluetooth 5 Long Range Advertising - Elimination Criteria

This simple test is a prerequisite for Bluetooth 5 Long Range functionality support, as it can easily discard phones that don’t have sufficient capabilities to support the Long Range feature.
However, it doesn’t provide all the necessary information to prove the Long Range support.

1. Install [nRF Connect for Mobile](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp).
2. Open the side menu and go to Device information.
3. Check if the rows for the Long Range (PHY Coded) and Extended advertisement are both labeled YES.

Both of the rows must say <span style="color:lightgreen">**YES**</span>, in order for the device to be able to receive the Remote ID Bluetooth Long Range broadcast signals.

### Bluetooth 5 Long Range Support - the Ability to Receive Data

Suppose the previous test did not eliminate the device from Long Range support.
In that case, it is now necessary to test the target device’s basic receiving capability.
At the moment, this information cannot be read from the OS information and must be verified by using another device that supports the Long Range.

1. Prepare the Device Under Test (DUT) for the Long Range receiving support by running [nRF Connect for Mobile](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp).
  * In the Settings menu of the app, select Scanner and set the Scanning period to at least 5 minutes
2. Use another device (Master) that passed the Elimination Criteria and run [nRF Connect for Mobile](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp).
  * Alternatively: use any other device that supports Long Range advertisements (might be Remote ID add-on or Bluetooth development kit - e.g., [nRF52840 DK](https://www.nordicsemi.com/Software-and-Tools/Development-Kits/nRF52840-DK)).
3. On the Master, go to the Advertiser tab and create a New advertising packet.
  * In options, select Advertising Extensions and in both Primary and Secondary PHY, select LE Coded (Long Range).
  * Press the "Add Record" and select "Complete Local Name".
  * Set the Interval to 160 (resulting in 100 ms).
  * Set the Tx power to 1 dBm (the maximum).
  * Leave the other fields at default.
4. On the Master, run the Advertisement by toggling the switch. Select "Until manually turned off" and "No maximum options".
  * Alternatively: Start the add-on device or run the [Bluetooth Long Range sample](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/samples/bluetooth/central_hr_coded/README.html).
5. On the DUT, run the scanning in the Scanner tab and try to find the Master device.
  * It will have the device name of the Master.
  * If the Master is close to the DUT, it should have significantly better RSSI than other devices in the list.
  * Click the found device and verify that the Advertising type is Advertising Extension and that both the Primary and Secondary PHYs are LE Coded.
  * Click the round icon on the left to add the device as a favorite (that adds a small star banner under the round icon).
	Then click the filter at the top and select the "Only favorites" checkbox.
  * Swipe on the device to the right in order to reveal the RSSI chart.
	If you have more favorites devices present, notice the color of the DUT.
	Find the correct color in the chart and observe whether there are gaps in the chart or whether the signals are received continuously.
	Add this information to the report for the device.

### Wi-Fi Beacon Broadcast

The only testing solution so far seems to be to have a drone or add-on device that transmits the Wi-Fi Beacon messages and test their reception on the smartphone in OpenDroneID receiver app. 

### Wi-Fi NAN Broadcast

The easiest way to verify support is to read the Android Feature flags.

1. Install the [AIDA64](https://play.google.com/store/apps/details?id=com.finalwire.aida64) diagnostic app.
2. Open the Network menu.
3. Scroll down to the bottom and check the WiFi Aware flag.

### Contributing to the Repository

#### I have a phone that isn’t listed and I want to add it.

That’s great! Please follow the methodology to verify its capabilities and create a Pull Request with additional information and screenshots from the tests proving the support.

Alternatively, create an Issue with the necessary information and we will add it to the list.

#### I found misleading information in the list.

Well, that might happen.
Please create an Issue for that and we will do our best to inspect it.

#### Found a better way to test smartphones for Direct Remote ID capabilities?

Any contribution is welcome! Feel free to open an Issue so we can discuss it further.

### List of Devices and Their Capabilities

You can find the list of tested devices in the table below. For each device, we provide either ✅ if it passed or ❌ if it failed. Each test contains the approximate date it happened. Untested features are marked with ❓.

Please note that most smartphones were tested in Q1 2020 and they do not contain proof screenshots. Therefore, their functionality may have changed since. We plan to continuously update this list and increase the reliability of information by adding screenshot evidence.

| Smartphone Model                                 | Chipset           | BT 5 LR Basic Support (Elimination criteria) | BT 5 LR Receiver Support | Wi-Fi Beacon | Wi-Fi NAN  | Proof | Note                                                         |
| ------------------------------------------------ | ----------------- | -------------------------------------------- | ------------------------ | ------------ | ---------- | ----- | ------------------------------------------------------------ |
| Samsung Galaxy Note 10, Note 10+                 | Exynos 9825       | ❓                                            | ❓                        | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Samsung Galaxy Note 9 (Global)                   | Exynos 9810       | ❓                                            | ❓                        | ❓            | ❓          | ⚠️     |                                                              |
| Samsung Galaxy Note 9 (USA, China, Japan)        | Snapdragon 845    | ❓                                            | ❓                        | ❓            | ❓          | ⚠️     |                                                              |
| Samsung Galaxy Note 8 (Global)                   | Exynos 8895       | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Samsung Galaxy Note 8 (USA, China, Japan)        | Snapdragon 835    | ❓                                            | ❓                        | ❓            | ❓          | ⚠️     |                                                              |
| Samsung S20, S20+, S20 ultra (Global)            | Exynos 990        | ✅ - 1/2021                                   | ✅ - 1/2021               | ✅ - 1/2020   | ✅ - 1/2020 | ⚠️     |                                                              |
| Samsung S20, S20+, S20 ultra (USA, China, Japan) | Snapdragon 865    | ❓                                            | ❓                        | ❓            | ❓          | ⚠️     |                                                              |
| Samsung Galaxy S10, S10e, S10+, S10 5G           | Exynos 9820       | ✅ - 1/2021                                   | ✅ - 1/2021               | ✅ - 1/2020   | ✅ - 1/2020 | ⚠️     |                                                              |
| Samsung Galaxy S9, S9+ (Global)                  | Exynos 9810       | ❌ - 1/2020                                   | ❌ - 1/2020               | ✅ - 1/2020   | ❌ - 1/2020 | ⚠️     |                                                              |
| Samsung Galaxy S8                                | Exynos 8895       | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Samsung Galaxy A5                                | Snapdragon 410    | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Samsung Galaxy Xcover Pro                        | Exynos 9611       | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Samsung Galaxy Xcover Pro                        | Snapdragon 865    | ❓                                            | ❓                        | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Samsung Galaxy Tab S7, S7+                       | Snapdragon 865+   | ❓                                            | ❓                        | ❓            | ✅ - 1/2021 | ⚠️     |                                                              |
| Samsung Galaxy Tab S6                            | Snapdragon 855    | ✅ - 6/2020                                   | ❓                        | ❓            | ❓          | ⚠️     |                                                              |
| OnePlus 8T                                       | Snapdragon 865    | ✅ - 1/2021                                   | ✅ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     | Long Range receiver implements power saving feature which does not do continuous scanning |
| One Plus 7 Pro                                   | Snapdragon 855    | ✅ - 1/2020                                   | ✅ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     | Probably similar power saving feature as in OnePlus 6 and OnepPlus 8T (unconfirmed) |
| One Plus 7T                                      | Snapdragon 855+   | ✅ - 1/2020                                   | ✅ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     | Probably similar power saving feature as in OnePlus 6 and OnepPlus 8T (unconfirmed) |
| One Plus 6 / 6T                                  | Snapdragon 845.   | ✅ - 1/2020                                   | ✅ - 1/2021               | ✅ - 1/2020   | ❌ - 1/2020 | ⚠️     | Long Range receiver implements power saving feature which does not do continuous scanning |
| Huawei Mate 20 Pro                               | Kirin 980         | ✅ - 1/2020                                   | ✅ - 1/2020               | ❌ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei Mate 10 Pro                               | Kirin 970         | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei Mate 20                                   | Kirin 980         | ✅ - 11/2019                                  | ✅ - 11/2019              | ❓            | ❓          | ⚠️     |                                                              |
| Huawei Mate 9                                    | Kirin 960         | ❓                                            | ❓                        | ❌ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei P30 Pro                                   | Kirin 980         | ✅ - 11/2019                                  | ✅ - 11/2019              | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei P20 Lite                                  | Kirin 659         | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei P30                                       | Kirin 980         | ✅ - 11/2019                                  | ✅ - 11/2019              | ❓            | ❓          | ⚠️     |                                                              |
| Huawei P9                                        | Kirin 955         | ❓                                            | ❓                        | ❌ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei P8 Lite                                   | Kirin 655         | ❓                                            | ❓                        | ❌ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei Nova 5T                                   | Kirin 980         | ✅ - 5/2020                                   | ❓                        | ❓            | ❓          | ⚠️     |                                                              |
| Huawei Honor Magic 2                             | Kirin 980         | ✅ - 1/2020                                   | ❓                        | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Huawei Honor 10 lite                             | Kirin 710         | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei Honor View 10                             | Kirin 970         | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei Honor 8S                                  | MT 6761 Helio A22 | ✅ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     | Not tested but expect the same behavior as the Nokia 2.2     |
| Huawei Y6 Pro                                    | MT 6761 Helio A22 | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Huawei MediaPad M5                               | Kirin 960s        | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Huawei Nexus 6P                                  | Snapdragon 810    | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Xiaomi Note 10                                   | Snapdragon 730G   | ✅ - 1/2020                                   | ✅ - 1/2020               | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Mi 9T Pro                                 | Snapdragon 855    | ✅ - 1/2020                                   | ✅ - 1/2020               | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Mi 9 SE                                   | Snapdragon 712    | ✅ - 1/2020                                   | ✅ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Mi 9                                      | Snapdragon 855    | ✅ - 1/2020                                   | ✅ - 1/2020               | ❓            | ✅ - 1/2020 | ⚠️     | Long Range receiver implements power saving feature - works 5 seconds, pauses 15 seconds and repeats |
| Xiaomi Mi 8                                      | Snapdragon 845    | ❓                                            | ❓                        | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Redmi Note 9s                             | Snapdragon 720G   | ✅ - 6/2020                                   | ❓                        | ❓            | ❓          | ⚠️     |                                                              |
| Xiaomi Redmi note 8 Pro                          | MT Helio G90T     | ✅ - 1/2020                                   | ✅ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Redmi note 7 Pro                          | Snapdragon 675    | ✅ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❓          | ⚠️     |                                                              |
| Xiaomi Redmi note 8T                             | Snapdragon 665    | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Redmi note 7                              | Snapdragon 660    | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Redmi CC9 Pro/Note10 Pro                  | Snapdragon 730G   | ❓                                            | ❓                        | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Redmi K20 Pro                             | Snapdragon 855    | ❓                                            | ❓                        | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Mi Mix 3                                  | Snapdragon 845    | ❓                                            | ❓                        | ✅ - 1/2020   | ✅ - 1/2020 | ⚠️     |                                                              |
| Xiaomi Mi A2                                     | Snapdragon 660    | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Google Pixel 4/4XL                               | Snapdragon 855    | ❓                                            | ❓                        | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Google Pixel 3/3XL                               | Snapdragon 845    | ❌ - 1/2019                                   | ❌ - 1/2019               | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Google Pixel 3A                                  | Snapdragon 670    | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ✅ - 1/2020 | ⚠️     |                                                              |
| Google Pixel 2/2XL                               | Snapdragon 835    | ❓                                            | ❓                        | ✅ - 1/2020   | ✅ - 1/2020 | ⚠️     |                                                              |
| Sony XQ-AD52 Xperia L4                           | MT6762 Helio P22  | ✅ - 1/2021                                   | ❌ - 1/2021               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Sony Xperia XA2                                  | Snapdragon 630    | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Sony Xperia XZ1 Compact                          | Snapdragon 835    | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Sony Xperia XZ2                                  | Snapdragon 845    | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| Motorola One Vision                              | Exynos 9609       | ✅ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Razer phone 2                                    | Snapdragon 845    | ✅ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❓          | ⚠️     |                                                              |
| Nokia 9 Pureview                                 | Snapdragon 845    | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| HMD Global Nokia 7.2                             | Snapdragon 660    | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| HMD Global Nokia 6.2                             | Snapdragon 636    | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| HMD Global Nokia 2.2                             | MT 6761 Helio A22 | ✅ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| Moto G 6 plus                                    | Snapdragon 630    | ❌ - 1/2020                                   | ❌ - 1/2020               | ❓            | ❌ - 1/2020 | ⚠️     |                                                              |
| HTC one M9                                       | Snapdragon 810    | ❌ - 1/2020                                   | ❌ - 1/2020               | No           | ❌ - 1/2020 | ⚠️     |                                                              |
| LG velvet 5G                                     | Snapdragon 765G   | ❓                                            | ❓                        | ❓            | ✅ - 1/2021 | ⚠️     |                                                              |
| LG G8X                                           | Snapdragon 855    | ❓                                            | ❓                        | ❓            | ✅ - 1/2021 | ⚠️     |                                                              |
| LG G5                                            | Snapdragon 820    | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| LG V60                                           | Snapdragon 865    | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| LG Nexus 5X                                      | Snapdragon 808    | ❓                                            | ❓                        | ✅ - 1/2020   | ❓          | ⚠️     |                                                              |
| LG X Cam                                         | MT 6735           | ❓                                            | ❓                        | ❌ - 1/2020   | ❓          | ⚠️     |                                                              |
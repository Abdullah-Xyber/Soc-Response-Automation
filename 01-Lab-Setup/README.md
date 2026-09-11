# Lab Setup

## Overview

This stage prepared the Windows endpoint for the SOC Response Automation project. I created a local Windows virtual machine in VirtualBox, enrolled it in LimaCharlie, and verified that endpoint activity was visible in the platform.

I used VirtualBox instead of the tutorial’s Vultr cloud instance to run the endpoint locally without cloud hosting costs.

## Lab Environment

| Component | Configuration |
|---|---|
| Virtualization platform | Oracle VirtualBox |
| Operating system | Windows Server 2022 Evaluation with Desktop Experience |
| Endpoint name | SOC-EDR-WIN01 |
| Network mode | NAT |
| EDR platform | LimaCharlie |
| Organization | SOC-Response-Automation |
| Sensor tag | soc-lab |

## 1. Windows VM Preparation

I installed Windows Server 2022 inside VirtualBox and configured the VM to use NAT networking. This provided the internet connectivity required to download the LimaCharlie sensor and communicate with its cloud platform.

The Windows VM serves as the monitored endpoint for the project. LimaCharlie is accessed through its web interface.

## 2. LimaCharlie Organization

I created a LimaCharlie organization named `SOC-Response-Automation` and selected **No template** to configure the project manually.

The organization initially displayed system extension sensors. These were separate from the Windows endpoint enrolled in the following steps.

## 3. Installation Key

I selected **Add Sensor**, chose Windows, and created an installation key with the following settings:

| Setting | Value |
|---|---|
| Description | SOC-EDR-WIN01-Installation |
| Tag | soc-lab |
| Use Public CA | Disabled |

The installation key associates the sensor with the correct LimaCharlie organization. The `soc-lab` tag is applied to sensors enrolled using that key.

Installation keys and other credentials are omitted from this documentation.

## 4. Sensor Installation

Inside the Windows VM, I opened PowerShell as administrator and ran the installation command provided by LimaCharlie.

The command downloaded the Windows sensor and installed it as a Windows service. The installer confirmed that the agent installation completed successfully.

![Successful LimaCharlie sensor installation](sensor-installation.png)

## 5. Endpoint Enrollment Verification

After installation, I returned to the LimaCharlie Sensors list and confirmed that the Windows endpoint appeared online.

This verified that the sensor had enrolled successfully and could communicate with the LimaCharlie platform.

![Windows endpoint online in the LimaCharlie Sensors list](sensor-list.png)

## 6. Telemetry Verification

To generate a recognizable process event, I launched Notepad from PowerShell inside the Windows VM:

```powershell
notepad.exe
```

The following screenshot shows Notepad running successfully on the monitored Windows endpoint.

![Notepad launched inside the Windows VM](process-event-notepad.png)

I then opened the sensor’s **Live Feed** in LimaCharlie and located the corresponding Notepad process event.

![Notepad process event captured in the LimaCharlie Live Feed](process-event-feed.png)

This confirmed that the LimaCharlie sensor was successfully collecting and reporting process activity from the Windows VM. This test verified telemetry collection only; the custom detection rule will be created during the next stage.

## 7. Working-State Snapshot

After verifying the sensor connection and endpoint telemetry, I shut down the Windows VM and created a VirtualBox snapshot named:

`02-LimaCharlie-Connected`

The snapshot provides a working restore point containing the configured Windows endpoint and its active LimaCharlie sensor before proceeding to detection testing and response automation.

## Results

| Check | Outcome |
|---|---|
| Windows VM prepared | Completed |
| NAT network connectivity | Confirmed |
| LimaCharlie organization created | Completed |
| Installation key created | Completed |
| Sensor installed | Successful |
| Endpoint enrollment | Confirmed online |
| Live endpoint telemetry | Verified |
| Notepad process event | Captured |
| Working-state snapshot | Created |

The lab setup is complete. The next stage will generate the required test activity and create a custom LimaCharlie detection and response rule.

[Return to the project overview](../README.md)

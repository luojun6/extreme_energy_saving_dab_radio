# Extreme Energy Saving Strategy for FICM - DAB Radio

## 1 Overview

### 1.1 Background

From the eletrical-battery-power-train vehicle perspective, any consumption of low-voltage-system eletrical components might produce the non-essentional waste of battery power, which could has impact to vehicle users' real capable mileage.

Therefore, this solution is trying to save low-voltage-system eletrical components consumption while users are not using. In addition, the applied energy saving solution would not cause any obvious unhappy exerperience from user standpoint.

### 1.2 Requirements Brief

Regarding to the external DAB radio module which has approximately 6W power consumption.

If an eletrical vehicle's power efficiency is 9km/1kwh, saving 6W power consumption could contribute approximately extra 6 multiplies 9 equals 54m of moving range per hour.

Therefore it is valuable to design and apply an "extreme" energy saving strategy for this extra 54m moving range from user usage perspective.

### 1.3 Acronyms

- FICM: Front Infortainment Control Module
- DAB: Digital Audio Broadcasting
- FM/AM Radio: Frequency Modulation/Analog Modulation Radio

## 2 Scope

### 2.1 Functional Scope

This documentation is **ONLY** applicable for the "extreme" energy saving strategy of _FICM_ radio functions:

- DAB
- FM/AM

### 2.2 Vehicle Program Scope

This documentation is **ONLY** applicable for SAIC international MG brand vehicles which installed the particular _FICM_ with _external DAB radio module_, or _independent DAB radio module_.

### 2.3 FICM Hardware Platform Scope

This documentation is **ONLY** applicable for _SAIC FICM2712 Platform with external DAB radio module_. From the _DAB radio module_ perspective, it is so-called "DAB Radio Distrubuted Solution" on the left side as bellow diagram.

### 2.3.1 DAB Radio Hardware Block Diagram - Distribution

![DAB Radio Hardware Block Diagram - Distribution](../res/images/dab_radio_hardware_block_diagram.drawio_distribution.png)

### 2.3.2 DAB Radio Hardware Block Diagram - Integration

![DAB Radio Hardware Block Diagram - Integration](../res/images/dab_radio_hardware_block_diagram.drawio_integration.png)

### 2.4 Regulation

- EN50067
- IEC62106
- UKSI 2017 / 1206 for radio equipment 2017 and all its amendments
- OFCOM standards
- etc.

Accessment: If the customer has physically and purposefullyswitched the radio unit off. Then should be ok for no broadcastfunctionality. The unit should not switch itself off whiledriving.  It must be a customer intervention.

## 3 Entities

### req_ees_ent_000_headunitpowermode_HeadunitPowerMode

```json
{
  "EntityID": "REQ_EES_ENT_000",
  "EntityName": "HeadunitPowerMode",
  "EntityType": "",
  "DataType": "Enumeration",
  "ValueRange": { "shutdown": 0, "suspendToRAM": 1, "running": 2 },
  "InitialValue": 0,
  "Comments": ""
}
```

### REQ_EES_ENT_001_DABRadioPowerStatus

```json
{
  "EntityID": "REQ_EES_ENT_001",
  "EntityName": "DABRadioPowerStatus",
  "EntityType": "",
  "DataType": "Enumeration",
  "ValueRange": { "shutdown": 0, "suspended": 1, "running": 2 },
  "InitialValue": 0,
  "Comments": ""
}
```

### req_ees_ent_010_dabradioshutdownbutton_DABRadioShutdownButton

```json
{
  "EntityID": "REQ_EES_ENT_010",
  "EntityName": "DABRadioShutdownButton",
  "EntityType": "Button",
  "DataType": "Enumeration",
  "ValueRange": { "open": 0, "shutdown": 1, "disabled": 2 },
  "InitialValue": 0,
  "Comments": ""
}
```

### REQ_EES_ENT_020_ForbittenShutdownPopup

```json
{
  "EntityID": "REQ_EES_ENT_020",
  "EntityName": "ForbittenShutdownPopup",
  "EntityType": "Popup",
  "DataType": "",
  "ValueRange": "",
  "InitialValue": "",
  "Comments": "Info user is not allowed to shutdown DAB radio while radio is running."
}
```

## 4 Functional Requirements

### 4.1 REQ_EES_DABRADIO_000

#### 4.1.1 Description

User is allowed to shutdown DAB radio module while any radio feature not being running.

#### 4.1.2 Pre-conditions

- FICM [HeadunitPowerMode](#req_ees_ent_000_headunitpowermode_headunitpowermode) is at _running_
- [DABRadioShutdownButton](#req_ees_ent_010_dabradioshutdownbutton_dabradioshutdownbutton) button has been in _open_ status
- Radio feature is not running, included:
  - Audio output channel is not occupied by **DAB/FM/AM**
  - Media source is not selected for **DAB/FM/AM**

#### 4.1.3 Use Cases

- [EES_UC_000: User shutdown DAB Radio module](#51-ees_uc_000-user-shutdown-dab-radio-module)

#### 4.1.4 Post-conditions

- [DABRadioShutdownButton](#req_ees_ent_010_dabradioshutdownbutton) button has been in _shutdwon_ status
- [DABRadioPowerStatus](#req_ees_ent_001_dabradiopowerstatus)

#### 4.1.5 Dependecies

N/A

### 4.2 REQ_EES_DABRADIO_001

#### 4.2.1 Description

User is **NOT** allowed to shutdown DAB radio module while any radio feature not being running.

#### 4.2.2 Pre-conditions

- FICM [HeadunitPowerMode](#req_ees_ent_000_headunitpowermode) is at _running_
- [DABRadioShutdownButton](#req_ees_ent_010_dabradioshutdownbutton) button has been in _open_ status
- Radio feature is running, included:
  - Audio output channel is occupied by **DAB/FM/AM**
  - Media source is selected for **DAB/FM/AM**

#### 4.2.3 Use Cases

- [EES_UC_000: User shutdown DAB Radio module](#51-ees_uc_000-user-shutdown-dab-radio-module)

#### 4.2.4 Post-conditions

- [DABRadioShutdownButton](#req_ees_ent_010_dabradioshutdownbutton) button has been in _open_ status
- [ForbittenShutdownPopup](#REQ_EES_ENT_020) shows up

### 4.3 REQ_EES_DABRADIO_002

#### 4.3.1 Description

User is **ALWAYS** to have radio feature.

- [EES_UC_001: User use radio feature](#52-ees_uc_001-user-use-radio-feature)

#### 4.3.2 Pre-conditions

- FICM [HeadunitPowerMode](#req_ees_ent_000_headunitpowermode) is at _running_

#### 4.3.3 Use Cases

#### 4.3.4 Post-conditions

- [DABRadioShutdownButton](#req_ees_ent_010_dabradioshutdownbutton) button has been in _open_ status

## 5 Use Cases

### 5.1 EES_UC_000: User shutdown DAB Radio module

### 5.2 EES_UC_001: User use radio feature

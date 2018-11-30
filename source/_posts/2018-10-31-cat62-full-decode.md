---
layout: single
classes: wide
title:  "CAT62的全数据译码"
date:   2018-10-31 22:44:20 +0800
categories: cat eurocontrol
---

# CAT
```java
(byte)0b00111110, //0
```
- CAT = 00111110 = 62

---

# LEN
```java
(byte)0b00000001  //1
(byte)0b10101001  //2
```
- LEN = 00000001 10101001 = 425

---

<!--more-->

# FSPEC
```java
(byte)0b10111111  //3
(byte)0b11111111  //4
(byte)0b11111111  //5
(byte)0b11111111  //6
(byte)0b00000000  //7
```
- F1 = 1 = Presence
- F2 = 0 = Spare
- F3 = 1 = Presence
- F4 = 1 = Presence
- F5 = 1 = Presence
- F6 = 1 = Presence
- F7 = 1 = Presence
- FX1 = 1 = extension
- F8 = 1 = Presence
- F9 = 1 = Presence
- F10 = 1 = Presence
- F11 = 1 = Presence
- F12 = 1 = Presence
- F13 = 1 = Presence
- F14 = 1 = Presence
- FX2 = 1 = extension
- F15 = 1 = Presence
- F16 = 1 = Presence
- F17 = 1 = Presence
- F18 = 1 = Presence
- F19 = 1 = Presence
- F20 = 1 = Presence
- F21 = 1 = Presence
- FX3 = 1 = extension
- F22 = 1 = Presence
- F23 = 1 = Presence
- F24 = 1 = Presence
- F25 = 1 = Presence
- F26 = 1 = Presence
- F27 = 1 = Presence
- F28 = 1 = Presence
- FX4 = 1 = extension
- F29 = 0 = Spare
- F30 = 0 = Spare
- F31 = 0 = Spare
- F32 = 0 = Spare
- F33 = 0 = Spare
- F34 = 0 = Reserved Expansion Field
- F35 = 0 = Reserved For Special Purpose Indicator
- FX5 = 0 = no extension

---

# Data Fields

## F1 Data Source Identifier
```java
(byte)0b10010000, //8
(byte)0b10010000, //9
```
- SAC = 10010000
- SIC = 10010000

> [SAC/SIC定义](https://www.eurocontrol.int/services/system-area-code-list)

## F2 Spare
**预留为空**

## F3 Service Identification
```java
(byte)0b00000001, //10
```
**系统分配**

## F4 Time Of Track Information
```java
(byte)0b00101110, //11
(byte)0b10011000, //12
(byte)0b01011001, //13
```
- LSB = 1/128 s
- DEC = 00101110 10011000 01011001= 3053657
- Time = DEC*LSB = 23857秒 (从每日00:00:00点计时)

## F5 Calculated Position In WGS-84 Co-ordinates
```java
(byte)0b00000000, //14
(byte)0b01001111, //15
(byte)0b01011000, //16
(byte)0b01110000, //17
```
- LSB = 180/2^25 degrees
- DEC = 00000000 01001111 01011000 01110000 = 5199984
- Latitude = DEC*LSB = 27.89 度

```java
(byte)0b00000001, //18
(byte)0b00100111, //19
(byte)0b11101111, //20
(byte)0b11101110, //21
```
- LSB = 180/2^25 degrees
- DEC = ‭00000001 00100111 11101111 11101110 = 19394542‬
- Longitude = DEC*LSB = 104.0404308 度

## F6 Calculated Track Position (Cartesian)
```java
(byte)0b00000000, //22
(byte)0b01000110, //23
(byte)0b11011111, //24
```
- LSB = 0.5 m
- DEC = 00000000 01000110 11011111 = 18143
- X = DEC*LSB = 9072 米

```java
(byte)0b11110110, //25
(byte)0b11101100, //26
(byte)0b01011000, //27
```
- LSB = 0.5 m
- DEC = 11110110 11101100 01011000 = 16182360
- Y = DEC*LSB = 8091180 米

## F7 Calculated Track Velocity (Cartesian)
```java
(byte)0b00000001, //28
(byte)0b11010011, //29
```
- LSB = 0.25 m/s
- DEC = 00000001 11010011 = 467
- Vx = DEC*LSB = 116 米/秒

```java
(byte)0b00000011, //30
(byte)0b01101111, //31
```
- LSB = 0.25 m/s
- DEC = 00000011 01101111 = 879
- Vy = DEC*LSB = 219 米/秒

## F8 Calculated Acceleration (Cartesian)
```java
(byte)0b01100100, //32
```
- LSB = 0.25 m/s^2
- DEC = 01100100 = 100
- Ax = 25 米/秒平方

```java
(byte)0b01100100, //33
```
- LSB = 0.25 m/s^2
- DEC = 01100100 = 100
- Ay = 25 米/秒平方

## F9 Track Mode 3/A Code
```java
(byte)0b00000111, //34
(byte)0b01010011, //35
```
- V = 0 = Code validated
- G = 0 = Default
- CH = 0 = No Change
- Mode-3/A reply = 011 101 010 011 = 3523

## F10 Target Identification
```java
(byte)0b00000000, //36
(byte)0b11000011, //37
(byte)0b00001100, //38
(byte)0b00110000, //39
(byte)0b11000011, //40
(byte)0b00001100, //41
(byte)0b00110000, //42
```
- STI = 00 = Callsign or registration downlinked from target
- Characters 1 = 110000 = 48 > ASCII = '0'
- Characters 2 = 110000 = 48 > ASCII = '0'
- Characters 3 = 110000 = 48 > ASCII = '0'
- Characters 4 = 110000 = 48 > ASCII = '0'
- Characters 5 = 110000 = 48 > ASCII = '0'
- Characters 6 = 110000 = 48 > ASCII = '0'
- Characters 7 = 110000 = 48 > ASCII = '0'
- Characters 8 = 110000 = 48 > ASCII = '0'
  
## F11 Aircraft Derived Data
### F11-0 Primary Subfield
```java
(byte)0b11111111, //43
(byte)0b11111111, //44
(byte)0b11111111, //45
(byte)0b11111110, //46  
```
- ADR = 1 = Presence
- ID = 1 = Presence
- MHG = 1 = Presence
- IAS = 1 = Presence
- TAS = 1 = Presence
- SAL = 1 = Presence
- FSS = 1 = Presence
- FX1 = 1 = extension
- TIS = 1 = Presence
- TID = 1 = Presence
- COM = 1 = Presence
- SAB = 1 = Presence
- ACS = 1 = Presence
- BVR = 1 = Presence
- GVR = 1 = Presence
- FX2 = 1 = extension
- RAN = 1 = Presence
- TAR = 1 = Presence
- TAN = 1 = Presence
- GSP = 1 = Presence
- VUN = 1 = Presence
- MET = 1 = Presence
- EMC = 1 = Presence
- FX3 = 1 = extension
- POS = 1 = Presence
- GAL = 1 = Presence
- PUN = 1 = Presence
- MB = 1 = Presence
- IAR = 1 = Presence
- MAC = 1 = Presence
- BPS = 1 = Presence
- FX4 = 0 = no extension

### F11-1 Target Address
```java
(byte)0b00000000, //47
(byte)0b00000000, //48
(byte)0b00000001, //49
```
- Address = 00000000 00000000 00000001

### F11-2 Target Identification
```java
(byte)0b11000011, //50
(byte)0b00001100, //51
(byte)0b00110000, //52
(byte)0b11000011, //53
(byte)0b00001100, //54
(byte)0b00110000, //55
```
- Characters 1 = 110000 = 48 > ASCII = '0'
- Characters 2 = 110000 = 48 > ASCII = '0'
- Characters 3 = 110000 = 48 > ASCII = '0'
- Characters 4 = 110000 = 48 > ASCII = '0'
- Characters 5 = 110000 = 48 > ASCII = '0'
- Characters 6 = 110000 = 48 > ASCII = '0'
- Characters 7 = 110000 = 48 > ASCII = '0'
- Characters 8 = 110000 = 48 > ASCII = '0'

### F11-3 Magnetic Heading
```java
(byte)0b00000000, //56
(byte)0b01100100, //57
```
- LSB = 360/2^16 degree
- DEC = 00000000 01100100 = 100
- heading = DEC*LSB = 0.549316406

### F11-4 Indicated Airspeed / Mach No
```java
(byte)0b00100111, //58
(byte)0b00010000, //59
```
- IM = 0 = IAS
- LSB = 1/2^14 nm/s
- DEC = 0100111  00010000= 10000
- ias = DEC*LSB = 0.6103516 海里/秒 节 knots

### F11-5 True Airspeed
```java
(byte)0b00000000, //60
(byte)0b01100100, //61
```
- LSB = 1 knot
- DEC = 00000000 01100100 = 100
- tas = 100 knots

### F11-6 Selected Altitude
```java
(byte)0b01000000, //62
(byte)0b01100100, //63
```
- SAS = 0 = No source information provided
- Source = 10 = FCU/MCP Selected Altitude
- LSB = 25 ft
- DEC = 00000 01100100 = 100
- Altitude = DEC*LSB = 2500 ft

### F11-7 Final State Selected Altitude
```java
(byte)0b11100000, //64
(byte)0b01100100, //65
```
- MV = Active
- AH = Active
- AM = Active
- LSB = 25 ft
- DEC = 00000 01100100 = 100
- altitude = DEC*LSB = 2500 ft

### F11-8 Trajectory Intent Status
```java
(byte)0b00000000, //66
```
- NAV = 0 = Trajectory Intent Data is available for this aircraft
- NVB = 0 = Trajectory Intent Data is valid
- FX = 0

### F11-9 Trajectory Intent Data
#### REP
```java
(byte)0b00000101, //67
```
- REP = 5

#### DATA-1
```java
(byte)0b00000001, //68
(byte)0b00000000, //69
(byte)0b01100100, //70
(byte)0b00000000, //71
(byte)0b00100111, //72
(byte)0b00010000, //73
(byte)0b00000000, //74
(byte)0b00100111, //75
(byte)0b00010000, //76
(byte)0b00011110, //77
(byte)0b00000000, //78
(byte)0b00100111, //79
(byte)0b00010000, //80
(byte)0b00000000, //81
(byte)0b01100100, //82
```
- TCA = 0
- NC = 0
- TCP Number = 000001 = 1
- Altitude
    - LSB = 10 ft
    - DEC = 00000000 01100100 = 100
    - Altitude = 1000 ft
- Latitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Latitude = DEC*LSB = 0.214576721 度
- Longitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Longitude = DEC*LSB = 0.214576721 度
- Point Type= 0001 = Fly by waypoint
- TD = 11 = No turn
- TRA = 1 = TTR available
- TOA = 0 = TOV available
- TOV
    - LSB = 1 s
    - DEC = 00000000 00100111 00010000 = 10000
    - TOV = DEC*LSB = 10000 秒
- TTR
    - LSB = 0.01 Nm
    - DEC = 00000000 01100100 = 100
    - TTR = DEC*LSB = 100 海里

#### DATA-2
```java
(byte)0b00000010, //83
(byte)0b00000000, //84
(byte)0b01100100, //85
(byte)0b00000000, //86
(byte)0b00100111, //87
(byte)0b00010000, //88
(byte)0b00000000, //89
(byte)0b00100111, //90
(byte)0b00010000, //91
(byte)0b00011110, //92
(byte)0b00000000, //93
(byte)0b00100111, //94
(byte)0b00010000, //95
(byte)0b00000000, //96
(byte)0b01100100, //97
```
- TCA = 0
- NC = 0
- TCP Number = 000010 = 2
- Altitude
    - LSB = 10 ft
    - DEC = 00000000 01100100 = 100
    - Altitude = 1000 ft
- Latitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Latitude = DEC*LSB = 0.214576721 度
- Longitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Longitude = DEC*LSB = 0.214576721 度
- Point Type= 0001 = Fly by waypoint
- TD = 11 = No turn
- TRA = 1 = TTR available
- TOA = 0 = TOV available
- TOV
    - LSB = 1 s
    - DEC = 00000000 00100111 00010000 = 10000
    - TOV = DEC*LSB = 10000 秒
- TTR
    - LSB = 0.01 Nm
    - DEC = 00000000 01100100 = 100
    - TTR = DEC*LSB = 100 海里

#### DATA-3
```java
(byte)0b00000011, //98
(byte)0b00000000, //99
(byte)0b01100100, //100
(byte)0b00000000, //101
(byte)0b00100111, //102
(byte)0b00010000, //103
(byte)0b00000000, //104
(byte)0b00100111, //105
(byte)0b00010000, //106
(byte)0b00011110, //107
(byte)0b00000000, //108
(byte)0b00100111, //109
(byte)0b00010000, //110
(byte)0b00000000, //111
(byte)0b01100100, //112
```
- TCA = 0
- NC = 0
- TCP Number = 000011 = 3
- Altitude
    - LSB = 10 ft
    - DEC = 00000000 01100100 = 100
    - Altitude = 1000 ft
- Latitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Latitude = DEC*LSB = 0.214576721 度
- Longitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Longitude = DEC*LSB = 0.214576721 度
- Point Type= 0001 = Fly by waypoint
- TD = 11 = No turn
- TRA = 1 = TTR available
- TOA = 0 = TOV available
- TOV
    - LSB = 1 s
    - DEC = 00000000 00100111 00010000 = 10000
    - TOV = DEC*LSB = 10000 秒
- TTR
    - LSB = 0.01 Nm
    - DEC = 00000000 01100100 = 100
    - TTR = DEC*LSB = 100 海里

#### DATA-4
```java
(byte)0b00000100, //113
(byte)0b00000000, //114
(byte)0b01100100, //115
(byte)0b00000000, //116
(byte)0b00100111, //117
(byte)0b00010000, //118
(byte)0b00000000, //119
(byte)0b00100111, //120
(byte)0b00010000, //121
(byte)0b00011110, //122
(byte)0b00000000, //123
(byte)0b00100111, //124
(byte)0b00010000, //125
(byte)0b00000000, //126
(byte)0b01100100, //127
```
- TCA = 0
- NC = 0
- TCP Number = 000100 = 4
- Altitude
    - LSB = 10 ft
    - DEC = 00000000 01100100 = 100
    - Altitude = 1000 ft
- Latitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Latitude = DEC*LSB = 0.214576721 度
- Longitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Longitude = DEC*LSB = 0.214576721 度
- Point Type= 0001 = Fly by waypoint
- TD = 11 = No turn
- TRA = 1 = TTR available
- TOA = 0 = TOV available
- TOV
    - LSB = 1 s
    - DEC = 00000000 00100111 00010000 = 10000
    - TOV = DEC*LSB = 10000 秒
- TTR
    - LSB = 0.01 Nm
    - DEC = 00000000 01100100 = 100
    - TTR = DEC*LSB = 100 海里

#### DATA-5
```java
(byte)0b00000101, //128
(byte)0b00000000, //129
(byte)0b01100100, //130
(byte)0b00000000, //131
(byte)0b00100111, //132
(byte)0b00010000, //133
(byte)0b00000000, //134
(byte)0b00100111, //135
(byte)0b00010000, //136
(byte)0b00011110, //137
(byte)0b00000000, //138
(byte)0b00100111, //139
(byte)0b00010000, //140
(byte)0b00000000, //141
(byte)0b01100100, //142
```
- TCA = 0
- NC = 0
- TCP Number = 000101 = 5
- Altitude
    - LSB = 10 ft
    - DEC = 00000000 01100100 = 100
    - Altitude = 1000 ft
- Latitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Latitude = DEC*LSB = 0.214576721 度
- Longitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Longitude = DEC*LSB = 0.214576721 度
- Point Type= 0001 = Fly by waypoint
- TD = 11 = No turn
- TRA = 1 = TTR available
- TOA = 0 = TOV available
- TOV
    - LSB = 1 s
    - DEC = 00000000 00100111 00010000 = 10000
    - TOV = DEC*LSB = 10000 秒
- TTR
    - LSB = 0.01 Nm
    - DEC = 00000000 01100100 = 100
    - TTR = DEC*LSB = 100 海里
  
### F11-10 Communications/ACAS Capability and Flight Status reported by Mode-S
```java
(byte)0b00100100, //143
(byte)0b11111111, //144
```
- COM = 001 = 1 = Comm. A and Comm. B capability
- STAT = 001 = 1 = No alert, no SPI, aircraft on ground
- SSC = 1 = Yes
- ARC = 1 = 25 ft resolution
- AIC = 1 = Yes
- B1A = 1
- B1B = 1

### F11-11 Status reported by ADS-B
```java
(byte)0b10100110, //145
(byte)0b00000001, //146
```
- AC = 10 = ACAS operational
- MN = 10 = Multiple navigational aids operating
- DC = 01 = Differential correction
- GBS = 1 = Transponder Ground Bit set
- STAT = 001 = 1 = General emergency


### F11-12 ACAS Resolution Advisory Report
```java
(byte)0b00000000, //147
(byte)0b00000000, //148
(byte)0b00000000, //149
(byte)0b00000000, //150
(byte)0b00000000, //151
(byte)0b00000000, //152
(byte)0b00000001, //153
```
- MB Data = 00000000 00000000 00000000 00000000 00000000 00000000 00000001


### F11-13 Barometric Vertical Rate
```java
(byte)0b00100111, //154
(byte)0b00010000, //155
```
- LSB = 6.25 feet/minute
- DEC = 00100111 00010000 = 10000
- rate = DEC*LSB = 62500 ft/min


### F11-14 Geometric Vertical Rate
```java
(byte)0b00100111, //156
(byte)0b00010000, //157
```
- LSB = 6.25 feet/minute
- DEC = 00100111 00010000 = 10000
- rate = DEC*LSB = 62500 ft/min


### F11-15 Roll Angle
```java
(byte)0b00100111, //158
(byte)0b00010000, //159
```
- LSB = 0.01 degree
- DEC = 00100111 00010000 = 10000
- rate = DEC*LSB = 100 deg


### F11-16 Track Angle Rate
```java
(byte)0b11000000, //160
(byte)0b00000010, //161
```
- TI = 01 = Left
- LSB = 0.25 deg/s
- DEC = 0000001 = 1
- rate = DEC*LSB = 0.25 deg/s


### F11-17 Track Angle
```java
(byte)0b00100111, //162
(byte)0b00010000, //163
```
- LSB = 360/2^16
- DEC = 00100111 00010000 = 10000
- angle = DEC*LSB = 55 deg


### F11-18 Ground Speed
```java
(byte)0b00100111, //164
(byte)0b00010000, //165
```
- LSB = 1/2^14 NM/s
- DEC = 00100111 00010000 = 10000
- gs = DEC*LSB = 2200 kt


### F11-19 Velocity Uncertainty
```java
(byte)0b00000001, //166
```
- vun = 00000001


### F11-20 Met Data
```java
(byte)0b11110000, //167
(byte)0b00000000, //168
(byte)0b00000001, //169
(byte)0b00000000, //170
(byte)0b00000001, //171
(byte)0b00000000, //172
(byte)0b00000001, //173
(byte)0b00000001, //174
```
- WS = 1 = Valid
- WD = 1 = Valid
- TMP = 1 = Valid
- TRB = 1 = Valid
- Wind Speed
    - LSB = 1 kt
    - DEC = 00000000 00000001 = 1
    - speed = DEC*LSB = 1 kt
- Wind Direction
    - LSB = 1 deg
    - DEC = 00000000 00000001 = 1
    - direction = DEC*LSB = 1 deg
- Temperature
    - LSB = 0.25 deg
    - DEC = 00000000 00000001 = 1
    - temperature = DEC*LSB = 1 deg
- turbulence = 00000001 = 1


### F11-21 Emitter Category
```java
(byte)0b00000001, //175
```
- category = 00000001 = 1 = light aircraft


### F11-22 Position
```java
(byte)0b00000000, //176
(byte)0b00100111, //177
(byte)0b00010000, //178
(byte)0b00000000, //179
(byte)0b00100111, //180
(byte)0b00010000, //181
```
- Latitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Latitude = DEC*LSB = 0.214576721
- Longitude
    - LSB = 180/2^23
    - DEC = 00000000 00100111 00010000 = 10000
    - Longitude = DEC*LSB = 0.214576721


### F11-23 Geometric Altitude
```java
(byte)0b00100111, //182
(byte)0b00010000, //183
```
- LSB = 6.25 ft
- DEC = 00100111 00010000 = 10000
- altitude = DEC*LSB = 62500 ft


### F11-24 Position Uncertainty
```java
(byte)0b00000001, //184
```
- pun = 00000001


### F11-25 MODE S MB DATA
#### REP
```java
(byte)0b00000011, //185
```
- REP=3

#### DATA-1
```java
(byte)0b00000000, //186
(byte)0b00000000, //187
(byte)0b00000000, //188
(byte)0b00000000, //189
(byte)0b00000000, //190
(byte)0b00000000, //191
(byte)0b00000001, //192
(byte)0b00010001, //193
```
- MB Data = 00000000 00000000 00000000 00000000 00000000 00000000 00000001
- BDS1 = 0001
- BDS2 = 0001


#### DATA-2
```java
(byte)0b00000000, //194
(byte)0b00000000, //195
(byte)0b00000000, //196
(byte)0b00000000, //197
(byte)0b00000000, //198
(byte)0b00000000, //199
(byte)0b00000010, //200
(byte)0b00100010, //201
```
- MB Data = 00000000 00000000 00000000 00000000 00000000 00000000 00000010
- BDS1 = 0010
- BDS2 = 0010


#### DATA-3
```java
(byte)0b00000000, //202
(byte)0b00000000, //203
(byte)0b00000000, //204
(byte)0b00000000, //205
(byte)0b00000000, //206
(byte)0b00000000, //207
(byte)0b00000011, //208
(byte)0b00110011, //209
```
- MB Data = 00000000 00000000 00000000 00000000 00000000 00000000 00000011
- BDS1 = 0011
- BDS2 = 0011


### F11-26 Indicated Airspeed
```java
(byte)0b00100111, //210
(byte)0b00010000, //211
```
- LSB = 1 kt
- DEC = 00100111 00010000 = 10000
- ias = DEC*LSB = 10000 kt


### F11-27 Mach Number
```java
(byte)0b00100111, //212
(byte)0b00010000, //213
```
- LSB = 0.008 mach
- DEC = 00100111 00010000 = 10000
- mach = DEC*LSB = 80 mach


### F11-28 Barometric Pressure Setting (derived from Mode S BDS 4,0)
```java
(byte)0b00000000, //214
(byte)0b00000001, //215
```
- LSB = 0.1 mb
- DEC = 0000 00000001 = 1
- bps = DEC*LSB = 0.1 mb


## F12 Track Number
```java
(byte)0b00000001, //216
(byte)0b00011101, //217
```
- TrackNumber = 00000001 00011101 = 285


## F13 Track Status
### F13-0
```java
(byte)0b00010101, //218
```
- MON = 0 = Multisensor track
- SPI = 0 = default value
- MRH = 0 = Barometric altitude
- SRC = 101 = speed look-up table
- CNF = 0 = Confirmed track


### F13-1
```java
(byte)0b10010001, //219
```
- SIM = 1 = Simulated track
- TSE = 0 = default value
- TSB = 0 = default value
- FPC = 1 = Flight plan correlated
- AFF = 0 = default value
- STP = 0 = default value
- KOS = 0 = Complementary service used
- FX = 1 = Extension into next extent


### F13-2
```java
(byte)0b10100011, //220
```
- AMA = 1 = track resulting from amalgamation process
- MD4 = 01 = Friendly target
- ME = 0 = default value
- MI = 0 = default value
- MD5 = 01 = Friendly target
- FX = 1 = Extension into next extent


### F13-3
```java
(byte)0b00000001, //221
```
- CST = 0 = Default value
- PSR = 0 = Default value
- SSR = 0 = Default value
- MDS = 0 = Default value
- ADS = 0 = Default value
- SUC = 0 = Default value
- AAC = 0 = Default value
- FX = 1 = Extension into next extent


### F13-4
```java
(byte)0b00001001, //222
```
- SDS = 00 = Combined
- EMS = 001 = 1 = General emergency
- PFT = 0 = No indication
- FPLT = 0 = Default value
- FX = 1 = Extension into next extent


### F13-5
```java
(byte)0b00000000, //223
```
- DUPT = 0 = Default value
- DUPF = 0 = Default value
- DUPM = 0 = Default value
- FX = 0 = End of data item


## F14 System Track Update Ages
### F14-0 Primary Subfield
```java
(byte)0b11111111, //224
(byte)0b11100000, //225
```
- TRK = 1 = Presence
- PSR = 1 = Presence
- SSR = 1 = Presence
- MDS = 1 = Presence
- ADS = 1 = Presence
- ES = 1 = Presence
- VDL = 1 = Presence
- FX1 = 1 = extension
- UAT = 1 = Presence
- LOP = 1 = Presence
- MLT = 1 = Presence
- FX2 = 0 = no extension


### F14-1 Track Age
```java
(byte)0b00000001, //226
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- Track Age = DEC*LSB = 0.25 s


### F14-2 PSR Age
```java
(byte)0b00000001, //227
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- PSR Age = DEC*LSB = 0.25 s


### F14-3 SSR Age
```java
(byte)0b00000001, //228
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- SSR Age = DEC*LSB = 0.25 s


### F14-4 Mode S Age
```java
(byte)0b00000001, //229
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- Mode S Age = DEC*LSB = 0.25 s


### F14-5 ADS-C Age
```java
(byte)0b00000000, //230
(byte)0b00000001, //231
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- ADS-C Age = DEC*LSB = 0.25 s


### F14-6 ES Age
```java
(byte)0b00000001, //232
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- ES Age = DEC*LSB = 0.25 s


### F14-7 VDL Age
```java
(byte)0b00000001, //233
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- VDL Age = DEC*LSB = 0.25 s


### F14-8 UAT Age
```java
(byte)0b00000001, //234
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- UAT Age = DEC*LSB = 0.25 s


### F14-9 Loop Age
```java
(byte)0b00000001, //235
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- Loop Age = DEC*LSB = 0.25 s


### F14-10 Multilateration Age
```java
(byte)0b00000001, //236
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- Multilateration Age = DEC*LSB = 0.25 s


## F15 Mode of Movement
```java
(byte)0b00010100, //237
```
- TRANS = 00 = Constant Course
- LONG = 01 = Increasing Groundspeed
- VERT = 01 = Climb
- ADF = 0 = No altitude discrepancy


## F16 Track Data Ages
### F16-0 Primary Subfield
```java
(byte)0b11111111, //238
(byte)0b11111111, //239
(byte)0b11111111, //240
(byte)0b11111111, //241
(byte)0b11100000, //242
```
- MFL = 1 = Presence
- MD1 = 1 = Presence
- MD2 = 1 = Presence
- MDA = 1 = Presence
- MD4 = 1 = Presence
- MD5 = 1 = Presence
- MHG = 1 = Presence
- FX1 = 1 = extension
- IAS = 1 = Presence
- TAS = 1 = Presence
- SAL = 1 = Presence
- FSS = 1 = Presence
- TID = 1 = Presence
- COM = 1 = Presence
- SAB = 1 = Presence
- FX2 = 1 = extension
- ACS = 1 = Presence
- BVR = 1 = Presence
- GVR = 1 = Presence
- RAN = 1 = Presence
- TAR = 1 = Presence
- TAN = 1 = Presence
- GSP = 1 = Presence
- FX3 = 1 = extension
- VUN = 1 = Presence
- MET = 1 = Presence
- EMC = 1 = Presence
- POS = 1 = Presence
- GAL = 1 = Presence
- PUN = 1 = Presence
- MB = 1 = Presence
- FX4 = 1 = extension
- IAR = 1 = Presence
- MAC = 1 = Presence
- BPS = 1 = Presence
- FX5 = 0 = no extension


### F16-1 Measured Flight Level Age
```java
(byte)0b00000001, //243
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MFL = DEC*LSB = 0.25 s


### F16-2 Mode 1 Age
```java
(byte)0b00000001, //244
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MD1 = DEC*LSB = 0.25 s


### F16-3 Mode 2 Age
```java
(byte)0b00000001, //245
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MD2 = DEC*LSB = 0.25 s


### F16-4 Mode 3/A Age
```java
(byte)0b00000001, //246
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MD3 = DEC*LSB = 0.25 s


### F16-5 Mode 4 Age
```java
(byte)0b00000001, //247
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MD4 = DEC*LSB = 0.25 s


### F16-6 Mode 5 Age
```java
(byte)0b00000001, //248
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MD5 = DEC*LSB = 0.25 s


### F16-7 Magnetic Heading Age
```java
(byte)0b00000001, //249
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MHG = DEC*LSB = 0.25 s


### F16-8 Indicated Airspeed / Mach Nb age
```java
(byte)0b00000001, //250
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- IAS = DEC*LSB = 0.25 s


### F16-9 True Airspeed Age
```java
(byte)0b00000001, //251
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- TAS = DEC*LSB = 0.25 s


### F16-10 Selected Altitude Age
```java
(byte)0b00000001, //252
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- SAL = DEC*LSB = 0.25 s


### F16-11 Final State Selected Altitude Age
```java
(byte)0b00000001, //253
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- FSS = DEC*LSB = 0.25 s


### F16-12 Trajectory Intent Age
```java
(byte)0b00000001, //254
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- TID = DEC*LSB = 0.25 s


### F16-13 Capability and Flight Status Age
```java
(byte)0b00000001, //255
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- COM = DEC*LSB = 0.25 s


### F16-14 Status Reported by ADS-B Age
```java
(byte)0b00000001, //256
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- SAB = DEC*LSB = 0.25 s


### F16-15 ACAS Resolution Advisory Report Age
```java
(byte)0b00000001, //257
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- ACS = DEC*LSB = 0.25 s


### F16-16 Barometric Vertical Rate Age
```java
(byte)0b00000001, //258
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- BVR = DEC*LSB = 0.25 s


### F16-17 Geometrical Vertical Rate Age
```java
(byte)0b00000001, //259
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- GVR = DEC*LSB = 0.25 s


### F16-18 Roll Angle Age
```java
(byte)0b00000001, //260
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- RAN = DEC*LSB = 0.25 s


### F16-19 Track Angle Rate Age
```java
(byte)0b00000001, //261
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- TAR = DEC*LSB = 0.25 s


### F16-20 Track Angle Age
```java
(byte)0b00000001, //262
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- TAN = DEC*LSB = 0.25 s


### F16-21 Ground Speed Age
```java
(byte)0b00000001, //263
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- GSP = DEC*LSB = 0.25 s


### F16-22 Velocity Uncertainty Age
```java
(byte)0b00000001, //264
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- VUN = DEC*LSB = 0.25 s


### F16-23 Meteorological Data Age
```java
(byte)0b00000001, //265
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MET = DEC*LSB = 0.25 s


### F16-24 Emitter Category Age
```java
(byte)0b00000001, //266
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- EMC = DEC*LSB = 0.25 s


### F16-25 Position Age
```java
(byte)0b00000001, //267
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- POS = DEC*LSB = 0.25 s


### F16-26 Geometric Altitude Age
```java
(byte)0b00000001, //268
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- GAL = DEC*LSB = 0.25 s


### F16-27 Position Uncertainty Age
```java
(byte)0b00000001, //269
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- PUN = DEC*LSB = 0.25 s


### F16-28 Mode S MB Data Age
```java
(byte)0b00000001, //270
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MB = DEC*LSB = 0.25 s


### F16-29 Indicated Airspeed Data Age
```java
(byte)0b00000001, //271
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- IAR = DEC*LSB = 0.25 s


### F16-30 Mach Number Data Age
```java
(byte)0b00000001, //272
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- MAC = DEC*LSB = 0.25 s


### F16-31 Barometric Pressure Setting Data Age
```java
(byte)0b00000001, //273
```
- LSB = 0.25 s
- DEC = 00000001 = 1
- BPS = DEC*LSB = 0.25 s


## F17 Measured Flight Level
```java
(byte)0b00100111, //274
(byte)0b00010000, //275
```
- LSB = 0.25 FL
- DEC = 00100111 00010000 = 10000
- MFL = DEC*LSB = 2500 FL


## F18 Calculated Track Geometric Altitude
```java
(byte)0b00100111, //276
(byte)0b00010000, //277
```
- LSB = 6.25 ft
- DEC = DEC = 00100111 00010000 = 10000
- Altitude = DEC*LSB = 62500 ft


## F19 Calculated Track Barometric Altitude
```java
(byte)0b10100111, //278
(byte)0b00010000, //279
```
- QNH = 1 = QNH correction applied
- LSB = 25 ft
- DEC = 0100111 00010000 = 10000
- Altitude = DEC*LSB = 250000 ft


## F20 Calculated Rate Of Climb/Descent
```java
(byte)0b00000000, //280
(byte)0b01100100, //281
```
- LSB = 6.25 feet/minute
- DEC = 00000000 01100100 = 100
- Rate = DEC*LSB = 6250 ft/min


## F21 Flight Plan Related Data
### F21-0 Primary Subfield
```java
(byte)0b11111111, //282
(byte)0b11111111, //283
(byte)0b11110000, //284
```
- TAG = 1 = Presence
- CSN = 1 = Presence
- IFI = 1 = Presence
- FCT = 1 = Presence
- TAC = 1 = Presence
- WTC = 1 = Presence
- DEP = 1 = Presence
- FX1 = 1 = extension
- DST = 1 = Presence
- RDS = 1 = Presence
- CFL = 1 = Presence
- CTL = 1 = Presence
- TOD = 1 = Presence
- AST = 1 = Presence
- STS = 1 = Presence
- FX2 = 1 = extension
- STD = 1 = Presence
- STA = 1 = Presence
- PEM = 1 = Presence
- PEC = 1 = Presence
- FX3 = 0 = no extension


### F21-1 FPPS Identification Tag
```java
(byte)0b00000001, //285
(byte)0b00000001, //286
```
- SAC = 00000001
- SIC = 00000001


### F21-2 Callsign
```java
(byte)0b01011010, //287
(byte)0b01011010, //288
(byte)0b01011010, //289
(byte)0b01011010, //290
(byte)0b01011010, //291
(byte)0b01011010, //292
(byte)0b01011010, //293
```
- Characters 1 = 01011010 = 90 > ASCII = 'Z'
- Characters 2 = 01011010 = 90 > ASCII = 'Z'
- Characters 3 = 01011010 = 90 > ASCII = 'Z'
- Characters 4 = 01011010 = 90 > ASCII = 'Z'
- Characters 5 = 01011010 = 90 > ASCII = 'Z'
- Characters 6 = 01011010 = 90 > ASCII = 'Z'
- Characters 7 = 01011010 = 90 > ASCII = 'Z'
- Characters 8 = 01011010 = 90 > ASCII = 'Z'


### F21-3 IFPS_FLIGHT_ID
```java
(byte)0b10000000, //294
(byte)0b00000000, //295
(byte)0b00000000, //296
(byte)0b00000001, //297
```
- TYP = 10 = Unit 2 internal flight number
- NBR = 000 00000000 00000000 00000001 = 1


### F21-4 Flight Category
```java
(byte)0b10000100, //298
```
- GAT/OAT = 10 = Operational Air Traffic
- FR1/FR2 = 00 = Instrument Flight Rules
- RVSM = 01 = Approved
- HPR = 0 = Normal Priority Flight


### F21-5 Type of Aircraft
```java
(byte)0b01011010, //299
(byte)0b01011010, //300
(byte)0b01011010, //301
(byte)0b01011010, //302
```
- Characters 1 = 01011010 = 90 > ASCII = 'Z'
- Characters 2 = 01011010 = 90 > ASCII = 'Z'
- Characters 3 = 01011010 = 90 > ASCII = 'Z'
- Characters 4 = 01011010 = 90 > ASCII = 'Z'


### F21-6 Wake Turbulence Category
```java
(byte)0b01001101, //303
```
- Turbulence = 01001101 = 77 > ASCII = 'M'


### F21-7 Departure Airport
```java
(byte)0b01011010, //304
(byte)0b01011010, //305
(byte)0b01011010, //306
(byte)0b01011010, //307
```
- Characters 1 = 01011010 = 90 > ASCII = 'Z'
- Characters 2 = 01011010 = 90 > ASCII = 'Z'
- Characters 3 = 01011010 = 90 > ASCII = 'Z'
- Characters 4 = 01011010 = 90 > ASCII = 'Z'


### F21-8 Destination Airport
```java
(byte)0b01011010, //308
(byte)0b01011010, //309
(byte)0b01011010, //310
(byte)0b01011010, //311
```
- Characters 1 = 01011010 = 90 > ASCII = 'Z'
- Characters 2 = 01011010 = 90 > ASCII = 'Z'
- Characters 3 = 01011010 = 90 > ASCII = 'Z'
- Characters 4 = 01011010 = 90 > ASCII = 'Z'


### F21-9 Runway Designation
```java
(byte)0b00000001, //312
(byte)0b00000001, //313
(byte)0b01011010, //314
```
- NU1 = 00000001 = 1
- NU2 = 00000001 = 1
- LTR = 01011010 = 90 > ASCII = 'Z'


### F21-10 Current Cleared Flight Level
```java
(byte)0b00000011, //315
(byte)0b11101000, //316
```
- LSB = 0.25 FL
- DEC = 00000011 11101000 = 1000
- CFL = DEC*LSB = 2500 FL


### F21-11 Current Control Position
```java
(byte)0b00000001, //317
(byte)0b00000001, //318
```
- Centre = 00000001
- Position = 00000001


### F21-12 Time of Departure / Arrival
#### REP
```java
(byte)0b00000011, //319
```
- REP = 00000011 = 3


#### DATA-1
```java
(byte)0b00000000, //320
(byte)0b00000110, //321
(byte)0b00000110, //322
(byte)0b00000110, //323
```
- TYP = 00000 = 0 = Scheduled off-block time
- DAY = 00 = Today
- HOR = 00110 = 6
- MIN = 000110 = 6
- AVS = 0 = Seconds available
- SEC = 000110 = 6


#### DATA-2
```java
(byte)0b00001000, //324
(byte)0b00000110, //325
(byte)0b00000110, //326
(byte)0b00000110, //327
```
- TYP = 00001 = 1 = Estimated off-block time
- DAY = 00 = Today
- HOR = 00110 = 6
- MIN = 000110 = 6
- AVS = 0 = Seconds available
- SEC = 000110 = 6


#### DATA-3
```java
(byte)0b00010000, //328
(byte)0b00000110, //329
(byte)0b00000110, //330
(byte)0b00000110, //331
```
- TYP = 00010 = 2 = Estimated take-off time
- DAY = 00 = Today
- HOR = 00110 = 6
- MIN = 000110 = 6
- AVS = 0 = Seconds available
- SEC = 000110 = 6


### F21-13 Aircraft Stand
```java
(byte)0b01011010, //332
(byte)0b01011010, //333
(byte)0b01011010, //334
(byte)0b01011010, //335
(byte)0b01011010, //336
(byte)0b01011010, //337
```
- Characters 1 = 01011010 = 90 > ASCII = 'Z'
- Characters 2 = 01011010 = 90 > ASCII = 'Z'
- Characters 3 = 01011010 = 90 > ASCII = 'Z'
- Characters 4 = 01011010 = 90 > ASCII = 'Z'
- Characters 5 = 01011010 = 90 > ASCII = 'Z'
- Characters 6 = 01011010 = 90 > ASCII = 'Z'


### F21-14 Stand Status
```java
(byte)0b01000000, //338
```
- EMP = 01 = Occupied
- AVL = 00 = Available


### F21-15 Standard Instrument Departure
```java
(byte)0b01011010, //339
(byte)0b01011010, //340
(byte)0b01011010, //341
(byte)0b01011010, //342
(byte)0b01011010, //343
(byte)0b01011010, //344
(byte)0b01011010, //345
```
- Characters 1 = 01011010 = 90 > ASCII = 'Z'
- Characters 2 = 01011010 = 90 > ASCII = 'Z'
- Characters 3 = 01011010 = 90 > ASCII = 'Z'
- Characters 4 = 01011010 = 90 > ASCII = 'Z'
- Characters 5 = 01011010 = 90 > ASCII = 'Z'
- Characters 6 = 01011010 = 90 > ASCII = 'Z'
- Characters 7 = 01011010 = 90 > ASCII = 'Z'


### F21-16 Standard Instrument Arrival
```java
(byte)0b01011010, //346
(byte)0b01011010, //347
(byte)0b01011010, //348
(byte)0b01011010, //349
(byte)0b01011010, //350
(byte)0b01011010, //351
(byte)0b01011010, //352
```
- Characters 1 = 01011010 = 90 > ASCII = 'Z'
- Characters 2 = 01011010 = 90 > ASCII = 'Z'
- Characters 3 = 01011010 = 90 > ASCII = 'Z'
- Characters 4 = 01011010 = 90 > ASCII = 'Z'
- Characters 5 = 01011010 = 90 > ASCII = 'Z'
- Characters 6 = 01011010 = 90 > ASCII = 'Z'
- Characters 7 = 01011010 = 90 > ASCII = 'Z'


### F21-17 Pre-Emergency Mode 3/A
```java
(byte)0b00010000, //353
(byte)0b00000001, //354
```
- VA = 1 = Valid Mode 3/A available
- Mode-3/A reply = 000 000 000 001 = 0001


### F21-18 Pre-Emergency Callsign
```java
(byte)0b01011010, //355
(byte)0b01011010, //356
(byte)0b01011010, //357
(byte)0b01011010, //358
(byte)0b01011010, //359
(byte)0b01011010, //360
(byte)0b01011010, //361
```
- Characters 1 = 01011010 = 90 > ASCII = 'Z'
- Characters 2 = 01011010 = 90 > ASCII = 'Z'
- Characters 3 = 01011010 = 90 > ASCII = 'Z'
- Characters 4 = 01011010 = 90 > ASCII = 'Z'
- Characters 5 = 01011010 = 90 > ASCII = 'Z'
- Characters 6 = 01011010 = 90 > ASCII = 'Z'
- Characters 7 = 01011010 = 90 > ASCII = 'Z'


## F22 Target Size & Orientation
### F22-0 LENGTH
```java
(byte)0b00000011, //362
```
- LSB = 1 m
- DEC = 0000001 = 1
- LENGTH = DEC*LSB = 1 m
- FX = 1 = Extension into first extent


### F22-1 ORIENTATION 
```java
(byte)0b00000011, //363
```
- LSB = 360/128 deg
- DEC = 0000001 = 1
- ORIENTATION = DEC*LSB = 2.8 deg
- FX = 1 = Extension into next extent


### F22-2 WIDTH 
```java
(byte)0b00000010, //364
```
- LSB = 1 m
- DEC = 0000001 = 1
- WIDTH = DEC*LSB = 1 m
- FX = 0 = End of Data Item


## F23 Vehicle Fleet Identification
```java
(byte)0b00000001, //365
```
- VFI = 00000001 = 1 = ATC equipment maintenance


## F24 Mode 5 Data reports & Extended Mode 1 Code
### F24-0 Primary Subfield
```java
(byte)0b11111110, //366
```
- SUM = 1 = Presence
- PMN = 1 = Presence
- POS = 1 = Presence
- GA = 1 = Presence
- EM1 = 1 = Presence
- TOS = 1 = Presence
- XP = 1 = Presence
- FX = 0 = no extension


### F24-1 Mode 5 Summary
```java
(byte)0b11111111, //367
```
- M5 = 1 = Mode 5 interrogation
- ID = 1 = Authenticated Mode 5 ID reply
- DA = 1 = Authenticated Mode 5 Data reply or Report (i.e any valid Mode 5 reply type other than ID)
- M1 = 1 = Mode 1 code from Mode 5 reply
- M2 = 1 = Mode 2 code from Mode 5 reply.
- M3 = 1 = Mode 3 code from Mode 5 reply.
- MC = 1 = Mode C altitude from Mode 5 reply
- X = 1 = X-pulse set to one.


### F24-2 Mode 5 PIN /National Origin/ Mission Code
```java
(byte)0b00000000, //368
(byte)0b00000001, //369
(byte)0b00000001, //370
(byte)0b00000001, //371
```
- PIN = 000000 00000001 = 1
- NAT = 00001
- MIS = 000001


### F24-3 Mode 5 Reported Position
```java
(byte)0b00000000, //372
(byte)0b00000011, //373
(byte)0b11101000, //374
(byte)0b00000000, //375
(byte)0b00000011, //376
(byte)0b11101000, //377
```
- Latitude
    - LSB = 180/2^23
    - DEC = 00000000 00000011 11101000 = 1000
    - Latitude = DEC*LSB = 0.0214576721 度
- Longitude
    - LSB = 180/2^23
    - DEC = 00000000 00000011 11101000 = 1000
    - Longitude = DEC*LSB = 0.0214576721 度


### F24-4 Mode 5 GNSS-derived Altitude
```java
(byte)0b01000011, //378
(byte)0b11101000, //379
```
- RES = 1 = GA reported in 25 ft increments.
- DEC = 000011 11101000 = 1000
- GA = 25000 ft


### F24-5 Extended Mode 1 Code in Octal Representation
```java
(byte)0b00000000, //380
(byte)0b00000001, //381
```
- EM1 = 000 000 000 001 = 1


### F24-6 Time Offset for POS and GA
```java
(byte)0b00000001, //382
```
- LSB = 1/128 s
- DEC = 00000001 = 1
- TOS = DEC*LSB = 0.0078125 s


### F24-7 X Pulse Presence
```java
(byte)0b00011111, //383
```
- X5 = 1 = X-pulse set to one (present).
- XC = 1 = X-pulse set to one (present).
- X3 = 1 = X-pulse set to one (present).
- X2 = 1 = X-pulse set to one (present).
- X1 = 1 = X-pulse set to one (present).


## F25 Track Mode 2 Code
```java
(byte)0b00000000, //384
(byte)0b00000001, //385
```
- M2 = 000 000 000 001 = 0001


## F26 Composed Track Number
```java
(byte)0b00000001, //386
(byte)0b00000000, //387
(byte)0b00000011, //388
(byte)0b00000010, //389
(byte)0b00000000, //390
(byte)0b00000100, //391
```
- SUI = 00000001
- STN = 00000000 0000001 = 1
- FX1 = 1 = extension into next extent
- SUI = 00000010
- STN = 00000000 0000010 = 2
- FX2 = 0 = end of data item


## F27 Estimated Accuracies
### F27-0 Primary Subfield
```java
(byte)0b11111111, //392
(byte)0b10000000, //393
```
- APC = 1 = Presence
- COV = 1 = Presence
- APW = 1 = Presence
- AGA = 1 = Presence
- ABA = 1 = Presence
- ATV = 1 = Presence
- AA = 1 = Presence
- FX1 = 1 = extension
- ARC = 1 = Presence
- FX2 = 0 = no extension


### F27-1 Estimated Accuracy Of Track Position (Cartesian)
```java
(byte)0b00000000, //394
(byte)0b00000001, //395
(byte)0b00000000, //396
(byte)0b00000001, //397
```
- X
    - LSB = 0.5 m
    - DEC = 00000000 00000001 = 1
    - X = DEC*LSB = 0.5 m
- Y
    - LSB = 0.5 m
    - DEC = 00000000 00000001 = 1
    - Y = DEC*LSB = 0.5 m


### F27-2 XY covariance component
```java
(byte)0b00000000, //398
(byte)0b00000001, //399
```
- LSB = 0.5 m
- DEC = 00000000 00000001 = 1
- COV = DEC*LSB = 0.5 m


### F27-3 Estimated Accuracy Of Track Position (WGS-84)
```java
(byte)0b00000000, //400
(byte)0b00000001, //401
(byte)0b00000000, //402
(byte)0b00000001, //403
```
- LAT
    - LSB = 180/2^25 deg
    - DEC = 00000000 00000001 = 1
    - LAT = DEC*LSB = 0.00000536 deg
- LONG
    - LSB = 180/2^25
    - DEC = 00000000 00000001 = 1
    - LONG = DEC*LSB = 0.00000536 deg


### F27-4 Estimated Accuracy Of Calculated Track Geometric Altitude
```java
(byte)0b00000001, //404
```
- LSB = 6.25 ft
- DEC = 00000001 = 1
- AGA = DEC*LSB = 6.25 ft


### F27-5 Estimated Accuracy Of Calculated Track Barometric Altitude
```java
(byte)0b00000001, //405
```
- LSB = 0.25 FL
- DEC = 00000001 = 1
- ABA = DEC*LSB = 0.25 FL


### F27-6 Estimated Accuracy Of Track Velocity (Cartesian)
```java
(byte)0b00000001, //406
(byte)0b00000001, //407
```
- X
    - LSB = 0.25 m/s
    - DEC = 00000001 = 1
    - X = DEC*LSB = 0.25 m/s
- Y
    - LSB = 0.25 m/s
    - DEC = 00000001 = 1
    - Y = DEC*LSB = 0.25 m/s


### F27-7 Estimated Accuracy Of Acceleration (Cartesian)
```java
(byte)0b00000001, //408
(byte)0b00000001, //409
```
- X
    - LSB = 0.25 m/s^2
    - DEC = 00000001 = 1
    - X = DEC*LSB = 0.25 m/s
- Y
    - LSB = 0.25 m/s^2
    - DEC = 00000001 = 1
    - Y = DEC*LSB = 0.25 m/s^2


### F27-8 Estimated Accuracy Of Rate Of Climb/Descent
```java
(byte)0b00000001, //410
```
- LSB = 6.25 ft/min
- DEC = 00000001 = 1
- ARC = DEC*LSB = 6.25 ft/min


## F28 Measured Information
### F28-0 Primary Subfield
```java
(byte)0b11111100, //411
```
- SID = 1 = Presence
- POS = 1 = Presence
- HEI = 1 = Presence
- MDC = 1 = Presence
- MDA = 1 = Presence
- TYP = 1 = Presence
- FX = 0 = no extension


### F28-1 Sensor Identification
```java
(byte)0b00000001, //412
(byte)0b00000001, //413
```
- SAC = 00000001
- SIC = 00000001


### F28-2 Measured Position
```java
(byte)0b00000011, //414
(byte)0b11101000, //415
(byte)0b00000011, //416
(byte)0b11101000, //417
```
- RHO
    - LSB = 1/256 NM
    - DEC = 00000011 11101000 = 1000
    - RHO = DEC*LSB = 3.9 NM
- THETA
    - LSB = 360/2^16 deg
    - DEC = 00000011 11101000 = 1000
    - THETA = DEC*LSB = 5.5 deg


### F28-3 Measured 3-D Height
```java
(byte)0b00000011, //418
(byte)0b11101000, //419
```
- LSB = 25 feet
- DEC = 00000011 11101000 = 1000
- HEIGHT = DEC*LSB = 25000 feet


### F28-4 Last Measured Mode C Code
```java
(byte)0b00000000, //420
(byte)0b01100100, //421
```
- V = 0 = Code validated
- G = 0 = Default
- LSB = 0.25 FL
- DEC = 000000 01100100 = 100
- MC = DEC*LSB = 25 FL


### F28-5 Last Measured Mode 3/A Code
```java
(byte)0b00000000, //422
(byte)0b00000001, //423
```
- V = 0 = Code validated
- G = 0 = Default
- L = 0 = MODE 3/A code as derived from the reply
- M3A = 000 000 000 001 = 0001

### F28-6 Report Type
```java
(byte)0b01111100  //424
```
- TYP = 011 = SSR + PSR detection
- SIM = 1 = Simulated target report
- RAB = 1 = Report from field monitor
- TST = 1 = Test target report

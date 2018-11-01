---
layout: single
classes: wide
title:  "CAT62的全数据译码"
date:   2018-10-31 22:44:20 +0800
categories: cat eurocontrol
---

# CAT
``` java
(byte)0b00111110, //0
```
**V=62，十进制**

---

# LEN
``` java
(byte)0b00000001  //1
(byte)0b10101001  //2
```
**V=425, 十进制**

---

<!--more-->

# FSPEC
``` java
(byte)0b10111111  //3
(byte)0b11111111  //4
(byte)0b11111111  //5
(byte)0b11111111  //6
(byte)0b00000000  //7
```

| F1   | F2   | F3   | F4   | F5   | F6   | F7   | FX   |
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
|1     | 0    | 1    | 1    | 1    | 1    | 1    | 1    |



| F8   | F9   | F10  | F11  | F12  | F13  | F14  | FX   |
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
|1     | 1    | 1    | 1    | 1    | 1    | 1    | 1    | 



| F15  | F16  | F17  | F18  | F19  | F20  | F21  | FX   |
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
|1     | 1    | 1    | 1    | 1    | 1    | 1    | 1    | 



| F22  | F23  | F24  | F25  | F26  | F27  | F28  | FX   |
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| 1    | 1    | 1    | 1    | 1    | 1    | 1    | 1    | 



| F29  | F30  | F31  | F32  | F33  | F34  | F35  | FX   |
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 

---

# Data Fields

## F1 Data Source Identifier
```java
(byte)0b10010000, //8
(byte)0b10010000, //9
```
**V=Turkmenistan, 由于是欧洲标准，所以没有亚洲国家，选择土库曼斯坦，SAC/SIC 查询地址为：[SAC/SIC定义](https://www.eurocontrol.int/services/system-area-code-list)**

## F2 Spare
**预留为空**

## F3 Service Identification
``` java
(byte)0b00000001, //10
```
**系统分配**

## F4 Time Of Track Information
``` java
(byte)0b00101110, //11
(byte)0b10011000, //12
(byte)0b01011001, //13
```

## F5 Calculated Track Position (WGS-84)

``` java
(byte)0b00000000, //14
(byte)0b01001111, //15
(byte)0b01011000, //16
(byte)0b01110000, //17
(byte)0b00000001, //18
(byte)0b00100111, //19
(byte)0b11101111, //20
(byte)0b11101110, //21
```

## F6 Calculated Track Position (Cartesian)

```java
(byte)0b00000000, //22
(byte)0b01000110, //23
(byte)0b11011111, //24
(byte)0b11110110, //25
(byte)0b11101100, //26
(byte)0b01011000, //27
```
## F7 Calculated Track Velocity (Cartesian)

```java
(byte)0b00000001, //28
(byte)0b11010011, //29
(byte)0b00000011, //30
(byte)0b01101111, //31
```


## F8 Calculated Acceleration (Cartesian)

```java
(byte)0b01100100, //32
(byte)0b01100100, //33
```
## F9 Track Mode 3/A Code

```java
(byte)0b00000111, //34
(byte)0b01010011, //35
```
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
## F11 Aircraft Derived Data
### F11-Primary Subfield
```java
(byte)0b11111111, //43
(byte)0b11111111, //44
(byte)0b11111111, //45
(byte)0b11111110, //46        
```
### F11-1 Target Address
``` java
(byte)0b00000000, //47
(byte)0b00000000, //48
(byte)0b00000001, //49
```

### F11-2 Target Identification
``` java
(byte)0b11000011, //50
(byte)0b00001100, //51
(byte)0b00110000, //52
(byte)0b11000011, //53
(byte)0b00001100, //54
(byte)0b00110000, //55
```

### F11-3 Magnetic Heading
```java
(byte)0b00000000, //56
(byte)0b01100100, //57
```

### F11-4 Indicated Airspeed / Mach No
```java
(byte)0b00000011, //58
(byte)0b11101000, //59
```

### F11-5 True Airspeed
```java
(byte)0b00000000, //60
(byte)0b01100100, //61
```

### F11-6 Selected Altitude
``` java
(byte)0b01000000, //62
(byte)0b01100100, //63
```
### F11-7 Final State Selected Altitude
``` java
(byte)0b11100000, //64
(byte)0b01100100, //65
```
### F11-8 Trajectory Intent Status
```java
(byte)0b00000000, //66
```

### F11-9 Trajectory Intent Data
- REP=5
```java
(byte)0b00000101, //67
```

- DATA1
```java
(byte)0b00000001, //68
```
TCA=0 NC=0 TCP Number=1

```java
(byte)0b00000000, //69
(byte)0b01100100, //70
```
Altitude=100

```java
(byte)0b00000000, //71
(byte)0b00100111, //72
(byte)0b00010000, //73
```
Latitude=10000

```java
(byte)0b00000000, //74
(byte)0b00100111, //75
(byte)0b00010000, //76
```
Longitude=10000

```java
(byte)0b00011110, //77
```
Point Type=0001 TD=11 TRA=1 TOA=0

```java
(byte)0b00000000, //78
(byte)0b00100111, //79
(byte)0b00010000, //80
```
TOV=10000

```java
(byte)0b00000000, //81
(byte)0b01100100, //82
```
TTR=1

- DATA2
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

- data3
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

- data4
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
- data5
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
### F11-10 Communications/ACAS Capability and Flight Status reported by Mode-S

```java
(byte)0b00100100, //143
(byte)0b11111111, //144
```

### F11-11 Status reported by ADS-B
```java
(byte)0b10100110, //145
(byte)0b00000001, //146
```
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
### F11-13 Barometric Vertical Rate
```java
(byte)0b00100111, //154
(byte)0b00010000, //155
```

### F11-14 Geometric Vertical Rate
```java
(byte)0b00100111, //156
(byte)0b00010000, //157
```
### F11-15 Roll Angle
```java
(byte)0b00100111, //158
(byte)0b00010000, //159
```
### F11-16 Track Angle Rate
```java
(byte)0b11000000, //160
(byte)0b00000010, //161
```
### F11-17 Track Angle
```java
(byte)0b00100111, //162
(byte)0b00010000, //163
```
### F11-18 Ground Speed
```java
(byte)0b00100111, //164
(byte)0b00010000, //165
```
### F11-19 Velocity Uncertainty
```java
(byte)0b00000001, //166
```
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
### F11-21 Emitter Category
```java
(byte)0b00000001, //175
```
### F11-22 Position
```java
(byte)0b00000000, //176
(byte)0b00100111, //178
(byte)0b00010000, //179
(byte)0b00000000, //180
(byte)0b00100111, //181
(byte)0b00010000, //182
```
### F11-23 Geometric Altitude
```java
(byte)0b00100111, //183
(byte)0b00010000, //184
```
### F11-24 Position Uncertainty
```java
(byte)0b00000001, //185
```
### F11-25 MODE S MB DATA
- rep=3
```java
(byte)0b00000011, //186
```

data1
```java
(byte)0b00000000, //187
(byte)0b00000000, //188
(byte)0b00000000, //189
(byte)0b00000000, //190
(byte)0b00000000, //191
(byte)0b00000000, //192
(byte)0b00000001, //193
(byte)0b00010001, //194
```
data2
```java
(byte)0b00000000, //195
(byte)0b00000000, //196
(byte)0b00000000, //197
(byte)0b00000000, //198
(byte)0b00000000, //199
(byte)0b00000000, //200
(byte)0b00000010, //201
(byte)0b00100010, //202
```

data3
```java
(byte)0b00000000, //203
(byte)0b00000000, //204
(byte)0b00000000, //205
(byte)0b00000000, //206
(byte)0b00000000, //207
(byte)0b00000000, //208
(byte)0b00000011, //209
(byte)0b00110011, //210
```
### F11-26 Indicated Airspeed
```java
(byte)0b00100111, //211
(byte)0b00010000, //212
```
### F11-27 Mach Number
```java
(byte)0b00100111, //213
(byte)0b00010000, //214
```
### F11-28 Barometric Pressure Setting (derived from Mode S BDS 4,0)
```java
(byte)0b00000000, //215
(byte)0b00000001, //216
```
## F12 Track Number
```java
(byte)0b00000001, //217
(byte)0b00011101, //218
```
## F13 Track Status
### F13-1
```java
(byte)0b00010101, //219
```
### F13-2
```java
(byte)0b10100011, //220
```
### F13-3
```java
(byte)0b00000001, //221
```
### F13-4
```java
(byte)0b00011001, //222
```
### F13-5
```java
(byte)0b00000000, //223
```
## F14 System Track Update Ages
### primary subfield
```java
(byte)0b11111111, //224
(byte)0b11100000, //225
```
### F14-1 Track Age
```java
(byte)0b00000001, //226
```
### F14-2 PSR Age
```java
(byte)0b00000001, //227
```
### F14-3 SSR Age
```java
(byte)0b00000001, //228
```
### F14-4 Mode S Age
```java
(byte)0b00000001, //229
```
### F14-5 ADS-C Age
```java
(byte)0b00000000, //230
(byte)0b00000001, //231
```
### F14-6 ES Age
```java
(byte)0b00000001, //232
```
### F14-7 VDL Age
```java
(byte)0b00000001, //233
```
### F14-8 UAT Age
```java
(byte)0b00000001, //234
```
### F14-9 Loop Age
```java
(byte)0b00000001, //235
```
### F14-10 Multilateration Age
```java
(byte)0b00000001, //236
```
## F15 Mode of Movement
```java
(byte)0b00010100, //237
```
## F16 Track Data Ages
### primary subfields
```java
(byte)0b11111111, //238
(byte)0b11111111, //239
(byte)0b11111111, //240
(byte)0b11111111, //241
(byte)0b11100000, //242
```
### F16-1 Measured Flight Level Age
```java
(byte)0b00000001, //243
```
### F16-2 Mode 1 Age
```java
(byte)0b00000001, //244
```
### F16-3 Mode 2 Age
```java
(byte)0b00000001, //245
```
### F16-4 Mode 3/A Age
```java
(byte)0b00000001, //246
```
### F16-5 Mode 4 Age
```java
(byte)0b00000001, //247
```
### F16-6 Mode 5 Age
```java
(byte)0b00000001, //248
```
### F16-7 Magnetic Heading Age
```java
(byte)0b00000001, //249
```
### F16-8 Indicated Airspeed / Mach Nb age
```java
(byte)0b00000001, //250
```
### F16-9 True Airspeed Age
```java
(byte)0b00000001, //251
```
### F16-10 Selected Altitude Age
```java
(byte)0b00000001, //252
```
### F16-11 Final State Selected Altitude Age
```java
(byte)0b00000001, //253
```
### F16-12 Trajectory Intent Age
```java
(byte)0b00000001, //254
```
### F16-13 Capability and Flight Status Age
```java
(byte)0b00000001, //255
```
### F16-14 Status Reported by ADS-B Age
```java
(byte)0b00000001, //256
```
### F16-15 ACAS Resolution Advisory Report Age
```java
(byte)0b00000001, //257
```
### F16-16 Barometric Vertical Rate Age
```java
(byte)0b00000001, //258
```
### F16-17 Geometrical Vertical Rate Age
```java
(byte)0b00000001, //259
```
### F16-18 Roll Angle Age
```java
(byte)0b00000001, //260
```
### F16-19 Track Angle Rate Age
```java
(byte)0b00000001, //261
```
### F16-20 Track Angle Age
```java
(byte)0b00000001, //262
```
### F16-21 Ground Speed Age
```java
(byte)0b00000001, //263
```
### F16-22 Velocity Uncertainty Age
```java
(byte)0b00000001, //264
```
### F16-23 Meteorological Data Age
```java
(byte)0b00000001, //265
```
### F16-24 Emitter Category Age
```java
(byte)0b00000001, //266
```
### F16-25 Position Age
```java
(byte)0b00000001, //267
```
### F16-26 Geometric Altitude Age
```java
(byte)0b00000001, //268
```
### F16-27 Position Uncertainty Age
```java
(byte)0b00000001, //269
```
### F16-28 Mode S MB Data Age
```java
(byte)0b00000001, //270
```
### F16-29 Indicated Airspeed Data Age
```java
(byte)0b00000001, //271
```
### F16-30 Mach Number Data Age
```java
(byte)0b00000001, //272
```
### F16-31 Barometric Pressure Setting Data Age
```java
(byte)0b00000001, //273
```
## F17 Measured Flight Level
```java
(byte)0b00100111, //274
(byte)0b00010000, //275
```
## F18 Calculated Track Geometric Altitude
```java
(byte)0b00100111, //276
(byte)0b00010000, //277
```
## F19 Calculated Track Barometric Altitude
```java
(byte)0b10100111, //278
(byte)0b00010000, //279
```
## F20 Calculated Rate Of Climb/Descent
```java
(byte)0b00000000, //280
(byte)0b01100100, //281
```
## F21 Flight Plan Related Data
### primary subfields
```java
(byte)0b11111111, //282
(byte)0b11111111, //283
(byte)0b11110000, //284
```

### F21-1 FPPS Identification Tag
```java
(byte)0b00000001, //285
(byte)0b00000001, //286
```
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
### F21-3 IFPS_FLIGHT_ID
```java
(byte)0b10000000, //294
(byte)0b00000000, //295
(byte)0b00000000, //296
(byte)0b00000001, //297
```
### F21-4 Flight Category
```java
(byte)0b10000100, //298
```
### F21-5 Type of Aircraft
```java
(byte)0b01011010, //299
(byte)0b01011010, //300
(byte)0b01011010, //301
(byte)0b01011010, //302
```
### F21-6 Wake Turbulence Category
```java
(byte)0b01001101, //303
```
### F21-7 Departure Airport
```java
(byte)0b01011010, //304
(byte)0b01011010, //305
(byte)0b01011010, //306
(byte)0b01011010, //307
```
### F21-8 Destination Airport
```java
(byte)0b01011010, //308
(byte)0b01011010, //309
(byte)0b01011010, //310
(byte)0b01011010, //311
```
### F21-9 Runway Designation
```java
(byte)0b00000001, //312
(byte)0b00000001, //313
(byte)0b01011010, //314
```
### F21-10 Current Cleared Flight Level
```java
(byte)0b00000011, //315
(byte)0b11101000, //316
```
### F21-11 Current Control Position
```java
(byte)0b00000001, //317
(byte)0b00000001, //318
```
### F21-12 Time of Departure / Arrival
- rep=3
```java
(byte)0b00000011, //319
```

data1
```java
(byte)0b00000000, //320
(byte)0b00000110, //321
(byte)0b00000110, //322
(byte)0b00000110, //323
```
data2
```java
(byte)0b00001000, //324
(byte)0b00000110, //325
(byte)0b00000110, //326
(byte)0b00000110, //327
```
data3
```java
(byte)0b00010000, //328
(byte)0b00000110, //329
(byte)0b00000110, //330
(byte)0b00000110, //331
```

### F21-13 Aircraft Stand
```java
(byte)0b01011010, //332
(byte)0b01011010, //333
(byte)0b01011010, //334
(byte)0b01011010, //335
(byte)0b01011010, //336
(byte)0b01011010, //337
```
### F21-14 Stand Status
```java
(byte)0b01000000, //338
```
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

### F21-17 Pre-Emergency Mode 3/A
```java
(byte)0b00010000, //353
(byte)0b00000001, //354
```

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
## F22 Target Size & Orientation
### F22-1 LENGTH
```java
(byte)0b00000011, //362
```
### F22-2 ORIENTATION 
```java
(byte)0b00000011, //363
```

### F22-3 WIDTH 
```java
(byte)0b00000010, //364
```

## F23 Vehicle Fleet Identification
```java
(byte)0b00000001, //365
```
## F24 Mode 5 Data reports & Extended Mode 1 Code
### primary
```java
(byte)0b11111110, //366
``` 
### F24-1 Mode 5 Summary
```java
(byte)0b11111111, //367
```
### F24-2 Mode 5 PIN /National Origin/ Mission Code
```java
(byte)0b00000000, //368
(byte)0b00000001, //369
(byte)0b00000001, //370
(byte)0b00000001, //371
```
### F24-3 Mode 5 Reported Position
```java
(byte)0b00000000, //372
(byte)0b00000011, //373
(byte)0b11101000, //374
(byte)0b00000000, //375
(byte)0b00000011, //376
(byte)0b11101000, //377
```
### F24-4 Mode 5 GNSS-derived Altitude
```java
(byte)0b01000011, //378
(byte)0b11101000, //379
```
### F24-5 Extended Mode 1 Code in Octal Representation
```java
(byte)0b00000000, //380
(byte)0b00000001, //381
```
### F24-6 Time Offset for POS and GA
```java
(byte)0b00000001, //382
```
### F24-7 X Pulse Presence
```java
(byte)0b00011111, //383
```
## F25 Track Mode 2 Code
```java
(byte)0b00000000, //384
(byte)0b00000001, //385
```
## F26 Composed Track Number
```java
(byte)0b00000001, //386
(byte)0b00000000, //387
(byte)0b00000011, //388
(byte)0b00000010, //389
(byte)0b00000000, //390
(byte)0b00000100, //391
```
## F27 Estimated Accuracies
### primary
```java
(byte)0b11111111, //392
(byte)0b10000000, //393
```
### F27-1 Estimated Accuracy Of Track Position (Cartesian)
```java
(byte)0b00000000, //394
(byte)0b00000001, //395
(byte)0b00000000, //396
(byte)0b00000001, //397
```
### F27-2 XY covariance component
```java
(byte)0b00000000, //398
(byte)0b00000001, //399
```
### F27-3 Estimated Accuracy Of Track Position (WGS-84)
```java
(byte)0b00000000, //400
(byte)0b00000001, //401
(byte)0b00000000, //402
(byte)0b00000001, //403
```
### F27-4 Estimated Accuracy Of Calculated Track Geometric Altitude
```java
(byte)0b00000001, //404
```
### F27-5 Estimated Accuracy Of Calculated Track Barometric Altitude
```java
(byte)0b00000001, //405
```
### F27-6 Estimated Accuracy Of Track Velocity (Cartesian)
```java
(byte)0b00000001, //406
(byte)0b00000001, //407
```
### F27-7 Estimated Accuracy Of Acceleration (Cartesian)
```java
(byte)0b00000001, //408
(byte)0b00000001, //409
```
### F27-8 Estimated Accuracy Of Rate Of Climb/Descent
```java
(byte)0b00000001, //410
```
## F28 Measured Information
### primary
```java
(byte)0b11111100, //411
```
### F28-1 Sensor Identification
```java
(byte)0b00000001, //412
(byte)0b00000001, //413
```
### F28-2 Measured Position
```java
(byte)0b00000011, //414
(byte)0b11101000, //415
(byte)0b00000011, //416
(byte)0b11101000, //417
```
### F28-3 Measured 3-D Height
```java
(byte)0b00000011, //418
(byte)0b11101000, //419
```
### F28-4 Last Measured Mode C Code
```java
(byte)0b00000000, //420
(byte)0b01100100, //421
```
### F28-5 Last Measured Mode 3/A Code
```java
(byte)0b00000000, //422
(byte)0b00000001, //423
```
### F28-6 Report Type
```java
(byte)0b01111100  //424
```
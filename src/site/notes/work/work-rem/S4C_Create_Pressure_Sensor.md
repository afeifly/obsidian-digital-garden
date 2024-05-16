---
{"dg-publish":true,"permalink":"/work/work-rem/s4-c-create-pressure-sensor/"}
---


1. Create modbus device
Device type : Pressure sensor
Can select three pressure sensor type: 
- Pressure 16 bar (g)
- Pressure 40 bar (g)
- Pressure 1.6 bar (a)

2. Pressure sensor channel only show  unit & resolution

| Description         | Unit | Resolution |
| ------------------- | ---- | ---------- |
| Pressure 16 bar(g) | bar  | 0.1        | 

3. Can change  unit in table cell with drop List, And resolution will also be changed.

**Description will change as unit change**
For example change Unit : bar -> MPa
Description :  Pressure 1.6 bar (a)  -> Pressure 1.6 MPa (a)

|            | Unit | Resolution |
| ---------- | ---- | ---------- |
| 16 bar (g) | bar  | 0.01       |
| 16 PSI (g) | PSI  | 0.1       |
| 16 MPa (g) | MPa  | 0.001      |


|            | Unit | Resolution |
| ---------- | ---- | ---------- |
| 40 bar (g) | bar  | 0.01       |
| 40 PSI (g) | PSI  | 0.1       |
| 40 MPa (g) | MPa  | 0.001      |

|             | Unit | Resolution |
| ----------- | ---- | ---------- |
| 1.6 bar (a) | bar  | 0.001      |
| 1.6 PSI (a) | PSI  | 0.01       |
| 1.6 MPa (a) | MPa  | 0.0001     |

4. Checkbox for sensor version 
Sensor version:   (Checked to select V2 version, otherwise normal version.)

X - V2 

| Sensor type | Version | X-Low | X-High | Y-Low | Y-High(bar) | Y-High(MPa) | Y-High(PSI)  |
| ----------- | ------- | ----- | ------ | ----- | ----------- | ----------- | ------------ |
| 16          | Normal  | 0     | 2000   | 0     | 16          | 16 x 0.1    | 16 x 14.503     | 
|             | V2      | 0     | 1000   | 0     | 10          | 10 x 0.1    | 10 x 14.503     |
| 40          | Normal  | 0     | 2000   | 0     | 40          | 40 x 0.1    | 40 x 14.503     |
|             | V2      | 0     | 1000   | 0     | 10          | 10 x 0.1    | 10 x 14.503     |
| 1.6         | Normal  | 0     | 2000   | 0     | 1.6         | 1.6 x 0.1   | 1.6 x 14.503 |
|             | V2      | 0     | 1000   | 0     | 10          | 10 x 0.1    | 10 x 14.503     |
Senosr version & Unit will change scaling define, no need show scaling in UI
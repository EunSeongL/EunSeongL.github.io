---
title: "CMOS Inverter"
date: "2025-06-16"
thumbnail: "/assets/img/thumbnail/not_sim.jpg"
---

# CMOS INVERTER
---

**Practice: Simulating the Schematic Symbol of a CMOS Inverter with Synopsys Custom Compiler**

![library](/assets/img/full_custom/library1.png "Library")
* New -> Library

![library](/assets/img/full_custom/library2.png "Library")
* Attributes Name
* Technology Tech Library 

![cellview](/assets/img/full_custom/cellview1.png "cellview")
* File -> New -> CellView

![cellview](/assets/img/full_custom/cellview2.png "cellview")
* Cell Name, View Name:schematic -> OK

![design](/assets/img/full_custom/design1.png "design")
* Options -> Design

![design](/assets/img/full_custom/design2.png "design")

**Editing**
Snap Spacing (X, Y) 부품/배선 배치 격자 간격 설정
Solder Dot Size	    배선 연결점(솔더 점) 크기 설정
Fat Wire Width  	굵은 배선의 선 굵기 설정
Default Net Prefix	자동 생성 신호선 이름의 접두사


![add](/assets/img/full_custom/add.png "add")

<div align="center"><span style="font-size:20px"><strong>Add</strong></span></div>

| I        | W    | L     | P   |
|:--------:|:----:|:-----:|:---:|
| Instance | Wire | Label | Pin |



![cmos_not](/assets/img/full_custom/cmos_not.png "cmos_not")

<div align="center"><span style="font-size:20px"><strong>CMOS Schematic</strong></span></div>

![property](/assets/img/full_custom/property1.png "property")
![property](/assets/img/full_custom/property2.png "property")
*q property

![symbol](/assets/img/full_custom/symbol1.png "symbol")
* Design -> New CellView -> From CellView

![symbol](/assets/img/full_custom/symbol2.png "symbol")
![symbol](/assets/img/full_custom/symbol3.png "symbol")
<div align="center">
<span style="font-size:20px"><strong>Adjust Pins</strong></span>
</div>

|Left|Right|Top|Bottom|
|:-----:|:-----:|:-----:|:-----:|
|VIN|VOUT|VDD|VSS|

![symbol](/assets/img/full_custom/symbol4.png "symbol")
<div align="center">
<span style="font-size:20px"><strong>Not symbol</strong></span>
</div>

![not_test](/assets/img/full_custom/not_test1.png "not_test")
* File -> New -> CellView
![not_test](/assets/img/full_custom/not_test2.png "not_test")
* Cell Name, View Name:schematic -> OK

![instance_not](/assets/img/full_custom/instance_not.png "instance_not")
* Instance Not

![instance_gnd](/assets/img/full_custom/instance_gnd.png "instance_gnd")
* Instance GND

![instance_vdc](/assets/img/full_custom/instance_vdc.png "instance_vdc")
* Instance VDC

![property](/assets/img/full_custom/property3.png "property")
---
* P: property
|VDD|VIN|VSS|

VIN : VIN
VSS : 0

![primewave](/assets/img/full_custom/primewave1.png "primewave1")
* Tools -> PrimeWave

![primewave](/assets/img/full_custom/primewave2.png "primewave2")
* Setup -> Model Files

![primewave](/assets/img/full_custom/primewave3.png "primewave3")
* Click Folder shape 

![primewave](/assets/img/full_custom/primewave4.png "primewave4")
* Select .lib file

![primewave](/assets/img/full_custom/primewave5.png "primewave5")
* Section -> FF

![primewave](/assets/img/full_custom/primewave6.png "primewave6")
* Variables -> Copy from Design

![primewave](/assets/img/full_custom/primewave7.png "primewave7")
* VIN 의 Value 값은 0으로 입력. 
**설정하지 않으면 simulation이 안나올 수 있다**

![primewave](/assets/img/full_custom/primewave8.png "primewave8")
* Simulation -> Options

![primewave](/assets/img/full_custom/primewave9.png "primewave9")
* PrimeSim HSPICE -> OK

![primewave](/assets/img/full_custom/primewave10.png "primewave10")
* Setup -> Analyses

![primewave](/assets/img/full_custom/primewave12.png "primewave12")
* Analysis Type: dc
* DC Analysis -> Design Variable
* Variable Name, Sweep Type 
* Setting Start, Stop, Step Size

![primewave](/assets/img/full_custom/primewave13.png "primewave13")
* Click the blank field under expression
* Click Pick from Design on the right

![primewave](/assets/img/full_custom/primewave14.png "primewave14")
* Simulation -> Netlist and Run

![primewave](/assets/img/full_custom/primewave15.png "primewave15")
* Run

![primewave](/assets/img/full_custom/primewave16.png "primewave16")
**NOT WaveView**

![del_lck](/assets/img/full_custom/del_lck.png "del_lck")
---
**Schematic이 Read전용일때 해결하는 방법**
- 비정상 종료 등으로 인해 lck 파일이 남아있으면, 파일이 계속 사용 중인 것으로 인식되어 접근이 제한된다.
=>*sch.oa.cdslck 삭제하여 해결한다.*


![variable1](/assets/img/full_custom/variable1.png "variable1")
![variable2](/assets/img/full_custom/variable2.png "variable2")
![variable3](/assets/img/full_custom/variable3.png "variable3")
![variable4](/assets/img/full_custom/variable4.png "variable4")

![tools1](/assets/img/full_custom/tools1.png "tools1")
![tools2](/assets/img/full_custom/tools2.png "tools2")
![tools3](/assets/img/full_custom/tools3.png "tools3")
![tools4](/assets/img/full_custom/tools4.png "tools4")

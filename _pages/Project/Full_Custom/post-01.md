---
title: "Day1: CMOS Inverter"
date: "2025-06-16"
thumbnail: "/assets/img/thumbnail/not_sim.jpg"
---

# DAY1
---

**Fill the image path to the 'thumbnail' attribute, if you want a main image to be displayed on the post header image.**

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
---
**Editing**
Snap Spacing (X, Y) 부품/배선 배치 격자 간격 설정
Solder Dot Size	    배선 연결점(솔더 점) 크기 설정
Fat Wire Width  	굵은 배선의 선 굵기 설정
Default Net Prefix	자동 생성 신호선 이름의 접두사


![add](/assets/img/full_custom/add.png "add")
---
**Add**
I  instance  
W  wire  
L  label(wire name)  
P  pin  

![cmos_not](/assets/img/full_custom/cmos_not.png "cmos_not")
**CMOS Schematic**

![property](/assets/img/full_custom/property1.png "property")
![property](/assets/img/full_custom/property2.png "property")
q     property

![symbol](/assets/img/full_custom/symbol1.png "symbol")
* Design -> New CellView -> From CellView

![symbol](/assets/img/full_custom/symbol2.png "symbol")
![symbol](/assets/img/full_custom/symbol3.png "symbol")
---
**Adjust Pins**
Left     VIN
Right    VOUT
Top      VDD
Bottom   VSS

![symbol](/assets/img/full_custom/symbol4.png "symbol")
**NOT Symbol**

![not_test](/assets/img/full_custom/not_test1.png "not_test")
* File -> New -> CellView
![not_test](/assets/img/full_custom/not_test2.png "not_test")
* Cell Name, View Name:schematic -> OK

![instance_not](/assets/img/full_custom/instance_not.png "instance_not")
* Instance Not

![instance_gnd](/assets/img/full_custom/instance_gnd.png "instance_gnd")
* Instance gnd

![instance_vdc](/assets/img/full_custom/instance_vdc.png "instance_vdc")
* Instance vdc

![property](/assets/img/full_custom/property3.png "property")
---
* P: property
VDD : 1
VIN : VIN
VSS : 0

![primewave](/assets/img/full_custom/primewave1.png "primewave1")
* Tools -> PrimeWave

![primewave](/assets/img/full_custom/primewave2.png "primewave2")
*Setup -> Model Files

![primewave](/assets/img/full_custom/primewave3.png "primewave3")
* Click Folder shape 

![primewave](/assets/img/full_custom/primewave4.png "primewave4")
* Select .lib file

![primewave](/assets/img/full_custom/primewave5.png "primewave5")
* Section -> FF

![primewave](/assets/img/full_custom/primewave6.png "primewave6")
* Variables -> Copy from Design

![primewave](/assets/img/full_custom/primewave7.png "primewave7")
* VIN 의 Value 값은 0으로 입력해줘야한다. 
**설정하지 않으면 simulation이 안나올 수 있다**

![primewave](/assets/img/full_custom/primewave8.png "primewave8")
![primewave](/assets/img/full_custom/primewave9.png "primewave9")
* PrimeSim HSPICE -> OK

![primewave](/assets/img/full_custom/primewave10.png "primewave10")
* Setup -> Analyses

![primewave](/assets/img/full_custom/primewave11.png "primewave11")
* Analysis Type: dc
* DC Analysis -> Design Variable
* Variable Name, Sweep Type 
* Setting Start, Stop, Step Size

![primewave](/assets/img/full_custom/primewave12.png "primewave12")
* Click the blank field under expression
* Click Pick from Design on the right

![primewave](/assets/img/full_custom/primewave13.png "primewave13")
* Simulation -> Netlist and Run

![primewave](/assets/img/full_custom/primewave14.png "primewave14")
* Run

![primewave](/assets/img/full_custom/primewave15.png "primewave15")
![primewave](/assets/img/full_custom/primewave16.png "primewave16")
**NOT WaveView**

![del_lck](/assets/img/full_custom/del_lck.png "del_lck")
---
**Schematic이 Read전용일때 해결하는 방법**
- 비정상 종료 등으로 인해 lck 파일이 남아있으면, 파일이 계속 사용 중인 것으로 인식되어 접근이 제한된다.
=>*sch.oa.cdslck 삭제하여 해결한다.*

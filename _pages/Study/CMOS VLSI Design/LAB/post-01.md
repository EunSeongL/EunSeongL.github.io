---
title: "CMOS Inverter"
date: "2025-06-16"
thumbnail: "/assets/img/full_custom/not_sim.png"
---

# CMOS Inverter Simulation with Synopsys Custom Compiler

This guide walks through creating, simulating, and analyzing a CMOS inverter schematic using Synopsys Custom Compiler and PrimeWave.

---

## 1. Library and Cell Setup

**Create a new library:**

1. **New → Library**
2. **Set attributes:**
   - **Name:** Library name
   - **Technology:** Select tech library

![library](/assets/img/full_custom/library1.png "Library")
![library](/assets/img/full_custom/library2.png "Library")

---

**Create a new CellView:**

1. **File → New → CellView**
2. **Configure:**
   - **Cell Name:** Inverter
   - **View Name:** schematic

![cellview](/assets/img/full_custom/cellview1.png "cellview")
![cellview](/assets/img/full_custom/cellview2.png "cellview")

---

**Design Settings:**

**Options → Design → Configure:**
- **Snap Spacing (X,Y):** Grid spacing
- **Solder Dot Size:** Connection point size
- **Fat Wire Width:** Thick wire width
- **Default Net Prefix:** Signal prefix

![design](/assets/img/full_custom/design1.png "design")
![design](/assets/img/full_custom/design2.png "design")

---

## 2. Schematic Design

**Add Components:**

Use the **Add** tool (`I`=Instance, `W`=Wire, `L`=Label, `P`=Pin):

![add](/assets/img/full_custom/add.png "add")

<div align="center"><span style="font-size:20px"><strong>Add</strong></span></div>

| I        | W    | L     | P   |
|:--------:|:----:|:-----:|:---:|
| Instance | Wire | Label | Pin |

---

**CMOS Inverter Schematic:**

1. Place PMOS and NMOS transistors
2. Connect to VDD (top) and VSS (bottom)
3. Add input (VIN) and output (VOUT) pins

![cmos_not](/assets/img/full_custom/cmos_not.png "cmos_not")

<div align="center"><span style="font-size:20px"><strong>CMOS Schematic</strong></span></div>

---

**Component Properties:**

Select components → Press `q` → Configure:
- **Transistor dimensions (W/L)**
- **Net connections**

![property](/assets/img/full_custom/property1.png "property")
![property](/assets/img/full_custom/property2.png "property")

---

## 3. Symbol Creation

**Generate Symbol:**


![symbol](/assets/img/full_custom/symbol1.png "symbol")
![symbol](/assets/img/full_custom/symbol2.png "symbol")
![symbol](/assets/img/full_custom/symbol3.png "symbol")

---

**Pin Arrangement:**

| Position | Pin Name |
|----------|----------|
| Left     | VIN      |
| Right    | VOUT     |
| Top      | VDD      |
| Bottom   | VSS      |

<div align="center">
<span style="font-size:20px"><strong>Adjust Pins</strong></span>
</div>

![symbol](/assets/img/full_custom/symbol4.png "symbol")

---

**Final Inverter Symbol:**

<div align="center">
<span style="font-size:20px"><strong>Not symbol</strong></span>
</div>

---

## 4. Test Schematic Setup

**Create Testbench:**


![not_test](/assets/img/full_custom/not_test1.png "not_test")
![not_test](/assets/img/full_custom/not_test2.png "not_test")

---

**Add Components:**

1. Inverter symbol (Instance)
2. Ground (GND)
3. Voltage source (VDC)

![instance_not](/assets/img/full_custom/instance_not.png "instance_not")
![instance_gnd](/assets/img/full_custom/instance_gnd.png "instance_gnd")
![instance_vdc](/assets/img/full_custom/instance_vdc.png "instance_vdc")

---

**Configure Voltage Sources:**

| Component | Property | Value |
|-----------|----------|-------|
| VDD       | Voltage  | 1.8V  |
| VIN       | Voltage  | DC variable |
| VSS       | Voltage  | 0V    |

![property](/assets/img/full_custom/property3.png "property")

---

## 5. Simulation with PrimeWave

**Launch PrimeWave:**


![primewave](/assets/img/full_custom/primewave1.png "primewave1")

---

**Set Model Files:**


![primewave](/assets/img/full_custom/primewave2.png "primewave2")
![primewave](/assets/img/full_custom/primewave3.png "primewave3")
![primewave](/assets/img/full_custom/primewave4.png "primewave4")

---

**Configure Simulation:**

1. **Model Section:** Select (e.g., FF)
2. **Variables:** `Copy from Design` → Set VIN=0 (설정하지 않으면 simulation이 안나올 수 있다)
3. **Simulation Engine:** PrimeSim HSPICE

![primewave](/assets/img/full_custom/primewave5.png "primewave5")
![primewave](/assets/img/full_custom/primewave6.png "primewave6")
![primewave](/assets/img/full_custom/primewave7.png "primewave7")
![primewave](/assets/img/full_custom/primewave8.png "primewave8")
![primewave](/assets/img/full_custom/primewave9.png "primewave9")

---

**Run Analysis:**

1. **Setup → Analyses**
2. **Analysis Type:** dc
3. **DC Analysis → Design Variable**
4. **Variable Name, Sweep Type:** VIN
5. **Start, Stop, Step Size:** e.g., 0, 1.8, 0.01

![primewave](/assets/img/full_custom/primewave10.png "primewave10")
![primewave](/assets/img/full_custom/primewave12.png "primewave12")
![primewave](/assets/img/full_custom/primewave13.png "primewave13")
![primewave](/assets/img/full_custom/primewave14.png "primewave14")

---

**Run Simulation:**


![primewave](/assets/img/full_custom/primewave15.png "primewave15")

---

**Results:**

**NOT WaveView**

![primewave](/assets/img/full_custom/primewave16.png "primewave16")

---

## 6. Troubleshooting

**Schematic Lock Issue:**

Delete lock file: `*sch.oa.cdslck`

![del_lck](/assets/img/full_custom/del_lck.png "del_lck")

---

**Advanced Configuration:**

- **Variable overrides**
- **Simulator options**

![variable1](/assets/img/full_custom/variable1.png "variable1")
![variable2](/assets/img/full_custom/variable2.png "variable2")
![variable3](/assets/img/full_custom/variable3.png "variable3")
![variable4](/assets/img/full_custom/variable4.png "variable4")

![tools1](/assets/img/full_custom/tools1.png "tools1")
![tools2](/assets/img/full_custom/tools2.png "tools2")
![tools3](/assets/img/full_custom/tools3.png "tools3")
![tools4](/assets/img/full_custom/tools4.png "tools4")

---

## Key Notes

1. **VIN Initialization:** Must be set to 0V for DC sweep
2. **Model Selection:** Use correct technology corner (e.g., FF)
3. **Port Connections:** Verify VDD/VSS connections in test schematic

This structured guide ensures reproducible CMOS inverter simulation with clear visualization at each step.

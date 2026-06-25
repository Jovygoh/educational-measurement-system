# 2-in-1 Educational Measurement Instrument (STM32)

[cite_start]An embedded 2-in-1 diagnostic instrument developed using an **STM32F429 microcontroller** (Atollic TrueSTUDIO framework)[cite: 1, 377, 382]. [cite_start]This system integrates a **Dual-Channel Oscilloscope** and a versatile **Digital Multimeter (DMM)** into a standalone portable unit [cite: 378, 382][cite_start], utilizing an onboard $320\times240$ ILI9341 TFT LCD screen and touchscreen interface for real-time visualization and parameter control[cite: 379, 395].

---

## 👥 Project Contributors & Roles
* [cite_start]**Goh Xuan Hui** — Voltage, Resistor, and Connectivity modules [cite: 3]
* [cite_start]**Wong Carman** — Temperature, Capacitance, and Current modules [cite: 3]
* [cite_start]**Elveis Ng Kai Wei** — Oscillator / Waveform Capture engine & User Interface design [cite: 3]

---

## 🛠️ System Overview & Core Features

### [cite_start]1. Dual-Channel Oscilloscope Mode [cite: 382, 384]
* [cite_start]**Simultaneous Sampling:** Captures real-time analog signals simultaneously via channels `CH1` (Pin `PC0`) and `CH2` (Pin `PC1`)[cite: 115, 393].
* [cite_start]**Adjustable Time Base:** Features 7 selectable time base settings ranging from $5\text{ ms/div}$ up to $1\text{ s/div}$[cite: 394].
* [cite_start]**Real-time Signal Analysis:** Calculates and highlights wave indicators above the grid area, including Peak-to-Peak Voltage ($V_{pp}$), Average Voltage ($V_{avg}$), Frequency, and Duty Cycle[cite: 397, 402].
* [cite_start]**Hardware Storage:** Leverages integrated onboard NAND Flash storage (2KB per page) allowing users to snapshot (`Save`) and reload (`Recall`) waveforms[cite: 115, 398].
* [cite_start]**Color-coded Visualization:** Interactive $6\times4$ division grid rendering Yellow traces for Channel 1 and Cyan traces for Channel 2[cite: 400, 401].

### [cite_start]2. Digital Multimeter (DMM) Mode [cite: 378, 382]
[cite_start]Users can toggle between multiple testing functionalities directly via individual touchscreen button options[cite: 36]:
* [cite_start]**DC Voltage Measurement:** Differential reading ($V_a - V_b$) parsed across `PA0` and `PA1` scaled down dynamically to a precise millivolt (mV) output[cite: 121, 180].
* [cite_start]**Current Measurement:** Measures current variations in milliamps (mA) scaled through reference pins `PA2` and `PA3`[cite: 181, 193].
* [cite_start]**Resistance (Wheatstone Bridge):** Computes unknown resistor profiles ($R_x$) using an active hardware bridge topology configured via ADC channels monitoring pins `PA2` and `PA3`[cite: 257, 258, 259].
* **Capacitance Meter:** Measures capacitance values using the classical **RC Charge/Discharge method** mapping control logic via Pin `PA6` and recording voltage slopes over Pin `PA7`.
* **Temperature Sensor:** Extracts environmental readings from a dedicated external **DHT22** digital sensor array mapped over structural timing sequences.
* **Continuity Tester:** Triggers an automated audio alert via a local active buzzer circuit whenever evaluated terminal resistance drops below $\le 1\ \Omega$.

---

## 🏗️ Technical Architecture & Algorithms
* [cite_start]**Microcontroller:** STM32F429ZIT6 (ARM Cortex-M4) [cite: 377, 382]
* [cite_start]**Sampling Engine:** Hardware Timer `TIM2` triggers dual-interleaved internal ADC channels[cite: 116]. [cite_start]Collected results are piped into memory via **Direct Memory Access (DMA)** to minimize CPU overhead[cite: 117].
* **Display Driver:** Structural layout built using standard `ILI9341` peripheral configurations layered over an `STMPE811` resistive touch driver chip.
* **Storage Interface:** Custom memory indexing via hardware FSMC peripherals linking directly into NAND block pages.

---

## 💻 Environment Setup & Build Instructions

### Prerequisites
* [cite_start]**IDE:** Atollic TrueSTUDIO (v9.3.0 or equivalent STM32CubeIDE ecosystem) [cite: 1]
* **Drivers / Library Stack:** STM32F4xx Standard Peripheral Library (`StdPeriph`) combined with custom `TM` (Tilen Majerle) drivers for Fonts, LCD, and Touchscreen processing.

### Compilation and Deployment
1. Import this repository folder structure directly as an existing C/C++ Eclipse project into Atollic TrueSTUDIO / STM32CubeIDE.
2. Ensure include path references are mapped toward the custom utility layers (`button_back.h`, `dht22.h`, `defines.h`, etc.).
3. Flash the compiled firmware onto your target STM32F429 Discovery development hardware setup using an ST-LINK V2 debugger probe.

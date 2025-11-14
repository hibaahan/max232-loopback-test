# MAX232 Loopback Test – CMOS ↔ RS-232 Breadboard

This repository documents my **breadboard implementation of a MAX232-based loopback test** used to verify correct **CMOS ↔ RS-232 voltage level shifting** and ensure safe UART interfacing for an FPGA (Cyclone) system.

The circuit:
- Converts **CMOS 0–5 V** UART signals to **RS-232 (±10–12 V)**  
- Converts RS-232 signals back to CMOS  
- Demonstrates a **hardware-only loopback test** using an oscilloscope and a function generator

---

## 🧠 Concept

FPGA UART pins use **CMOS logic levels**:

- Logic 0 → 0 V  
- Logic 1 → +5 V (or +3.3 V)

PC serial ports use **RS-232 levels**, which are:
- **Inverted**, and  
- **Much higher amplitude**: +10 V (logic 0), −10 V (logic 1)

To safely connect CMOS and RS-232, the **MAX232** performs:

- **CMOS → RS-232:** `T1IN → T1OUT`  
- **RS-232 → CMOS:** `R1IN → R1OUT`

---

## 🔌 Hardware Used

- MAX232 IC  
- Breadboard  
- 4× charge-pump capacitors (1 µF or 0.1 µF)  
- Function generator  
- Oscilloscope  
- Jumper wires  

---

## 📐 MAX232 – Important Pins

- **Power:** `VCC = +5 V`, `GND`
- **Charge Pump:** `C1+`, `C1−`, `C2+`, `C2−`, `V+`, `V−`
- **Driver (CMOS → RS-232):**
  - `T1IN`  ← CMOS signal  
  - `T1OUT` → RS-232 output  
- **Receiver (RS-232 → CMOS):**
  - `R1IN`  ← RS-232 signal  
  - `R1OUT` → CMOS output  

---

## 🧪 Loopback Test Performed (Hardware Measurement)
This project validates MAX232 functionality **without** using a computer or UART code.  
Instead, the test uses a **square pulse as input** and verifies both conversion directions on the oscilloscope.

### **1. CMOS → RS-232 Conversion (T1IN → T1OUT)**

- A **0–5 V square wave** was applied to **T1IN** using a function generator.  
- The corresponding signal on **T1OUT** showed:
  - Proper **inversion**  
  - **RS-232 amplitude** (~+10 V / −10 V)  
  - Stable high/low voltage levels

This confirms the **charge pump** and line driver stages are functioning.

---

### **2. RS-232 → CMOS Conversion (R1IN → R1OUT)**

The output RS-232 signal was internally looped back through the MAX232:

T1OUT → R1IN → R1OUT

On the oscilloscope:
- **R1OUT** reproduced the original square wave  
- Correct **CMOS voltage range**: 0–5 V  
- Correct **polarity restoration** (inversion reversed)

This verifies the receiver and level-shifting path.

---

## 📸 Photos & Oscilloscope Screenshots

### **Breadboard Setup**

<img width="527" height="614" alt="Circuit_implementation" src="https://github.com/user-attachments/assets/4238f662-6368-44aa-8426-53f9546e6e80" />

---

### **Input Signal — T1IN (CMOS 0–5 V)**

<img width="500" height="235" alt="cmos-signal" src="https://github.com/user-attachments/assets/a54338dd-313f-41a1-b598-b759d3a8a799" />

---

### **RS-232 Output — T1OUT / R1IN**  
(Inversion + higher amplitude, used to form the loopback)

![IMG_2425](https://github.com/user-attachments/assets/093a949c-71fa-4560-b102-71bed5edef83)


---

### **Loopback Output — R1OUT (Converted Back to CMOS)**

<img width="965" height="702" alt="Loop_Bac_signal_in_R1_out" src="https://github.com/user-attachments/assets/99b40cc6-62de-4042-9b15-9144136fa56d" />


---

## ✔️ Conclusion

The MAX232 successfully reproduced at **R1OUT** the same waveform injected at **T1IN**, confirming:

- CMOS → RS-232 conversion works  
- RS-232 → CMOS conversion works  
- The internal loopback path is correct  
- The device produces safe voltage levels suitable for FPGA UART pins  

This verifies full MAX232 functionality **without** requiring a PC, UART code, or serial terminal.

---

  

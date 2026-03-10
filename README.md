# Embedded Feed-Forward & PI Control System Validation

**Role:** Embedded Software developer, Test & Validation Intern  
**Company:** Chemnitz Power Labs GmbH  
**Timeline:** [Oct 2025 - Jan 2026]  

## 📌 Project Overview
Developed and rigorously validated an embedded control system for a high-performance power electronic Gate Driver. The project required precise regulation of Gate-Source Voltages from 0–25V and Measurement Currents up to 50mA. 

The core challenge was to design a system capable of handling dynamic load changes while enforcing strict safety constraints using an STM32 microcontroller and a CMSIS-RTOS multi-threaded environment.

## 🛠️ Tech Stack & Tools
*   **Languages:** Embedded C 
*   **Microcontroller:** STM32 (STM32CubeIDE) 
*   **RTOS:** CMSIS-RTOS (Concurrent task management for State Machine, CAN, and ADC) 
*   **Testing & Validation:** Oscilloscopes, Digital Multimeters, Electronic Loads, Statistical Data Analysis
*   **Protocols:** CAN Bus, SPI (for external 24-bit ADC)

---

## 🏗️ System Architecture
The system utilizes a "Two-Stage Control" philosophy to balance rapid response with high-precision steady-state performance:

1.  **Feed-Forward (FFWD) Stage:** Utilizes pre-characterized Look-Up Tables (LUTs) mapped from hardware measurements to set a fast, predictive baseline PWM duty cycle.
2.  **PI Feedback Loop:** A discrete-time Proportional-Integral (PI) controller using the Backward Euler method to correct residual error and reject dynamic disturbances.

### 🛡️ Layered Embedded Safety
To protect the power electronics, several safety mechanisms were integrated directly into the control logic:
*   **Deliberate State Management:** Strict separation of `REQ_STATE` (requested via CAN) and `CUR_STATE`. Setpoints are only promoted and applied after passing rigorous boundary validation.
*   **Slew Rate Limiting:** Capped the maximum allowable PWM duty cycle change per tick to prevent hardware stress.
*   **Anti-Windup Protection:** Implemented conditional integral accumulation. The integrator freezes when the actuator reaches saturation limits, allowing for rapid recovery from large disturbances.

---

## 🧪 Validation, Testing & Debugging
A major focus of this role was hardware-level testing and defect resolution to ensure reliable performance across all operating modes.

*   **Statistical Accuracy Validation:** Conducted exhaustive testing of the PI controller across the full 0–25V range. Performed statistical analysis on tens of thousands of samples, confirming the system maintained a target accuracy of (x.0x format) at critical high-voltage setpoints.
*   **Ziegler-Nichols & Empirical Tuning:** Systematically derived initial K_p and K_i gains using the Ziegler-Nichols frequency response method, followed by empirical refinement using an oscilloscope to minimize overshoot and settling time.
*   **Defect Resolution (Integer Math Refactoring):** 
    *   *Issue:* Discovered severe control loop oscillations in the low-voltage range (0–12V) during dynamic testing.
    *   *Root Cause:* Identified floating-point arithmetic precision limitations on the microcontroller.
    *   *Solution:* Refactored the core PI calculation engine to utilize scaled integer arithmetic. This restored numerical determinism and entirely eliminated the oscillations.

## 📈 Key Achievements
*   Successfully regulated 3 distinct voltage rails and a multi-source current topology.
*   Achieved a highly responsive rise time of ~1.5 ms with negligible overshoot (< 0.8%).
*   Integrated real-time parameter configuration via CAN bus messaging.
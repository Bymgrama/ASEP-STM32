# ASEP-STM32: Adaptive Safety-Embedded Programming

## 📁 Project Structure
- **Data\_Analysis/**: GNUPlot scripts, 2D and 3D simulation results, block diagrams, and flowcharts.
- **Firmware/**: Keil uVision project source code (Core, Drivers) and `.ioc` configuration file.
- **Report/**: Full LaTeX report (`main.tex`) and PDF detailing the synthesis of 15 reference journals.
- **Simulation/**: Proteus 8 simulation files (`.rar`).

## 📖 Project Description
ASEP-STM32 is a novel lightweight bare-metal framework for STM32F401VE microcontrollers that integrates Adaptive PID-lite control with hardware-level safety mechanisms. Developed as a synthesis of 15 recent academic papers, it addresses the need for real-time determinism, ultra-low latency, and embedded anomaly protection (safety shutdown) without the heavy memory overhead of RTOS or Machine Learning algorithms.

## 🏗️ Flowchart & Architecture
The system utilizes continuous polling of environmental sensors via ADC. It calculates real-time variance to determine the operating mode:
- **Normal Mode:** Steady state, optimal PID control (50% PWM).
- **Warning Mode:** Mild fluctuations, adaptive PID parameters applied to compensate.
- **Critical Mode:** Extreme anomalies trigger an immediate Hardware-Level Safety Shutdown (0% PWM) to protect the actuator.

![Architecture Diagram](Data_Analysis/Diagram%20blokk.jpg)
*(Note: See `Data_Analysis/Flowchart fixx.jpg` for the full software logic sequence)*

## 🚀 How to Run the Simulation
1. **Firmware Preparation:** Open `Firmware/ASEP_F401.ioc` via STM32CubeMX, or directly open the Keil uVision project in `Firmware/MDK-ARM`. Press `F7` to Rebuild All and generate the `.hex` file.
2. **Proteus 8 Simulation:** Extract the `.rar` file in the `Simulation/` folder and open the Proteus schematic. Double-click the STM32F401VE component and load the compiled `.hex` file.
3. **Execution:** Press Play. Use RV1 (Potentiometer) to simulate sensor variance. Monitor the UART telemetry via Virtual Terminal and the PWM response via Oscilloscope.

## 📊 Simulation Results & Analysis
The simulation data (voltage, adaptive PWM duty cycle, and variance) were modeled and plotted using **GNUPlot**.

### 3D Control Surface Response
![3D Response](Data_Analysis/grafik_3D_ASEP.png)
*The GNUPlot results clearly demonstrate the system's ability to maintain a flat 50% PWM during low variance, adapt dynamically during mild fluctuations, and instantly drop to 0% PWM when the variance hits the critical 85% threshold, validating the pseudo non-blocking architecture.*

## 💡 Advantages of ASEP-STM32
- **Ultra-Lightweight Memory:** Consumes `< 2KB RAM` compared to heavy TinyML or FreeRTOS alternatives.
- **Zero-Jitter Execution:** O(1) Matrix lookup and direct register manipulation eliminate nested if-else delays.
- **Hardware-Level Hard-Stop:** Autonomous critical shutdown mechanism that responds in microseconds before network latency delays can occur.
- **Agnostic & Modular:** Built natively on STM32 HAL C/C++, making it easily portable across the STM32 ecosystem.
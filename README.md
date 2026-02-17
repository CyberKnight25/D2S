

# Hardware-Accelerated KNN Classifier on FPGA 🚀

A real-time, hardware-accelerated **K-Nearest Neighbors (KNN)** classification engine implemented entirely in Verilog on the **Xilinx ZedBoard**. This project features a custom-built SPI driver for OLED visualization and a pipelined distance calculation core.

## 🎯 Key Features

* **Hardware Acceleration:** Parallel processing of distance calculations and sorting logic.
* **Custom OLED Driver:** Light-weight, state-machine-driven SPI interface for SSD1306 displays.
* **Interactive Interface:** Uses ZedBoard switches and buttons for real-time user input.
* **Configurable K:** Supports dynamic switching between  and  neighbors.
* **Low Latency:** Classification completes in **32 clock cycles** (approx. 320ns @ 100MHz).

## 🛠 Hardware & Tools

* **Board:** Avnet ZedBoard (Zynq-7000 SoC)
* **Display:** 128x64 OLED (SSD1306 controller) via SPI (Pmod Port JE/JD)
* **Language:** Verilog HDL
* **Synthesis:** Xilinx Vivado

## 🕹 Control Scheme (The "Golden Sequence")

| Button | Name | Function |
| --- | --- | --- |
| **BTNR (Right)** | `RESET` | **System Reset** (Press first!) |
| **BTNC (Center)** | `LOAD` | **Load Data** (Step-through: X  Y  K) |
| **BTNL (Left)** | `START` | **Run Classification** |
| **Switches [7:0]** | `DATA` | 8-bit Input Data Selection |

### How to Demo

1. **Reset:** Press **BTNR**. The OLED initializes.
2. **Input X:** Set Switches 0-7. Press **BTNC**. (OLED: `Input X`)
3. **Input Y:** Set Switches 0-7. Press **BTNC**. (OLED: `Input Y`)
4. **Input K:** Set Switches 0-2 (e.g., 3 or 5). Press **BTNC**. (OLED: `K`)
5. **Compute:** Press **BTNL**.
* **Result:** OLED displays `Class = 0` or `Class = 1`.
* **LED0:** Lights up if Class is 1.
* **LED1:** Lights up to signal "Done".



## 🏗 Architecture

The system is composed of five main modules:

1. **`knn_core`**: The top-level wrapper managing data flow.
2. **`dist_cal`**: Manhattan Distance calculator ().
3. **`kclass`**: A sorting network that maintains the top  nearest neighbors in real-time.
4. **`vote`**: Majority voting logic to determine the final class.
5. **`oled_simple`**: A compact FSM-based SPI driver to render text without a frame buffer.

## 🔌 Pin Mapping (ZedBoard)

* **OLED_DC:** `U10`
* **OLED_RES:** `U9`
* **OLED_SCLK:** `AB12`
* **OLED_SDIN:** `AA12`
* **OLED_VDD/VBAT:** `U12`, `U11`

## 🚀 Future Improvements

* Support for higher dimensions (Z-axis).
* DMA integration for processing larger datasets stored in DRAM.
* VGA output for a heatmap visualization of the decision boundary.

---

*Built for the [Hackathon Name] 2025.*

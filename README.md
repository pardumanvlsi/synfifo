## 🚀 Synchronous FIFO in Verilog

This project demonstrates the **Synchronous FIFO (First-In-First-Out)** buffer implemented in Verilog with a 64-location depth and 8-bit data width. The design includes a complete testbench to validate the functionality.

---

### 📚 **Features:**
✅ 64x8 FIFO Buffer  
✅ Full and Empty Detection  
✅ Read/Write Pointer Management  
✅ Overflow and Underflow Handling  

---

### 📄 **Design Overview:**
- **FIFO Module:**  
  - `wr_ptr` and `rd_ptr` manage write and read operations.
  - `fifo_counter` tracks the number of valid entries.
  - Full and empty conditions are monitored to prevent overflow/underflow.

- **Testbench (tb_FIFO):**  
  - Writes multiple values into FIFO.
  - Reads values and checks underflow/overflow scenarios.
  - Validates FIFO depth limit (64 entries).

---

### 📂 **Files:**
- `FIFO.v` - Synchronous FIFO Design  
- `tb_FIFO.v` - Testbench for Verification  
- `dump.vcd` - Waveform File for Debugging  

---

### 🧪 **Test Cases Covered:**
1. ✅ **Write Multiple Values:** Verify writing 10 values correctly.  
2. ✅ **Read Multiple Values:** Check if 10 values are read correctly.  
3. ✅ **Underflow Test:** Attempt to read from an empty FIFO.  
4. ✅ **Overflow Test:** Write beyond FIFO capacity and verify full condition.

---

### 📊 **Waveform Analysis:**
Run the simulation and analyze the waveform in tools like **GTKWave** to inspect read/write behavior, pointer movement, and FIFO conditions.

---

### 📝 **How to Run:**
1. Open the project in your preferred Verilog simulation tool.  
2. Compile and run the testbench (`tb_FIFO.v`).  
3. View the waveform using `dump.vcd` for analysis.

---

### 🔗 **Contribute:**
Feel free to fork, modify, and contribute! 💡  
Report any issues or improvements via GitHub.

---

💻 **Author:** Parduman Gupta  
🎯 **Goal:** Enhance understanding of synchronous FIFO buffers for VLSI applications.

Happy Coding! 🎉

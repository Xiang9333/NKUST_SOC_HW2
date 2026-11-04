# NKUST_SOC_HW2
# <h2>1.
Board Support Packages (BSP) 頁數：第 26 頁  
Interrupt Service Routine (ISR) 頁數：第 261 頁

# <h2>2. 查找Zynq-7020 SoC技術手冊ug585-Zynq-7000-TRM
1.中斷來源與對應 ID（IRQ ID 32～95，共 64 個 SPI）：  
* Core / System 周邊：例如 OCM（On-Chip Memory）錯誤、L2 Cache 中斷、DMA 控制器中斷（DMAC）、System Watchdog 等。  
* 通訊介面周邊：例如 UART (0/1)、I2C (0/1)、SPI (0/1)、CAN (0/1)、SDIO (0/1)、Gigabit Ethernet (0/1)、USB (0/1) 等控制器的中斷。  
* 定時器與計數器：TTC (Triple Timer Counter)、WDT (Watchdog Timer)。  
* PL 邏輯端中斷 (PL-to-PS Interrupts)：包含由 PL 發送給 PS 的 16 個中斷訊號（Fabric Interrupts，分配於 IRQ ID 61-68 及 84-91）。  
<img width="682" height="599" alt="image" src="https://github.com/user-attachments/assets/8d733adf-b9a4-43c5-a813-129862f96773" />
<img width="676" height="448" alt="image" src="https://github.com/user-attachments/assets/d6b44466-9453-473c-804c-aaa56aa6fc48" />

2. ICDISR0 ～ ICDISR2 (Interrupt Security Registers)  
* 功能說明：中斷安全性設定暫存器。  
* 主要用途：定義每個中斷訊號（IRQ ID 0~95）屬於 Secure (S) 還是 Non-secure (NS) 狀態（配合 TrustZone 安全機制使用）。  
3. ICDISER0 ～ ICDISER2 (Interrupt Set-Enable Registers)  
* 功能說明：中斷致能設定暫存器。  
* 主要用途：寫入 `1` 可以開啟（Enable）特定中斷。  
4. ICDICER0 ～ ICDICER2 (Interrupt Clear-Enable Registers)  
* 功能說明：中斷停用/清除設定暫存器。  
* 主要用途：寫入 `1` 可以關閉/禁用（Disable）**特定中斷。  
5. ICDISPR0 ～ ICDISPR2 (Interrupt Set-Pending Registers)  
* 功能說明：強製掛起（Set-Pending）設定暫存器。  
* 主要用途：寫入 `1` 可以手動將特定中斷設為 Pending（掛起/等待處理）狀態（常用於軟體測試或模擬中斷）。  
<img width="633" height="93" alt="image" src="https://github.com/user-attachments/assets/b3e628e1-4155-4438-9e3d-0f05f7a9e8a8" />
<img width="634" height="412" alt="image" src="https://github.com/user-attachments/assets/aa364e47-c922-4175-96c2-c48db2ef3dd5" /><br>
6. ICDICPR0 ～ ICDICPR2 (Interrupt Clear-Pending Registers)  
* 功能說明：清除掛起（Clear-Pending）設定暫存器。  
* 主要用途：寫入 `1` 可以**清除特定中斷的 Pending 狀態。  
7. ICDABR0 ～ ICDABR2 (Active Bit Registers)  
* 功能說明：中斷活躍狀態暫存器（Active Status）。  
* 主要用途：唯讀（Read-Only）。表示對應的中斷目前是否正在由 CPU 核心處理中（已進入 Interrupt Service Routine, ISR）。  
8. ICDIPR0 ～ ICDIPR23 (Interrupt Priority Registers)  
* 功能說明：中斷優先權暫存器。  
* 主要用途：設定每個中斷的優先級大小（Priority Value）。數值越小，代表該中斷的優先權越高（Zynq-7000 支持多級優先權分類與搶佔）。  
9. ICDIPTR0 ～ ICDIPTR23 (Interrupt Processor Targets Registers)  
* 功能說明：中斷目標處理器暫存器。  
* 主要用途：設定每個 SPI（Shared Peripheral Interrupt）中斷要發送給哪一個 CPU 核心（CPU0、CPU1 或同時發送給兩者）進行處理。  
10. ICDICFR0 ～ ICDICFR5 (Interrupt Configuration Registers)  
* 功能說明：中斷觸發型態組態暫存器。  
* 主要用途：為每個中斷定義其觸發型態（Sensitivity）與模型（Handling Model）：  
  - 電位觸發（Level-sensitive） 還是 邊緣觸發（Edge-triggered）。  
  - 1-N 模型（只需任一 Target CPU 處理）還是 N-N 模型（所有指定的 Target CPU 均需獨立處理）。  
<img width="629" height="386" alt="image" src="https://github.com/user-attachments/assets/5b22c33d-32b6-4884-81c5-7d7c42381274" />
<img width="632" height="861" alt="image" src="https://github.com/user-attachments/assets/e9dad413-0f86-4581-b77a-fb0f5b62a90c" />
<img width="630" height="479" alt="image" src="https://github.com/user-attachments/assets/b9064c9e-5521-458a-943b-c507147a08d1" />
<img width="662" height="138" alt="image" src="https://github.com/user-attachments/assets/e974fe0b-2365-4c91-8e34-d195ddc9369b" />
<img width="666" height="687" alt="image" src="https://github.com/user-attachments/assets/0ffbb057-3667-4529-9976-9f8a8ee4a74a" />

<h2>3. 實作
<img width="1053" height="534" alt="image" src="https://github.com/user-attachments/assets/41c0ae68-55db-4785-9a39-08219d9a28f9" />


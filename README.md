# dfu-stm32
Device Update Firmware using STM32 F4-Discovery. Bootloader and Application.

 ┌─────────────────────────┐
 │     Power On / Reset    │
 └────────────┬────────────┘
              │
              ▼
 ┌─────────────────────────┐
 │   Check Boot Mode Pin   │
 │   (Bootloader or App)   │
 └────────────┬────────────┘
      Yes ───▶│ Bootloader │◀── No
              ▼
 ┌─────────────────────────┐
 │   Initialize UART Port  │
 │  (Set baud rate, etc.)  │
 └────────────┬────────────┘
              │
              ▼
 ┌─────────────────────────┐
 │ Wait for Host Command   │
 │   (DFU start signal)    │
 └────────────┬────────────┘
      No ────▶│ Timeout?    │───▶ Jump to Application
              │
      Yes     ▼
 ┌─────────────────────────┐
 │ Send ACK to Host        │
 │  (Ready for Update)     │
 └────────────┬────────────┘
              │
              ▼
 ┌─────────────────────────┐
 │ Receive Firmware Packet │
 │  via UART (Hex/Bin)     │
 └────────────┬────────────┘
              │
              ▼
 ┌─────────────────────────┐
 │ Verify Packet Checksum  │
 └────────────┬────────────┘
       OK ───▶│ Write to Flash│
              ▼
 ┌─────────────────────────┐
 │  Send ACK/NACK to Host  │
 └────────────┬────────────┘
              │
              ▼
 ┌─────────────────────────┐
 │ More Data?              │
 └────────────┬────────────┘
      Yes ───▶│ Receive Next │
              │ Packet       │
      No      ▼
 ┌─────────────────────────┐
 │ Verify Entire Firmware  │
 │   (CRC / Signature)     │
 └────────────┬────────────┘
              │
              ▼
 ┌─────────────────────────┐
 │ Send DFU Complete ACK   │
 │   to Host               │
 └────────────┬────────────┘
              │
              ▼
 ┌─────────────────────────┐
 │ Jump to New Application │
 └─────────────────────────┘

# STM32 FreeRTOS Control & Telemetry System

Multi-threaded embedded C application using FreeRTOS (CMSIS-RTOS v2) on STM32. Demonstrates peripheral drivers, inter-task communication, and thread-safe data handling.

## System Architecture

* **Encoder Handling:** Hardware quadrature encoder decoding using TIM1.
* **Analog Output:** DAC generation scaled from encoder values.
* **Thread Safety:** Mutex synchronization for shared global states.
* **Inter-Task Communication:** Non-blocking message queues between threads.
* **Telemetry:** UART output via DMA with redirected standard `printf`.

## Task Breakdown

| Task | Priority | Description |
| :--- | :--- | :--- |
| `encoderTask` | Above Normal | Polls TIM1 counter and posts data to `encoderQueue`. |
| `dacTask` | Normal | Reads queue, updates DAC output, writes to shared state under mutex. |
| `ledTask` | Low | Reads shared state under mutex and toggles status LED. |
| `uartTask` | Below Normal | Reads shared state under mutex and outputs telemetry via UART. |

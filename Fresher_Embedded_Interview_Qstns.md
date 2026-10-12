# AUMOVIO Fresher Embedded Software Interview Questionnaire

---

## MODULE 1: Embedded C
### 40 Questions

---

**C Fundamentals**

**1.** What is the difference between `int`, `short`, `long`, and `char` in C? Why does the size of `int` vary across platforms?

**2.** What is the output of the following code?
```c
int x = 5;
printf("%d %d %d", x++, x, ++x);
```
What does this illustrate about sequence points?

**3.** What is the difference between `while`, `do-while`, and `for` loops? Give an embedded scenario where `do-while` is preferred.

**4.** What is the difference between `break` and `continue`? Write a loop that skips even numbers and breaks at 15.

**5.** What is a function prototype? Why is it important in embedded C where files are split across multiple translation units?

**6.** What is the difference between `=`, `==`, and `===`? What bug does confusing `=` and `==` cause in an `if` condition?

---

**Arrays, Strings, Pointers**

**7.** What is the difference between an array and a pointer in C? Is `arr[i]` the same as `*(arr + i)`?

**8.** Write a function to reverse a string in-place without using any library function.

**9.** Write a function to find the length of a string without using `strlen()`.

**10.** Write a function to find the largest element in an integer array. Handle the case where the array has only one element.

**11.** What is pointer arithmetic? If `int *p` points to address `0x1000` on a 32-bit system, what is `p + 2`?

**12.** What is a null pointer? What happens when you dereference one? How do you guard against it in embedded code?

**13.** What is the difference between `char *str = "hello"` and `char str[] = "hello"`? Which one can you modify?

**14.** What is a dangling pointer? Give an example and explain how to avoid it.

---

**Structures, Unions, Enums, Typedef**

**15.** What is the difference between a `struct` and a `union`? Give an embedded example where a union is useful for register overlays.

**16.** What is structure padding? What is the size of this struct and why?
```c
struct S {
    char a;
    int b;
    char c;
};
```

**17.** What is `typedef`? Rewrite this declaration using typedef:
```c
struct CANFrame { uint8_t id; uint8_t data[8]; uint8_t dlc; };
```

**18.** What is an `enum`? Write an enum for the states of a traffic light and use it in a switch-case.

---

**Storage Classes**

**19.** What is the difference between a `static` local variable and a regular local variable? Write a function that counts how many times it has been called using a static variable.

**20.** What does `extern` mean? When would you use it in a multi-file embedded project?

**21.** What is the difference between a global variable and a local variable in terms of lifetime, scope, and memory location?

**22.** What does declaring a function as `static` in a `.c` file do? Why is this good practice in embedded projects?

---

**`const` and `volatile`**

**23.** What does `volatile` mean in C? Why is it critical in embedded systems? Give two examples: one for a hardware register and one for an ISR-shared variable.

**24.** What does `const` mean? What is the difference between:
```c
const int *p;
int * const p;
const int * const p;
```

**25.** Can a variable be both `const` and `volatile`? Give an embedded example where this combination makes sense.

---

**Bitwise Operations**

**26.** Write macros to **set**, **clear**, **toggle**, and **check** a specific bit *n* in a register variable `reg`.

**27.** A GPIO control register is at address `0x40020000`. Bit 5 controls an LED. Write the C statements to:
- Enable the LED (set bit 5)
- Disable the LED (clear bit 5)
- Toggle the LED

**28.** Count the number of set bits (population count) in an unsigned integer without using any library. Write the function.

**29.** What is the output?
```c
uint8_t x = 0xAB;
printf("0x%X", (x >> 4) & 0x0F);
```
What does this operation extract?

**30.** A register value is `0x3C` (binary: `0011 1100`). Extract bits 3–5 as a 3-bit value. Show the formula and result.

---

**Memory Basics**

**31.** Describe the memory sections of a typical embedded C program: `.text`, `.data`, `.bss`, stack, and heap. What is stored in each?

**32.** What is the stack? What causes a stack overflow in an embedded system, and how do you detect it?

**33.** What is the difference between static and dynamic memory allocation? Why is `malloc()` generally avoided in safety-critical embedded systems?

**34.** What does `sizeof()` return for:
```c
int arr[10];
int *p = arr;
```
Why are the two results different?

---

**Preprocessor, Macros, Headers**

**35.** What is the difference between `#define` and `const`? When should you prefer one over the other in embedded C?

**36.** What is an include guard? Write one for a header file named `can_driver.h`. Why is it necessary?

**37.** What is the danger in this macro and how do you fix it?
```c
#define SQUARE(x) x * x
```
Test with `SQUARE(2 + 3)`.

---

**Types, Overflow, Casting**

**38.** What is integer overflow? What happens when a `uint8_t` variable holding `255` is incremented by 1? What about a signed `int8_t` holding `127`?

**39.** What is implicit type conversion (promotion)? What is the result and type of:
```c
uint8_t a = 200;
uint8_t b = 100;
uint16_t c = a + b;
```
Is there a risk here?

---

**Function Pointers and Coding**

**40.** What is a function pointer? Write a function pointer declaration for a function that takes an `int` and returns `void`. Show how to assign it and call it. Give an embedded use case (e.g., a callback for a UART receive handler).

---

## MODULE 2: Microcontrollers and Embedded Systems
### 60 Questions (Q41–Q100)


## A. Microcontroller Fundamentals

---

**Q41. Microcontroller vs Microprocessor**

A **microprocessor** is a CPU-only chip — it needs external RAM, Flash, and peripherals to function. A **microcontroller (MCU)** integrates CPU, Flash, RAM, and peripherals (GPIO, timers, ADC, UART, etc.) on a single chip.

In automotive applications, MCUs are preferred because they are self-contained, cost-effective, low-power, and designed for real-time control — exactly what an ECU needs. A microprocessor would require an entire PCB of support chips to do the same job.

**Key terms:** SoC (System on Chip), ECU, peripherals, real-time control.

*Follow-up: Can a microcontroller run Linux? Why or why not?*

---

**Q42. Flash, SRAM, and EEPROM**

| Memory | Type | What is stored |
|---|---|---|
| **Flash** | Non-volatile | Program code (`.text`), constants (`.rodata`), initial values of globals |
| **SRAM** | Volatile | Variables (`.data`, `.bss`), stack, heap — lost on power off |
| **EEPROM** | Non-volatile | Configuration data, calibration values, odometer — needs to survive resets |

Flash is fast for reads but cannot be written byte-by-byte at runtime without special erase cycles. EEPROM allows individual byte writes, making it ideal for persistent settings.

*Follow-up: Why can't you just store everything in Flash and avoid SRAM?*

---

**Q43. GPIO — Input, Output, Floating Input**

**GPIO (General Purpose Input/Output)** pins are the MCU's digital I/O.

- **Output mode:** MCU drives the pin HIGH or LOW — used to turn on LEDs, control transistors, send signals.
- **Input mode:** MCU reads the external voltage on the pin — used for buttons, sensors.

A **floating input** is an input pin with no pull-up or pull-down resistor and nothing driving it. The pin voltage is undefined — it can randomly read 0 or 1 due to noise. This causes unpredictable behavior (e.g., a button that appears to be pressed continuously).

**Fix:** Always enable an internal pull-up or pull-down, or add an external resistor.

*Follow-up: What is open-drain output mode and when is it used?*

---

**Q44. Memory-Mapped Registers**

In ARM Cortex-M MCUs, every peripheral register is mapped to a fixed address in the CPU's address space. You access them by casting the address to a pointer.

```c
// Define the register address as a volatile pointer
#define GPIOA_ODR  (*(volatile uint32_t *)0x40020014)

// Set pin 5 HIGH
GPIOA_ODR |= (1 << 5);

// Clear pin 5
GPIOA_ODR &= ~(1 << 5);
```

`volatile` is mandatory — it tells the compiler not to optimize away repeated reads/writes because the hardware can change the value independently.

*Follow-up: What happens if you forget `volatile` on a peripheral register pointer?*

---

**Q45. Clock Source and Prescaler**

The **clock source** drives every operation inside the MCU — instruction fetch, peripheral timing, communication baud rates. Common sources: internal RC oscillator, external crystal (HSE), PLL.

A **prescaler** divides the clock frequency before it reaches a peripheral, slowing it down.

**Calculation:**
```
Peripheral clock = System clock / Prescaler
                 = 16 MHz / 8
                 = 2 MHz
```

Getting the clock frequency wrong is one of the most common causes of incorrect baud rates and timer periods in embedded projects.

*Follow-up: What is a PLL and why is it used in MCUs?*

---

**Q46. Reset Sources**

A **reset** forces the MCU back to its initial state — program counter goes to the reset vector, registers are cleared.

Three common reset sources:

| Reset Type | Cause |
|---|---|
| **Power-on Reset (POR)** | Supply voltage rises from 0 V at startup |
| **Watchdog Reset** | Firmware failed to kick the watchdog in time — indicates a hang |
| **External Reset** | NRST pin pulled low by an external signal or debug tool |

Others include brown-out reset (supply drops below threshold) and software reset (firmware triggers it intentionally).

*Follow-up: How does firmware detect which reset source just occurred?*

---

**Q47. Datasheet and Register Maps**

A **datasheet** is the hardware reference document for a chip. For peripheral initialization (e.g., UART), you look for:

- **Register map:** base address and offset of every register (e.g., `USART_CR1` at offset `0x00`)
- **Bit field descriptions:** which bits control baud rate, data length, parity, enable
- **Initialization sequence:** the order in which registers must be written
- **Electrical characteristics:** voltage levels, max current, timing constraints

Not reading the datasheet is the number-one cause of peripheral initialization bugs.

*Follow-up: What is the difference between a datasheet and a reference manual for an MCU?*

---

## B. Timers and PWM

---

**Q48. Timer/Counter — Timer Mode vs Counter Mode**

A **timer** counts clock pulses from the MCU's internal clock to measure time intervals, generate delays, or trigger interrupts.

A **counter** counts external events on a pin (e.g., pulses from a rotary encoder or a tachometer signal).

Internally the hardware is the same — a register that increments each tick. The difference is the clock source: internal for timer mode, external pin for counter mode.

*Follow-up: What is input capture mode in a timer?*

---

**Q49. Prescaler in Timers**

The prescaler divides the timer input clock, reducing how fast the counter increments.

**Calculation:**
```
Timer tick frequency = Timer clock / Prescaler
                     = 8 MHz / 64
                     = 125 kHz

Each tick = 1 / 125,000 = 8 µs
```

Without a prescaler, an 8 MHz clock would overflow a 16-bit counter in only ~8 ms, which is too fast for many applications. The prescaler lets you trade resolution for range.

*Follow-up: If you need a 1-second delay, how would you set up the prescaler and ARR together?*

---

**Q50. Timer Overflow Frequency — Calculation**

**Given:** Timer clock = 1 MHz, ARR = 999

**Formula:**
```
Overflow frequency = Timer clock / (ARR + 1)
                   = 1,000,000 / (999 + 1)
                   = 1,000,000 / 1,000
                   = 1,000 Hz  →  1 kHz

Period = 1 / 1,000 Hz = 1 ms
```

The `+1` accounts for the count from 0 to ARR inclusive (that is 1000 ticks total). Forgetting the `+1` is a classic off-by-one error.

*Follow-up: How would you use this 1 ms overflow to implement a `delay_ms()` function?*

---

**Q51. Timer Overflow ISR with Millisecond Tick**

```c
volatile uint32_t ms_tick = 0;

// Called every 1 ms by the timer overflow interrupt
void TIM2_IRQHandler(void) {
    if (TIM2->SR & TIM_SR_UIF) {   // Check update interrupt flag
        ms_tick++;
        TIM2->SR &= ~TIM_SR_UIF;   // Clear the flag
    }
}

// Usage in main loop
uint32_t start = ms_tick;
while ((ms_tick - start) < 500);  // Wait 500 ms
```

**Key points:**
- `volatile` is mandatory — the compiler must not cache `ms_tick` in a register.
- Always **clear the interrupt flag** inside the ISR, or the interrupt fires again immediately.
- Use subtraction (`ms_tick - start`) instead of comparison to handle rollover safely.

*Follow-up: Why is `uint32_t` better than `uint16_t` for a millisecond counter?*

---

**Q52. PWM — Frequency, Duty Cycle, Calculation**

**PWM (Pulse Width Modulation)** is a digital signal that switches between HIGH and LOW at a fixed frequency. The **duty cycle** controls the proportion of time the signal is HIGH.

```
Duty cycle = (ON time / Period) × 100%
           = (5 ms / 20 ms) × 100%
           = 25%
```

A 25% duty cycle delivers 25% of the supply voltage on average to the load — useful for dimming an LED to 25% brightness or running a motor at 25% speed.

*Follow-up: What is the difference between PWM frequency and duty cycle? Which one affects motor noise?*

---

**Q53. PWM ARR Calculation for 1 kHz**

**Given:** System clock = 16 MHz, Prescaler = 16, PWM frequency = 1 kHz

**Step 1 — Timer clock after prescaler:**
```
Timer clock = 16 MHz / 16 = 1 MHz
```

**Step 2 — ARR:**
```
ARR = (Timer clock / PWM frequency) − 1
    = (1,000,000 / 1,000) − 1
    = 1,000 − 1
    = 999
```

Set `ARR = 999`. To achieve a 50% duty cycle, set the compare register (CCR) to `499`.

*Follow-up: How do you change duty cycle dynamically without stopping the timer?*

---

**Q54. Compare Register vs Auto-Reload Register**

| Register | Role |
|---|---|
| **ARR (Auto-Reload Register)** | Sets the PWM period — determines frequency |
| **CCR (Capture/Compare Register)** | Sets the pulse width — determines duty cycle |

The counter counts from 0 to ARR. The output is HIGH while the counter is below CCR, and LOW from CCR to ARR (or vice versa depending on polarity).

To change duty cycle at runtime: simply write a new value to CCR. No need to stop the timer. This is how a motor controller smoothly ramps speed up and down.

*Follow-up: What is center-aligned PWM mode and when is it used?*

---

**Q55. Automotive PWM Examples**

1. **Electric fan / blower motor speed control** — The ECU sends a PWM signal to the motor driver. Increasing duty cycle increases fan speed proportionally.
2. **LED daytime running light (DRL) brightness control** — Headlight ECU adjusts PWM duty cycle to dim or brighten DRLs based on ambient light or driver setting.

Other examples: fuel injector pulse width control, electronic throttle actuator, seat heater power control.

---

## C. ADC

---

**Q56. ADC Resolution**

An **ADC (Analog-to-Digital Converter)** converts a continuous analog voltage into a discrete digital number.

**Resolution** is the number of bits in the digital output. It determines how finely the input voltage range is divided.

```
Number of levels = 2^n = 2^10 = 1,024 levels (for a 10-bit ADC)
```

The smallest voltage change the ADC can detect (1 LSB) is:
```
1 LSB = V_ref / (2^n − 1) = 3.3 V / 1023 ≈ 3.2 mV
```

*Follow-up: What is the advantage of a 12-bit ADC over a 10-bit ADC?*

---

**Q57. ADC Voltage Calculation**

**Given:** 10-bit ADC, V_ref = 3.3 V, ADC reading = 512

**Formula:**
```
V_in = (ADC_value / (2^n − 1)) × V_ref
     = (512 / 1023) × 3.3 V
     = 0.5005 × 3.3
     ≈ 1.65 V
```

This is almost exactly half of 3.3 V, which makes sense — 512 is approximately half of 1023.

*Follow-up: What happens if the input voltage exceeds V_ref?*

---

**Q58. Sampling Rate and Nyquist Criterion**

**Sampling rate** is how many times per second the ADC takes a measurement (samples/second or Hz).

**Nyquist criterion:** The sampling rate must be at least **twice** the highest frequency component in the input signal to reconstruct it without aliasing.

```
f_sample ≥ 2 × f_signal_max
```

**Example:** An automotive knock sensor signal contains frequencies up to 10 kHz. You must sample at ≥ 20 kHz, or high-frequency content will fold back into the data as phantom low-frequency signals (aliasing).

*Follow-up: What is an anti-aliasing filter and where is it placed?*

---

**Q59. ADC Noise Reduction Techniques**

ADC noise comes from power supply fluctuations, PCB layout, and the analog input signal itself.

Two common software techniques:

1. **Averaging:** Take N samples and compute the mean.
   ```c
   uint32_t sum = 0;
   for (int i = 0; i < 16; i++) sum += read_adc();
   uint16_t result = sum / 16;
   ```

2. **Oversampling and decimation:** Sample at a higher rate than needed, then average down — effectively increases resolution at the cost of sampling speed.

Hardware techniques include decoupling capacitors on VREF, a separate analog ground plane, and an RC low-pass filter on the input.

*Follow-up: What is the trade-off of using a very large averaging window (e.g., N = 256)?*

---

**Q60. Temperature Conversion from ADC Reading**

**Given:** 12-bit ADC, V_ref = 3.3 V, sensor range = 0–3.3 V → 0–100 °C

```c
float adc_to_temperature(uint16_t adc_raw) {
    // Step 1: Convert ADC count to voltage
    float voltage = ((float)adc_raw / 4095.0f) * 3.3f;

    // Step 2: Convert voltage to temperature (linear: 0V=0°C, 3.3V=100°C)
    float temperature = (voltage / 3.3f) * 100.0f;

    return temperature;
}
```

Simplified single expression: `temp = (adc_raw / 4095.0f) * 100.0f`

**Edge cases:** Clamp the result to [0, 100] to handle out-of-range inputs gracefully.

*Follow-up: What if the sensor's transfer function is nonlinear? How would you handle it?*

---

## D. Watchdog Timer

---

**Q61. What is a Watchdog Timer?**

A **watchdog timer (WDT)** is a hardware countdown timer. Firmware must periodically reset it (called "kicking" or "petting" the watchdog). If the firmware fails to reset it before timeout — because of a hang, infinite loop, or hard fault — the watchdog automatically resets the MCU.

In automotive systems, an ECU that freezes could leave a vehicle in a dangerous state (throttle stuck open, ABS inactive). The watchdog ensures the system always recovers, which is a core requirement of ISO 26262 functional safety.

*Follow-up: Is it ever acceptable to kick the watchdog inside an interrupt? Why is that dangerous?*

---

**Q62. Hardware vs Software Watchdog**

| | Hardware Watchdog | Software Watchdog |
|---|---|---|
| **Implemented in** | Dedicated hardware timer in MCU | Software counter in firmware |
| **Survives firmware hang** | Yes — hardware keeps running | No — if CPU hangs, software stops too |
| **Reliability** | Higher | Lower |
| **Use case** | Safety-critical reset guarantee | Additional application-level timeout monitoring |

**Hardware watchdog is more reliable.** A software watchdog is implemented by the same CPU that can hang, so it cannot rescue the system from a hard fault or infinite loop. Always use hardware watchdog for safety guarantees.

*Follow-up: What is a windowed watchdog, and how is it stricter than a regular watchdog?*

---

**Q63. Kicking the Watchdog**

"Kicking" means writing a specific value to the watchdog's service register to reset its countdown to zero before it reaches the timeout.

```c
// Example: STM32 IWDG kick
IWDG->KR = 0xAAAA;  // Reload the watchdog counter
```

If the firmware hangs and the watchdog never gets kicked, the counter reaches zero and the WDT fires a system reset. The key discipline: kick the watchdog **only** from a known-good point in the main loop — never from an interrupt or a random subroutine, or you may mask a real hang.

*Follow-up: Where in the main loop is the best place to kick the watchdog?*

---

**Q64. Watchdog Timeout Analysis**

**Scenario:** Watchdog timeout = 100 ms, main loop = 80 ms.

This is **borderline safe** in normal operation — the loop completes in 80 ms, leaving 20 ms margin. However:

- If an **interrupt handler blocks for 50 ms**, the total time before the loop can kick the watchdog becomes 80 + 50 = 130 ms, **which exceeds the 100 ms timeout**. The watchdog will reset the system.

**Best practice:** Keep interrupt handlers very short (microseconds, not milliseconds). Set the watchdog timeout to at least 2–3× the worst-case loop time, and never do blocking work in ISRs.

*Follow-up: How would you restructure a long interrupt handler to avoid this problem?*

---

**Q65. Watchdog Reset vs Power-On Reset Detection**

Different reset sources set different flags in the MCU's reset-cause register (e.g., `RCC_CSR` on STM32).

```c
void check_reset_cause(void) {
    if (RCC->CSR & RCC_CSR_IWDGRSTF) {
        // Watchdog reset — firmware hung somewhere
        log_error(ERR_WATCHDOG_RESET);
    } else if (RCC->CSR & RCC_CSR_PORRSTF) {
        // Normal power-on reset
    }
    RCC->CSR |= RCC_CSR_RMVF;  // Clear all reset flags
}
```

Detecting a watchdog reset is valuable in production: it tells you the firmware experienced an unexpected hang, which you can log to EEPROM for later diagnosis.

*Follow-up: Where in firmware startup should you check the reset cause?*

---

## E. Communication Protocols

### CAN

---

**Q66. What is CAN Bus?**

**CAN (Controller Area Network)** was developed by Bosch in the 1980s specifically for automotive use. Before CAN, every ECU was connected to every other ECU with dedicated point-to-point wiring — a modern vehicle with 100+ ECUs would have required kilometres of wire, making the harness impossibly heavy and expensive.

CAN solves this with a single two-wire **shared bus**: all ECUs connect to the same CANH/CANL pair and communicate by broadcasting messages. Any ECU on the bus can receive any message. This dramatically reduces wiring, weight, and cost.

*Follow-up: CAN is a broadcast bus — how does an ECU know which messages are intended for it?*

---

**Q67. CANH, CANL, Dominant and Recessive States**

CAN uses **differential signaling** on two wires: CANH and CANL.

| State | CANH voltage | CANL voltage | Differential (CANH − CANL) |
|---|---|---|---|
| **Dominant (logic 0)** | ~3.5 V | ~1.5 V | ~2 V |
| **Recessive (logic 1)** | ~2.5 V | ~2.5 V | ~0 V |

The dominant state actively drives the bus; the recessive state is passive (both lines float to ~2.5 V via termination resistors). This is the foundation of CAN arbitration: any node can force the bus dominant, but no single node can force it recessive.

*Follow-up: Why does CAN use differential signaling instead of a single wire?*

---

**Q68. CAN Controller vs CAN Transceiver**

| Component | Role | Location |
|---|---|---|
| **CAN controller** | Implements the CAN protocol — framing, arbitration, error detection, bit stuffing | Inside the MCU (or as a separate IC connected via SPI) |
| **CAN transceiver** | Converts the controller's digital TX/RX signals to/from the differential CANH/CANL bus voltages | External IC between MCU and the bus (e.g., MCP2551, TJA1050) |

The controller handles the logic; the transceiver handles the electrical interface. You always need both.

*Follow-up: What happens if a CAN transceiver is missing and you connect the MCU's TX pin directly to CANH?*

---

**Q69. CAN Standard Data Frame Structure**

| Field | Bits | Purpose |
|---|---|---|
| **SOF** (Start of Frame) | 1 | Marks beginning of frame (dominant bit) |
| **Arbitration field** | 11 (std) | Message ID — determines priority |
| **RTR** | 1 | Remote Transmission Request (0 = data frame) |
| **IDE** | 1 | Identifier Extension (0 = standard 11-bit) |
| **r0** | 1 | Reserved |
| **DLC** | 4 | Data Length Code — number of data bytes (0–8) |
| **Data field** | 0–64 | Actual payload (0–8 bytes) |
| **CRC** | 15 + delimiter | Error detection |
| **ACK** | 2 | Receiver acknowledges receipt |
| **EOF** | 7 | End of frame (recessive bits) |

*Follow-up: What is an RTR frame and when is it used?*

---

**Q70. Standard (11-bit) vs Extended (29-bit) CAN ID**

| | Standard CAN | Extended CAN |
|---|---|---|
| **ID bits** | 11 bits | 29 bits |
| **Max unique IDs** | 2,048 | 536,870,912 |
| **IDE bit** | 0 | 1 |
| **Frame size** | Smaller | Slightly larger |

Standard IDs (11-bit) are sufficient for most in-vehicle networks (CAN has at most a few hundred messages on one bus). Extended IDs (29-bit) are used in applications needing globally unique identifiers — e.g., SAE J1939 (trucks and heavy equipment), OBD-II diagnostic frames, and large industrial networks.

*Follow-up: In J1939, how is the 29-bit ID structured?*

---

**Q71. CAN Arbitration**

CAN arbitration is **non-destructive** and happens bit-by-bit during the arbitration field (ID transmission).

**Rule:** A node that transmits a recessive bit (1) but reads a dominant bit (0) on the bus immediately stops transmitting and becomes a receiver. The node sending the dominant bit wins and continues.

**Result:** Lower ID = more dominant bits = **higher priority**.

- Node A: ID `0x100` = `000 0001 0000 0` in binary
- Node B: ID `0x200` = `000 0010 0000 0` in binary

Comparing bit 9: Node A drives dominant (0), Node B drives recessive (0 vs 1). Node B detects the collision, backs off. **Node A wins** (lower ID = higher priority).

*Follow-up: What happens to Node B after it loses arbitration? Does it retry?*

---

**Q72. Bit Stuffing**

After five consecutive bits of the same polarity (five 1s or five 0s), CAN automatically inserts a **stuff bit** of the opposite polarity. The receiver removes these stuff bits.

**Why:** CAN uses NRZ encoding with no separate clock line. The receiver recovers the clock from bit transitions. If a long sequence of identical bits occurs, there are no transitions and the receiver loses synchronization. Bit stuffing guarantees a transition at least every 6 bits, keeping the receiver in sync.

**Side effect:** Bit stuffing slightly increases frame size, which is why the exact bit count of a real frame is larger than the nominal count.

*Follow-up: Is the CRC field subject to bit stuffing?*

---

**Q73. CAN ACK Field**

The **ACK field** (2 bits: ACK slot + ACK delimiter) confirms that at least one node received the frame without error.

- The **transmitter** sends the ACK slot as recessive (1).
- Any **receiver** that successfully received the frame overwrites the ACK slot with a dominant bit (0) during transmission.
- If the transmitter reads dominant in the ACK slot, it knows at least one node acknowledged.
- If it reads recessive (no one acknowledged), it generates an ACK error.

This means **the receivers write the ACK**, not the transmitter. This is a unique feature of CAN — acknowledgment happens on the fly, in real time, during the same frame.

*Follow-up: What happens if a node is alone on the bus and no other node can send the ACK?*

---

**Q74. CAN Termination Resistors**

CAN bus requires **120 Ω termination resistors** at **both ends** of the bus cable (not in the middle, not at every node).

**Why:** The differential CAN bus behaves as a transmission line at high bit rates. Without termination, signals reflect off the ends of the cable, causing voltage spikes and corrupting bits. The 120 Ω resistors absorb the signal energy and prevent reflections.

In a properly terminated CAN bus, measuring resistance across CANH–CANL with all nodes powered off should read approximately **60 Ω** (two 120 Ω resistors in parallel).

*Follow-up: What symptoms do you see on an oscilloscope if termination resistors are missing?*

---

**Q75. CAN Bit Time and Frame Duration**

**Bit duration at 500 kbps:**
```
t_bit = 1 / 500,000 = 2 µs per bit
```

**Frame duration (standard frame, 8 bytes data, ignoring bit stuffing):**
```
Total bits = SOF(1) + ID(11) + Control(6) + Data(64) + CRC(16) + ACK(2) + EOF(7)
           = 107 bits

Frame time = 107 × 2 µs = 214 µs ≈ 0.214 ms
```

In practice, bit stuffing adds up to ~20% more bits in the worst case, making real frame transmission slightly longer. At 500 kbps, roughly 4,000–5,000 standard frames can fit in one second.

*Follow-up: How does CAN FD reduce transmission time for the same 8 bytes?*

---

**Q76. CAN FD**

**CAN FD (Flexible Data-rate)** extends classical CAN in two ways:

1. **Higher data-phase bit rate:** Up to 8 Mbps for the data field (vs 1 Mbps max for classical CAN), while keeping the arbitration phase at the standard rate.
2. **Larger payload:** Up to **64 bytes** per frame (vs 8 bytes in classical CAN).

These two improvements together make CAN FD up to ~30× faster for large payloads — important for modern automotive features like OTA firmware updates, camera data, and advanced ADAS sensor fusion.

CAN FD is backward-compatible in the sense that a CAN FD controller can participate on a classical CAN network in compatibility mode.

*Follow-up: Can a classical CAN node and a CAN FD node share the same bus?*

---

**Q77. CAN Error States**

A CAN node maintains two counters: **TEC (Transmit Error Counter)** and **REC (Receive Error Counter)**.

| State | Condition | Behavior |
|---|---|---|
| **Error Active** | TEC and REC both < 128 | Normal operation; sends active error flags (6 dominant bits) |
| **Error Passive** | TEC or REC ≥ 128 | Still communicates but sends passive error flags (6 recessive bits); reduced ability to disrupt the bus |
| **Bus Off** | TEC ≥ 256 | Node disconnects from bus completely; requires 128 × 11 recessive bits (or a software reset) to recover |

**Transition trigger:** Each transmission error increments TEC by 8; each successful transmission decrements it by 1. A faulty node gradually accumulates errors and moves toward Bus Off, protecting the rest of the network.

*Follow-up: What are the six types of CAN errors?*

---

**Q78. Missing CAN Messages — Diagnosis**

A node stops receiving expected CAN messages. Four possible causes and diagnostic steps:

| Cause | Diagnostic step |
|---|---|
| **Broken wire / connector** on CANH or CANL | Measure continuity with multimeter; inspect connector pins for corrosion or damage |
| **Missing or wrong termination resistor** | Measure CANH–CANL resistance with bus unpowered; expect ~60 Ω |
| **Transmitting node is in Bus Off state** | Read the transmitting node's error counters via OBD or JTAG; look for Bus Off flag in CAN status register |
| **Baud rate mismatch** between nodes | Connect oscilloscope; measure actual bit time and compare to expected; check clock configuration in both nodes |

*Follow-up: How would you use a logic analyzer or CAN sniffer to diagnose this problem?*

---

### UART

---

**Q79. UART — Baud Rate, Frame Format, "8N1"**

**UART (Universal Asynchronous Receiver/Transmitter)** is a simple two-wire (TX, RX) serial protocol with no shared clock — sender and receiver must be pre-configured to the same baud rate.

| Field | Purpose |
|---|---|
| **Baud rate** | Bits per second (e.g., 9600, 115200) — must match on both ends |
| **Start bit** | 1 low bit — tells the receiver a frame is starting |
| **Data bits** | Usually 8 bits of payload |
| **Parity bit** | Optional error detection (even/odd/none) |
| **Stop bit(s)** | 1 or 2 high bits — marks end of frame |

**"8N1"** = **8** data bits, **N**o parity, **1** stop bit. This is the most common UART configuration in embedded systems.

*Follow-up: If the baud rate is 9600, how long does it take to transmit one byte in 8N1 format?*

---

**Q80. Baud Rate Mismatch**

When one side transmits at 9600 baud and the other receives at 115200 baud, the receiver is sampling ~12× too fast. Each bit is sampled at the wrong time, producing completely garbage data — random bytes, framing errors, and parity errors (if enabled). The UART receive interrupt fires with corrupted data.

This is one of the most common UART bugs. Always verify both sides use the same baud rate, and double-check that the MCU's peripheral clock is what you think it is — a wrong clock configuration changes the actual baud rate even if the register value looks correct.

*Follow-up: How do you calculate the baud rate register (BRR) value from the peripheral clock?*

---

**Q81. UART vs USART**

| | UART | USART |
|---|---|---|
| **Full name** | Universal Asynchronous Receiver/Transmitter | Universal Synchronous/Asynchronous Receiver/Transmitter |
| **Clock line** | No — asynchronous only | Optional CK pin for synchronous mode |
| **Synchronous mode** | Not supported | Supported — receiver uses the transmitter's clock |

USART is a superset of UART. When USART is used in asynchronous mode (no CK pin), it behaves identically to a UART. Most modern MCU datasheets list the peripheral as "USART" but the synchronous mode is rarely used.

*Follow-up: What is IrDA mode on a USART?*

---

### I2C

---

**Q82. I2C — SDA, SCL, Pull-up Resistors**

**I2C (Inter-Integrated Circuit)** is a two-wire, half-duplex, synchronous serial bus supporting multiple controllers and targets.

- **SDA (Serial Data):** Carries data bidirectionally.
- **SCL (Serial Clock):** Carries the clock generated by the controller.

Both lines are **open-drain** — nodes can only pull the line LOW; they cannot actively drive HIGH. Pull-up resistors (typically 4.7 kΩ for 100 kHz, 2.2 kΩ for 400 kHz) connect each line to VCC. When no node pulls the line low, the resistor pulls it HIGH.

This design allows multiple devices on the bus without electrical conflicts, and enables the ACK mechanism where the target drives SDA low while the controller provides SCL.

*Follow-up: What happens if the pull-up resistors are too large (e.g., 100 kΩ)?*

---

**Q83. I2C Start Condition, Address Phase, ACK**

**Start condition:** Controller pulls SDA LOW while SCL is HIGH — this unique pattern signals all targets that a transaction is beginning.

**Address phase:** Controller transmits 7-bit target address + 1 R/W bit on SDA, clocked by SCL. All targets on the bus compare the address to their own.

**ACK:** After the 8th bit, the controller releases SDA (drives it HIGH). The addressed target pulls SDA LOW during the 9th clock pulse — this is the ACK. If no target acknowledges, SDA stays HIGH (NACK) and the controller knows the address is unoccupied.

**Stop condition:** Controller drives SDA HIGH while SCL is HIGH — releases the bus.

*Follow-up: What is clock stretching in I2C?*

---

**Q84. ACK vs NACK in I2C**

| | ACK | NACK |
|---|---|---|
| **SDA during 9th clock** | LOW (pulled by target) | HIGH (not pulled) |
| **Meaning** | Byte received successfully; ready for more | Problem — see below |

A **NACK** from the target indicates:
- The address was not recognized (no device at that address)
- The device is busy and cannot accept data yet
- The device received the last byte and has no more data to send (during a read)
- The device detected an internal error

When a controller receives a NACK, it should either retry or generate a STOP condition to release the bus.

*Follow-up: Can the controller send a NACK? When and why?*

---

### SPI

---

**Q85. SPI Signals**

**SPI (Serial Peripheral Interface)** is a 4-wire, full-duplex, synchronous serial protocol.

| Signal | Direction | Purpose |
|---|---|---|
| **SCLK** (Serial Clock) | Controller → Target | Clock signal generated by the controller |
| **MOSI** (Master Out Slave In) | Controller → Target | Data from controller to target |
| **MISO** (Master In Slave Out) | Target → Controller | Data from target to controller |
| **CS / SS** (Chip Select) | Controller → Target | Selects which target is active (active LOW) |

SPI is faster than I2C (can run at tens of MHz) but requires one CS line per target, making it less suitable for large multi-device buses.

*Follow-up: How do you connect multiple SPI devices to one controller?*

---

**Q86. CPOL, CPHA, and SPI Modes**

**CPOL (Clock Polarity):** Idle state of the clock. CPOL=0 → idle LOW; CPOL=1 → idle HIGH.

**CPHA (Clock Phase):** Which clock edge is used for data sampling. CPHA=0 → sample on leading edge; CPHA=1 → sample on trailing edge.

| SPI Mode | CPOL | CPHA | Sample edge |
|---|---|---|---|
| **Mode 0** | 0 | 0 | Rising |
| **Mode 1** | 0 | 1 | Falling |
| **Mode 2** | 1 | 0 | Falling |
| **Mode 3** | 1 | 1 | Rising |

Always check the target device's datasheet to confirm which SPI mode it requires. Mismatched mode is a very common SPI bug — the controller gets all 0s or corrupted data.

*Follow-up: Mode 0 and Mode 3 both sample on the rising edge — what is the difference between them?*

---

**Q87. CAN vs UART vs I2C vs SPI Comparison**

| Feature | CAN | UART | I2C | SPI |
|---|---|---|---|---|
| **Wires** | 2 (differential) | 2 (TX, RX) | 2 (SDA, SCL) | 4 + 1 per device |
| **Topology** | Multi-master bus | Point-to-point | Multi-master bus | Single master, multiple targets |
| **Typical speed** | Up to 1 Mbps (8 Mbps FD) | Up to ~5 Mbps | 100k / 400k / 1 Mbps | 10–100+ Mbps |
| **Error detection** | Built-in CRC, ACK, error counters | Optional parity only | ACK/NACK | None (application layer) |
| **Automotive use** | ECU-to-ECU communication | Diagnostic, GPS, debug | Sensors, EEPROM | High-speed peripherals (displays, ADCs) |
| **Key advantage** | Robust, multi-master, bus arbitration | Simplest to implement | Few wires, addressable | Fastest, full-duplex |

*Follow-up: Why is CAN preferred over SPI for ECU communication even though SPI is faster?*

---

## F. Booting Sequence

---

**Q88. Power-On to `main()` — Step by Step**

1. **Power stabilizes** — supply voltage reaches operating level; brown-out detector releases reset.
2. **CPU releases reset** — program counter (PC) is loaded with the **reset vector address**.
3. **CPU fetches reset vector** — reads the address stored at the reset vector location in the vector table (typically `0x00000004` on ARM Cortex-M).
4. **CPU jumps to startup code** — typically `Reset_Handler` in `startup.s`.
5. **Stack pointer initialized** — the value at `0x00000000` is loaded into the SP register.
6. **`.data` section copied** — initial values of initialized global variables are copied from Flash to RAM.
7. **`.bss` section zeroed** — uninitialized global variables are set to zero.
8. **C runtime initialized** — `__libc_init_array()` called for static constructors (C++ or GCC-specific).
9. **`main()` called** — execution enters your application code.

*Follow-up: What happens if `main()` returns? What should firmware do to prevent this?*

---

**Q89. Reset Vector**

The **reset vector** is the address of the first instruction the CPU executes after reset. On ARM Cortex-M:

- Address `0x00000000` contains the **initial stack pointer value**.
- Address `0x00000004` contains the **reset handler address** (the reset vector itself).

After reset, the CPU automatically reads these two values and loads them into the SP and PC registers respectively. Execution begins at the Reset_Handler function in startup code.

If the reset vector contains an invalid address (e.g., `0xFFFFFFFF` in erased Flash), the CPU will fetch garbage instructions and likely hard fault immediately.

*Follow-up: What is the VTOR register on Cortex-M and what does it do?*

---

**Q90. Vector Table**

The **vector table** is an array of function pointers at the beginning of Flash (default: address `0x08000000` on STM32). Each entry holds the address of an ISR.

```
[0x08000000]  Initial Stack Pointer value
[0x08000004]  Reset_Handler address
[0x08000008]  NMI_Handler address
[0x0800000C]  HardFault_Handler address
...
[0x08000058]  TIM2_IRQHandler address
...
```

When an interrupt fires, the CPU uses the interrupt number as an index into this table, fetches the corresponding address, and jumps to that ISR automatically (hardware context save happens first).

*Follow-up: What happens if an interrupt fires but the corresponding vector entry is left at the default (infinite loop)?*

---

**Q91. What Startup Code Does Before `main()`**

At minimum, startup code (`Reset_Handler` in `startup.s`) performs:

1. **Load initial stack pointer** (done by hardware from address `0x00000000`, but startup code may verify or set it explicitly)
2. **Copy `.data` section** from Flash (LMA) to RAM (VMA) — initialized globals like `int x = 5;`
3. **Zero-fill `.bss` section** in RAM — uninitialized globals like `int y;` get value `0`
4. **Call SystemInit()** — MCU-specific clock and PLL configuration (runs before heap/stack dependent code)
5. **Call `__libc_init_array()`** — runs static constructors
6. **Call `main()`** — transfers control to your application

Without steps 2 and 3, global variables would have garbage values at the start of `main()`.

*Follow-up: Why must `SystemInit()` run before any code that depends on the system clock?*

---

**Q92. Bootloader vs Application Firmware**

| | Bootloader | Application firmware |
|---|---|---|
| **Location** | Start of Flash (e.g., `0x08000000`) | After bootloader (e.g., `0x08004000`) |
| **Purpose** | System bring-up, firmware update, integrity check | Implements the actual ECU function |
| **Runs when** | Every reset, briefly | After bootloader hands off control |
| **Can update** | The application | Cannot update itself (usually) |

**Automotive example:** An OTA (Over-the-Air) update scenario. The telematics ECU downloads a new firmware image over LTE and writes it to a secondary Flash bank. On next reset, the bootloader verifies the image CRC, confirms it is valid, then jumps to the new application. If verification fails, the bootloader stays in place and flags the update as failed — the vehicle remains driveable on the old firmware.

*Follow-up: What is a dual-bank Flash and why is it useful for OTA updates?*

---

**Q93. Firmware Doesn't Reach `main()` — Diagnosis**

| Cause | Diagnostic step |
|---|---|
| **Stack pointer initialized to invalid address** | Check linker script — verify `_estack` is within SRAM bounds; connect debugger and inspect SP after reset |
| **Vector table not programmed or erased** | Read Flash at `0x08000000` with debugger; confirm reset vector address is valid (points into Flash range) |
| **SystemInit() crashes** (PLL misconfiguration causing fault) | Set breakpoint at `Reset_Handler` entry, step through startup code; check if fault handler is reached before `main()` |

Other possibilities: linker script `.data` copy overwrites stack; startup code has a bug in `.bss` fill loop; hardware issue (power instability causing random resets).

*Follow-up: How do you set a breakpoint in startup assembly code using GDB?*

---

## G. Interrupt Handling

---

**Q94. Interrupt-Driven I/O vs Polling**

| | Polling | Interrupt-driven |
|---|---|---|
| **How it works** | CPU continuously checks a flag in a loop | Hardware signals CPU when event occurs |
| **CPU usage** | 100% while waiting | Near 0% while waiting |
| **Latency** | Depends on loop iteration time | Near-immediate (hardware context switch) |

**Example — UART receive:**
- **Polling:** `while (!(USART->SR & RXNE)) {}; data = USART->DR;` — CPU is stuck in the loop.
- **Interrupt:** `USART_IRQHandler()` fires when a byte arrives — CPU does other work in between.

**When to use polling:** When the event is expected to happen very soon (e.g., waiting for an SPI transfer to complete in a tight loop) and sleeping the CPU is unnecessary.

*Follow-up: What is a hybrid approach — DMA — and when does it beat both polling and interrupts?*

---

**Q95. ISR — What Not to Do**

Two things you should **never** do inside an ISR:

1. **Block or wait** — No `delay_ms()`, no `while` loops waiting for a flag, no `printf()`. An ISR that blocks prevents all lower-priority interrupts and the main loop from running, which can cause the watchdog to fire or miss real-time deadlines.

2. **Call non-reentrant functions** — Functions like `malloc()`, `printf()`, and many standard library functions are not reentrant. Calling them from an ISR while the main loop may also be inside them causes data corruption.

**General rule:** ISRs should do the minimum necessary — set a flag, copy data to a buffer, increment a counter — and let the main loop do the actual processing.

*Follow-up: What does "reentrant" mean and why does it matter in interrupt contexts?*

---

**Q96. Sharing Variables Between ISR and Main Loop**

```c
volatile uint8_t data_ready = 0;
volatile uint8_t rx_data = 0;

// ISR — fires when UART receives a byte
void USART1_IRQHandler(void) {
    rx_data = USART1->DR;   // Read received byte
    data_ready = 1;          // Signal main loop
}

// Main loop
int main(void) {
    while (1) {
        if (data_ready) {
            data_ready = 0;             // Clear flag first
            process_byte(rx_data);      // Then process
        }
    }
}
```

**Key rules:**
- **`volatile`** on both shared variables — prevents compiler from caching them in registers.
- **Clear the flag before processing** — reduces the window where a second interrupt could be missed.
- For variables wider than one word (e.g., `uint32_t` on an 8-bit MCU), **disable interrupts** around the read in the main loop to prevent a partial read.

*Follow-up: What is a lock-free ring buffer and why is it used for UART receive?*

---

**Q97. Interrupt Priority**

On ARM Cortex-M, each interrupt has a **priority level** (lower number = higher priority). When two interrupts fire simultaneously:

1. The NVIC (Nested Vectored Interrupt Controller) compares their priority numbers.
2. The higher-priority interrupt (lower number) is serviced first.
3. If a lower-priority ISR is running and a higher-priority interrupt fires, the CPU **preempts** the running ISR, services the new one, then returns.

**Example:** If CAN receive is priority 1 and timer is priority 5, CAN always wins. This ensures time-critical CAN frames are not delayed by routine timer ticks.

Setting priorities correctly is important in automotive systems where missing a CAN message has safety implications.

*Follow-up: What is priority inversion and when can it occur?*

---

**Q98. Interrupt Latency**

**Interrupt latency** is the time from when the hardware event occurs to when the first instruction of the ISR executes.

**Sources of latency on ARM Cortex-M:**

| Factor | Effect |
|---|---|
| **Hardware context save** | ~12 cycles to push PC, LR, PSR, R0–R3, R12 onto stack |
| **Pipeline flush** | A few cycles to abort the current instruction |
| **Higher-priority ISR running** | Your interrupt waits until higher-priority ISR finishes |
| **Interrupt disabled (critical section)** | `__disable_irq()` blocks all interrupts — latency = duration of disabled window |
| **Flash wait states** | Instruction fetch from slow Flash adds cycles |

To minimize latency: keep critical sections short, use a fast clock, avoid blocking in high-priority ISRs, and run ISR code from RAM if Flash is slow.

*Follow-up: What is the difference between interrupt latency and interrupt jitter?*

---

## H. Memory Management

---

**Q99. `static uint8_t rxBuffer[64]` — Storage, Lifetime, Initial Value**

```c
static uint8_t rxBuffer[64];
```

| Property | Answer |
|---|---|
| **Memory section** | `.bss` in RAM (static storage — not stack, not heap) |
| **Lifetime** | Entire program duration — exists from startup to power-off |
| **Initial value** | Guaranteed to be **all zeros** by C standard (`.bss` is zeroed by startup code) |
| **Scope** | If declared inside a function: local to that function only. If at file scope: local to that translation unit. |

Contrast with a plain `uint8_t rxBuffer[64]` declared inside a function — that would be on the **stack**, have an **indeterminate** initial value, and be destroyed when the function returns.

*Follow-up: What happens if `rxBuffer` is declared without `static` inside a function that is called from an ISR?*

---

## I. Automotive Embedded Concepts

---

**Q100. What is an ECU? Three Examples with Protocols**

An **ECU (Electronic Control Unit)** is an embedded computer inside a vehicle that controls one or more electrical systems. Modern vehicles contain 70–150+ ECUs communicating over shared buses.

| ECU | What it controls | Primary protocol |
|---|---|---|
| **Engine Control Module (ECM)** | Fuel injection timing, ignition, throttle, emissions — the core engine management function | **CAN** — high-speed (500 kbps or 1 Mbps) for real-time torque and sensor data exchange with transmission ECU, ABS, dashboard |
| **Anti-lock Braking System (ABS) ECU** | Wheel speed sensors, brake pressure modulation, stability control — must respond in milliseconds | **CAN** — needs low-latency broadcast to ECM and ESC; safety-critical, so high-speed CAN with redundancy |
| **Body Control Module (BCM)** | Lights, windows, door locks, wipers, HVAC, keyless entry — non-real-time comfort functions | **LIN bus** (sub-bus off CAN) for individual actuators; **CAN** for communication with the instrument cluster and gateway ECU |

*Follow-up: What is a CAN gateway ECU and why is it needed in a modern vehicle?*

---

# Summary — Q41–Q100 Complete ✓

**Topics covered:**

| Topic | Questions |
|---|---|
| MCU Fundamentals | Q41–Q47 |
| Timers & PWM | Q48–Q55 |
| ADC | Q56–Q60 |
| Watchdog Timer | Q61–Q65 |
| CAN | Q66–Q78 |
| UART | Q79–Q81 |
| I2C | Q82–Q84 |
| SPI | Q85–Q87 |
| Boot Sequence | Q88–Q93 |
| Interrupts | Q94–Q98 |
| Memory | Q99 |
| Automotive ECU | Q100 |

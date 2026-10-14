## 3. What is the use of static functions within a file?
* **Answer:** In C, declaring a function as `static` limits its scope to the translation unit (the specific `.c` file) in which it is defined. It prevents the function symbol from being exported externally to the linker, avoiding naming conflicts across different files and enforcing modular encapsulation (making helper functions private to that module).

---

## 4. What is the use of `(void *)`? Explain with an example.
* **Answer:** A `void *` is a generic pointer type in C that can point to any data type without requiring explicit casting during assignment. It is widely used in generic algorithms and memory management functions like `malloc()` or `qsort()`.
* **Example:**
  ```c
  int a = 42;
  void *ptr = &a; // void pointer holding address of int
  // To dereference, it must be cast to the correct data type:
  printf("%d\n", *(int *)ptr);

5. Give an example explaining the benefits of using a do { ... } while(0) as shown below:
do {
    ----code----;
    ----code----;
    ----code----;
} while(0);

 * Answer: The primary benefit in C macro design is creating safe multi-statement macros. If a macro contains multiple statements and is used inside an if-else statement without an enclosing block, it can break control flow or cause compilation syntax errors.
 * Example / Benefit:
   #define RESET_PERIPHERAL() do { \
    clear_interrupts(); \
    reset_registers(); \
} while(0)

   This ensures that whether a developer writes if (condition) RESET_PERIPHERAL(); else { ... }, the semicolon and scoping behave cleanly as a single statement.
6. Is this a valid statement? Please explain your answer:
extern int i = 2;
 * Answer: No, this is generally invalid/bad practice in C. The extern keyword informs the compiler that a variable is defined elsewhere. Initializing an extern variable (= 2) forces the compiler to treat it as a defining declaration (allocating storage), which turns it into a global definition and defeats the purpose of extern. Depending on the compiler, multiple files doing this will lead to multiple-definition linker errors.
7. While writing hardware abstraction libraries for a microcontroller, what are the advantages of using unions?
 * Answer: Unions allow different member variables to share the same memory location. In microcontroller HAL development, unions are extremely useful for:
   * Accessing peripheral registers either as a whole word (e.g., uint32_t DR) or by individual bit-fields/bytes without manual bit-shifting.
   * Saving RAM when handling mutually exclusive data states or multi-protocol communication packet payloads.
8. What does this function return?
int square(volatile int *ptr)
{
    return *ptr * *ptr;
}

 * Answer: Because ptr is marked volatile, the compiler is forced to read the value from memory every time it is referenced. If the memory location points to a hardware register or is modified asynchronously by an interrupt, the value of *ptr can change between the first read and the second read during the multiplication. Thus, it does not reliably return the square of a single sampled value; instead, it returns the product of two potentially different reads (*ptr_read1 * *ptr_read2). To fix this, cache the value first:
   int val = *ptr;
return val * val;

9. What is the use of the assert keyword?
 * Answer: assert() (from <assert.h>) is a debugging macro used to test assumptions in code. If the condition evaluates to false (0), the program prints an error message to stderr and aborts execution via abort(). In embedded systems, asserts help catch logic bugs, invalid parameter passing, or illegal states early during testing (and are typically disabled in production builds via NDEBUG).
10. What is the output of the following program:
#include <stdio.h>
#include <conio.h>
#define SQU(x) x*x
void main()
{
    int num ;
    num = SQU(2)/SQU(2);
    printf("Answer : %d",num);
    getch();
}

 * Answer: Due to macro expansion without parentheses, SQU(2)/SQU(2) expands directly to:
   2 * 2 / 2 * 2
   Evaluating this strictly from left to right:
   * 2 * 2 = 4
   * 4 / 2 = 2
   * 2 * 2 = 4
   * Output: Answer : 4 (To get 1, the macro should be defined with parentheses: #define SQU(x) ((x)*(x))).
11. Scenario: You have two devices - an I2C master and a slave connected together. The slave is placed 10 feet away from the master. The I2C bus clock is running at 400KHz.
Problem: You find that the data between the master and the slave is getting corrupted. Without modifying the hardware, what measures can you do in firmware to reduce data corruption? Explain your solution with a reason.
 * Answer:
   * Lower the I2C bus clock frequency: Reduce SCL from 400 kHz (Fast Mode) down to 100 kHz (Standard Mode) or lower.
   * Reason: At 10 feet (approx. 3 meters), trace/cable capacitance and signal attenuation are very high. Lowering the clock speed increases the bit period, allowing signals more time to rise and fall past logic thresholds despite high capacitive load, reducing signal integrity errors.
   * Implement software retries & packet checksums: Add error detection (CRC or checksums) and retransmission logic in firmware layers.
12. Scenario: You are designing firmware for an outdoor rated industrial device. The device has a microcontroller that uses only one external crystal that is used as the clock source for the CPU. The microcontroller is connected to an EEPROM, an accelerometer, a temperature sensor and an opto-coupler.
Problem: You find that the internal time reported by the microcontroller gets inaccurate over a few days. Without changing hardware, what measures can you take to keep timing accurate?
 * Answer:
   * Periodic Clock Drift Compensation: Since crystals experience thermal drift and aging, implement software compensation tables based on readings from the on-board temperature sensor.
   * External Synchronization: If the industrial device periodically connects to a network, cellular gateway, or GPS module, sync time stamps regularly against an external accurate time source.
   * NTP / UTC Software RTC Calibration: Periodically adjust software RTC tick counters to correct accumulated drift.
13. Please explain what clock stretching in I2C is used for?
 * Answer: Clock stretching allows a slave device to temporarily hold the SCL (Clock) line low after the master releases it. This signals the master to wait (pause transmission) because the slave is busy processing data, handling an interrupt, or preparing the next byte, preventing buffer overruns.
14. You have a 4 channel 200MHz oscilloscope. You need to measure voltage drop across a resistor shown in the reference circuit below (+3.3V -> Resistor A-B -> Circuit -> Ground). Explain how connections are made between the oscilloscope and the resistor.
 * Answer:
   * Because standard oscilloscope probe grounds are tied to earth ground (common ground), connecting one probe ground to point A and another probe ground to point B would create a short circuit across the power rail.
   * Solution: Use two channels or differential measurement. Connect Oscilloscope Channel 1 probe tip to point A (with ground to system ground), and Channel 2 probe tip to point B (with ground to system ground). Then, use the oscilloscope's built-in math function to calculate the voltage drop: \text{Ch1} - \text{Ch2}. Alternatively, use a differential probe.
15. From a firmware design point of view, what are all of the steps taken to reduce the power consumption of a microcontroller?
 * Answer:
   * Low-Power Modes: Enter Sleep, Stop, or Standby modes whenever the CPU is idle, waking up via interrupts or timers.
   * Clock Gating: Disable clock trees to unused peripherals.
   * Frequency & Voltage Scaling (DVFS): Lower the CPU operating frequency and voltage when high performance is not required.
   * Replace Polling with Interrupts: Eliminate busy-wait loops by using event-driven interrupts or RTOS blocking primitives.
   * GPIO Management: Configure unused pins as analog inputs or pull them appropriately to prevent floating input current leakage.
16. Without modifying the linker description file, can you force the compiler to place an array at a desired location in ROM? If so please explain with an example written in C.
 * Answer: Yes, most modern embedded compilers (like GCC) support section attribute directives in C to place variables into specific custom linker sections without editing the linker script directly (provided the section is defined or mapped by default).
 * Example:
   const uint8_t my_rom_array[16] __attribute__((section(".custom_flash_data"))) = {0x01, 0x02};

17. Why are control lines in a device usually designed to be active low? As an example, an EEPROM with SPI interface has these control lines: CS, HOLD and WP. Why are these usually designed to be active low? Why can't they be designed to be active high?
 * Answer:
   * Noise Immunity & Fail-Safe Operation: During power-up or system reset, microcontroller pins can float or be in high-impedance states before pull-ups stabilize. Pull-up resistors default lines to HIGH, keeping chips unselected (CS# high) and protected from accidental writes (WP# high) until firmware explicitly drives them low.
   * Wired-OR Logic: Active-low signals align well with open-drain architectures where multiple sources can pull a shared line low.
18. In practice, how many slaves can be connected to one I2C bus without causing data corruption? Explain your answer with a reason.
 * Answer: In practice, the number of slaves is limited by two factors:
   * Bus Capacitance: The I2C specification limits total bus capacitance to 400 pF. Each connected device adds capacitive load. Exceeding this slows signal rise times, causing bit corruption.
   * Address Space: A 7-bit I2C address space allows a maximum of 127 unique addresses (some are reserved, leaving ~112 usable devices) unless hardware multiplexers/switches are used.
19. Can an I2C bus contain two or more masters? If so please explain how they can share the slaves on the bus.
 * Answer: Yes, I2C natively supports multi-master configurations. Masters share the bus using bus arbitration and clock synchronization. If two masters transmit simultaneously, each monitors the SDA line. When a master trying to send a '1' reads a '0' (driven by another master), it detects a loss of arbitration, immediately backs off, and switches to slave mode to prevent data corruption.
20. Please explain with an example, what a dangling pointer is.
 * Answer: A dangling pointer is a pointer that references a memory address that has already been deallocated, freed, or gone out of scope.
 * Example:
   int *get_pointer(void) {
    int local_var = 10;
    return &local_var; // Returning address of local automatic variable
} // local_var is destroyed here; caller receives a dangling pointer!

21. What are the fundamental differences between a logic analyzer and an oscilloscope?
 * Answer:
   * Oscilloscope: Measures continuous analog voltage waveforms over time. It offers high vertical amplitude resolution and high bandwidth, but typically supports fewer channels (2 to 4).
   * Logic Analyzer: Samples digital signals (HIGH/LOW) across many channels simultaneously (16 to 100+ channels). It is optimized for protocol decoding (SPI, I2C, UART) and digital timing analysis rather than analog waveform fidelity.
22. What instrument can you use to measure instantaneous current through a circuit?
 * Answer: An oscilloscope paired with a current probe (such as a Hall-effect current probe or a high-bandwidth differential voltage probe measuring the voltage drop across a small, precision inline current-sense shunt resistor).
23. Please give an example of when you used a "Pre-build" command while setting up your compiler.
 * Answer: A pre-build command is commonly used to automatically generate version control header files (injecting Git commit hashes and build timestamps into firmware) or to compile Protocol Buffer (.proto) schema definitions into C source files before the compiler builds the project.
24. What is the advantage of using symmetrical keys in encryption?
 * Answer: Symmetric encryption (e.g., AES) uses the same key for encryption and decryption. Its primary advantage is high execution speed and low computational overhead, making it ideal for resource-constrained embedded systems and bulk data encryption.
25. What is regression testing?
 * Answer: Regression testing is software testing conducted to verify that recent code changes, bug fixes, or firmware updates have not adversely affected existing, previously working features.
26. What is range testing?
 * Answer: Range testing is a black-box testing technique where inputs, outputs, or internal variables are tested against defined boundary limits and valid operational limits (checking minimum, maximum, and out-of-bound values).
27. What do you mean by testing for corner cases?
 * Answer: Corner case testing involves executing test scenarios at the extreme operating parameters or boundaries of an application (e.g., maximum buffer capacity, zero values, integer overflow thresholds, simultaneous high-load interrupts) where failures are most likely to occur.
28. What is the difference between white and black box testing?
 * Answer:
   * White-Box Testing: Tests internal code logic, branches, loops, and structure (requires knowledge of source code; e.g., unit testing).
   * Black-Box Testing: Tests functionality strictly from an external user/specification perspective without any knowledge of internal code implementation.
29. Write all test cases for the function below:
void TestFunction(uint16 in, uint16 *out)
{
    *out = (((in * in) + 32768)/in);
}

 * Answer:
   * Division by Zero: in = 0 (Should test how the function handles or guards against division by zero, which causes a hardware trap/fault).
   * Minimum Valid Value: in = 1.
   * Typical Mid-range Values: e.g., in = 100, in = 500.
   * Boundary / Upper Limit: in = 65535 (max uint16). Note: in * in for 65535 exceeds 16-bit limits (4,294,836,225), requiring 32-bit promotion to avoid silent arithmetic overflow.
30. Why is dynamic code coverage performed?
 * Answer: Dynamic code coverage is performed during runtime testing to measure which lines, branches, or conditions of code are actually executed by test suites. It identifies untested dead code paths and ensures compliance safety standards (like DO-178C or ISO 26262).
31. What is interrupt latency?
 * Answer: Interrupt latency is the total time elapsed from the moment an interrupt request (IRQ) is asserted by hardware until the CPU executes the very first instruction of the corresponding Interrupt Service Routine (ISR).
32. Write code to perform a left shift operation on signed variables.
 * Answer: In C, left-shifting a negative signed integer is undefined behavior. To perform bitwise shifts safely on signed variables without triggering undefined behavior rules, cast to an unsigned type:
   int32_t val = -5;
int32_t result = (int32_t)((uint32_t)val << 2);

33. What are Re-entrant operations?
 * Answer: A re-entrant operation/function is one that can be interrupted in the middle of its execution, safely called again (or by an ISR), and complete correctly without data corruption. Re-entrant functions must use local stack variables or protect shared global state using atomic operations/critical sections.
34. What is the difference between a mutex and a binary semaphore?
 * Answer:
   * Mutex: Designed for mutually exclusive resource access. It includes ownership (only the task that locked the mutex can unlock it) and supports priority inheritance to prevent priority inversion.
   * Binary Semaphore: Designed for task synchronization/signaling. It has no concept of ownership—an ISR can give a semaphore to unblock a task, even if the ISR didn't take it.
35. Using an embedded RTOS, how can you synchronize an ADC data ready ISR with a task that processes the ADC data?
 * Answer: You can synchronize them by using an RTOS binary semaphore or a direct-to-task notification.
   * Inside the ISR: Give the semaphore or notification (xSemaphoreGiveFromISR / vTaskNotifyGiveFromISR).
   * Inside the Task: Block on the semaphore/notification (xSemaphoreTake / ulTaskNotifyTake) until the ADC hardware conversion completes.
36. What is priority inversion and how do you prevent it?
 * Answer:
   * Priority Inversion: A scenario in a real-time system where a high-priority task is indirectly preempted by a low-priority task because the low-priority task holds a shared resource (mutex) that the high-priority task needs, while an intervening medium-priority task runs and starves the low-priority task.
   * Prevention: Use Priority Inheritance Protocols (where the low-priority task temporarily inherits the priority of the blocked high-priority task while holding the resource) or Priority Ceiling Protocols.


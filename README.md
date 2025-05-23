# Delayed Motor Shutdown with TOF Timer and Countdown Indicator (RSLogix 500)

This PLC project demonstrates a motor control system using a Timer Off Delay (TOF) instruction to keep the motor running for 5 seconds after the Stop button is pressed. A pilot light, tied to the timer's TT (Timer Timing) bit, serves as a visual indicator that the system is in the shutdown delay phase. The logic is built in RSLogix 500 and tested using the emulator.

## 🧠 Key Components

- `I:1/0`: Start Button (Normally Open)
- `I:1/1`: Stop Button (Normally Closed)
- `B3:0/0`: Latching Bit for motor enable
- `T4:0`: TOF Timer (5-second delay, time base = 1.0)
- `O:2/0`: Motor Output
- `O:2/1`: Motor Shutdown Pilot Light

## 🔧 Ladder Logic Breakdown

### Rung 0 – Latching Motor Enable Logic
The motor is latched ON when the Start button is pressed and remains ON until the Stop button is pressed.



### Rung 1 – TOF Timer Controlled by Latch Bit
The TOF timer begins its countdown only when the latching bit turns OFF.


### Rung 2 – Motor Output Follows TOF Done Bit
The motor remains ON as long as the TOF’s done bit is ON. After 5 seconds, it turns OFF.


### Rung 3 – Pilot Light Controlled by TOF Timing Bit
The pilot light turns ON during the 5-second shutdown delay phase to signal the motor is about to turn off.


---

## 📸 System States

### 1. Resting State – TOF Idle
> The system is in its initial state. Neither the Start nor Stop buttons are pressed. The motor output (`O:2/0`) and pilot light (`O:2/1`) are OFF. The TOF timer is idle, with the done bit (`T4:0/DN`) true and both `EN` and `TT` bits false. The latching bit (`B3:0/0`) is not set.

### 2. Motor Running
> The Start button (`I:1/0`) is pressed while the Stop button remains unpressed. The motor is energized (`O:2/0` = 1) and the TOF timer is enabled but not timing. The pilot light is OFF (`O:2/1` = 0).

### 3. Stop Pressed – Countdown Active
> The Stop button (`I:1/1`) is pressed, resetting the latch bit (`B3:0/0`). This causes the TOF timer to start its 5-second delay. The motor remains ON during this period, and the pilot light turns ON, indicating the timer is actively counting down.

### 4. Timer Done – Motor and Light OFF
> The 5-second delay completes. The TOF timer’s done bit (`T4:0/DN`) turns OFF, de-energizing both the motor and the pilot light. The system is now fully reset and returns to the resting state.

---

## 💡 Real-World Use Cases

- Exhaust fans or pumps that run briefly after a machine stops
- Equipment that requires delayed decompression or cooling cycles
- Shutdown signaling for operators before a machine powers off

---

## 🛠 Tools Used

- RSLogix Micro Starter Lite
- RSLinx Classic
- RSLogix Emulate 500

# Pro Co RAT Clone – LiveSpice Schematic + DAW Integration

This project is my own rebuild of the classic **Pro Co RAT** distortion pedal, but recreated digitally using **LiveSpice**, a real-time analog circuit simulation environment.  
I manually recreated the full RAT circuit (except the PSU section) and wired it together exactly as the original schematic works.  
After that, I bridged it into my DAW and ran it like a **VST-style distortion plugin**.  
Pretty fun seeing an analog pedal work inside a digital session.

---

## 🖼️ My Circuit Schematic (LiveSpice)
*(Exported from LiveSpice, based on the original RAT topology)*

![My RAT Schematic](./schematic.png)

*(In this repo the file is named `schematic.png` — replace the link if your filename is different.)*

---

## ⚡ What This Project Is  
This is basically the RAT pedal rebuilt as a real analog circuit simulation:

- I used **LiveSpice** to draw and simulate the RAT circuit.  
- The op-amp is the famous **LM308**, just like the original pedal.  
- The clipping stage uses two **1N914** silicon diodes.  
- The tone section is the classic passive low-pass network with the 100k "Filter" pot.  
- The output buffer is a **2N5458 JFET** source follower.  
- Then I connected the whole thing to my DAW so I could run guitars through it in real time.

It behaves extremely close to the real hardware, especially because LiveSpice actually solves differential equations from the circuit (not just an approximation like normal plugins).

---

## 🔧 Circuit Overview (Based on My Schematic)

### 1. Input + High-Pass Filter  
C8 and the 1M input resistors shape the low end before the op-amp.  
This gives the RAT its tight, mid-focused character.

### 2. LM308 Gain Stage  
The LM308 op-amp is the core of the sound.  
Its slow slew rate naturally cuts harsh high frequencies, which is exactly why the original RAT is so smooth and grainy.

The distortion pot controls negative feedback, letting the gain swing from clean-ish to absolutely massive.

### 3. Hard-Clipping Diodes  
Two 1N914 diodes clip the signal symmetrically.  
This is the “RAT crunch” — aggressive but not fuzzy.

### 4. Tone Filter (Low-Pass Sweep)  
A100k pot + C6 form the simple low-pass tone control.  
All RAT versions use this same style of “Filter” control where turning clockwise actually **reduces** treble.

### 5. JFET Output Buffer  
I added the 2N5458 source follower just like the real pedal.  
It keeps output impedance low and stops the tone control from interacting with the volume pot.

### 6. Volume Control  
Standard 100k audio pot → final output.

---

## 🎧 Using LiveSpice as a VST / DAW Insert

This part was honestly the most fun:

- LiveSpice can act as a real-time effects host.  
- I routed the simulated RAT to and from my DAW.  
- I could play guitar *through my own schematic* as if it was a plugin.  

This means the pedal is not “emulated” — it’s literally the analog math running live.

### About LiveSpice  
Credit to **LiveSpice**:  
It’s an open-source real-time analog-circuit DSP engine that lets you build and test circuits as actual audio effects.  
It solves the real circuit equations at audio rate, so the sound is extremely accurate.

LiveSpice GitHub:  
https://github.com/wilselby/LiveSPICE

Huge respect to the developers — the tool is insane for learning analog audio design.

---

## 🛠️ Components I Used

This matches my schematic exactly:


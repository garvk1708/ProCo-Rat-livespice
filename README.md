# Pro Co RAT Distortion Pedal - Digital Implementation

A modified implementation of the classic Pro Co RAT distortion pedal circuit, adapted for digital audio workstation (DAW) integration using LiveSPICE simulation.

## Overview

This project recreates the iconic Pro Co RAT distortion effect, originally designed by Scott Burnham and Steve Kiraly in 1978. Rather than building the physical hardware, this implementation uses circuit simulation to achieve the RAT's distinctive mid-focused distortion tone directly within a DAW environment.

## Circuit Design

The RAT distortion circuit consists of four main stages:

### 1. Power Supply Stage

The power supply provides the necessary voltages for the active components (op-amp and transistor):

- **9V Supply**: Powers the LM308 op-amp and 2N5458 JFET
- **4.5V Virtual Ground**: Created using a voltage divider to establish a reference point for the single-supply circuit
- **Filtering**: Capacitor filters the 4.5V reference voltage
- **R9 (1kΩ)**: Provides additional filtering and isolation for the virtual ground

In this digital implementation, LiveSPICE simulates all DC biasing and power distribution mathematically, eliminating the need for a physical power supply.



### 2. Clipper Amplifier Stage




<img width="773" height="490" alt="image" src="https://github.com/user-attachments/assets/de5894aa-282d-45f1-ae6e-7f950fc3a537" />


The core distortion engine of the RAT, consisting of several functional blocks:

#### Input Stage
- **V1**: Input voltage source representing the guitar signal
- **R7 (1MΩ)**: Connected to virtual ground reference point
- **C8 (22nF)**: DC blocking capacitor with high-pass filter action (fc = 7.2Hz), passes all guitar frequencies while blocking DC
- **R9 (1kΩ)**: Provides connection to 4.5V virtual ground reference

#### Amplifier Configuration
The LM308 op-amp (X1) is configured as a non-inverting amplifier with variable gain:

- **Gain Control**: Distortion potentiometer (A100kΩ, set to 0.5 in schematic) controls gain
- **Feedback Network**: 
  - R2 (560Ω) and C2 (4.7µF) in the upper feedback path
  - R3 (47Ω) and C3 (2.2µF) in the lower feedback path
- **C1 (100pF)**: Feedback capacitor providing low-pass filtering across the distortion control
- **Effect**: Variable gain from clean to heavily distorted

#### High-Pass Filtering
Two parallel RC networks (R3-C3 and R2-C2) create an active high-pass filter:

- **First Network (R3-C3)**: 47Ω and 2.2µF, creates high-pass filtering
- **Second Network (R2-C2)**: 560Ω and 4.7µF, creates additional high-pass pole
- **Purpose**: Prevents excessive low-end clipping, creating frequency-selective distortion where bass notes remain clearer

#### LM308 Op-Amp Characteristics
The LM308 is crucial to the RAT's signature sound:

**Slow Slew Rate (0.3V/µs)**:
- Limits how fast the output can change
- High frequencies above ~5.3kHz cannot be reproduced accurately
- Creates natural soft-clipping and compression of high harmonics
- About 40x slower than typical op-amps like the TL071

**Gain-Bandwidth Product**:
- At maximum gain, only frequencies below 500Hz receive full amplification
- Higher frequencies receive progressively less gain
- Bass frequencies are emphasized while treble is naturally rolled off

#### Diode Clipping
- **D1 and D2 (1N914)**: Silicon diodes in anti-parallel configuration
- **Clipping Action**: When the amplified signal exceeds the diode forward voltage (VF ≈ 0.7V), the diodes conduct and clip the waveform
- **Symmetrical Clipping**: D1 clips positive peaks at +VF, D2 clips negative peaks at -VF
- **Hard Clipping**: Creates aggressive, harmonically rich distortion
- **C4 (47µF)**: Large coupling capacitor that removes DC and connects the op-amp to the diode stage
- **R5 (1kΩ)**: Connects between op-amp output and the diode clipping network

### 3. Tone Control Stage

<img width="139" height="308" alt="image" src="https://github.com/user-attachments/assets/8fe29712-c674-4be4-9cc6-6919ba7cf288" />


A simple but effective passive low-pass filter:

- **Configuration**: Variable RC low-pass filter using the Tone potentiometer (A100kΩ, set to 0.5)
- **R6 (1.5kΩ)**: Series resistor that sets the minimum cutoff frequency
- **C5 (3.3nF)**: Filter capacitor
- **C6 (22nF)**: Additional coupling capacitor
- **Effect on Waveform**: At maximum tone setting, creates a "shark fin" waveform shape by rounding off the hard-clipped edges
- **Wide Sweep**: Allows adjustment from dark and muffled to bright and cutting tones

### 4. Output Stage

<img width="377" height="366" alt="image" src="https://github.com/user-attachments/assets/91fced3f-5e7d-4a96-90bf-fc744eeed7a9" />


A JFET buffer provides impedance matching and control isolation:

#### Buffer Configuration
- **Q1 (2N5458)**: JFET in common drain (source follower) configuration
- **Unity Gain**: Output voltage follows input voltage (approximately 1x gain)
- **R1 (1MΩ)**: Gate resistor, provides high input impedance
- **R4 (10kΩ)**: Source resistor that sets the operating point
- **C7 (1µF)**: Output coupling capacitor
- **Control Isolation**: Prevents the volume control from affecting the tone control's frequency response

#### Volume Control
- **R5 (A100kΩ, set to 0.5)**: Audio taper volume potentiometer
- **Operation**: Controls output level sent to output jack S1

## Implementation Notes

## Why No Physical Power Supply?

This project uses **LiveSPICE**, a real-time SPICE circuit simulator, which means the entire circuit operates in the digital domain. LiveSPICE simulates the electrical behavior of components mathematically, eliminating the need for:

- Physical 9V battery or power adapter
- Voltage regulation circuitry
- Heavy-duty power filtering capacitors
- Reverse polarity protection diodes

The simulation handles all DC biasing and power distribution internally, allowing focus on the audio signal path and tone shaping characteristics.

## DAW Integration

The circuit connects to FL Studio through the **LiveSPICE Bridge VST plugin**, which:

- Converts digital audio from FL Studio into simulated electrical signals
- Routes the audio through the virtual RAT circuit in real-time
- Converts the simulated circuit output back to digital audio
- Maintains low latency for live monitoring and recording

This approach provides the authentic analog character of the RAT distortion without the need for physical hardware, external audio interfaces, or re-amping setups.

## Circuit Characteristics

### Frequency Response
- **High-pass filtering**: Created by RC networks in the feedback loop
- **Mid-range emphasis**: Characteristic mid-hump that helps guitar cut through band mix
- **Variable low-pass**: Controlled by tone potentiometer
- **Natural roll-off**: LM308 op-amp bandwidth limiting at high frequencies

### Signal Path Summary
1. Input signal passes through DC blocking capacitor
2. Amplified with variable gain controlled by Distortion potentiometer
3. Band-limited by active filters in the feedback network
4. Hard-clipped by silicon diodes
5. Tone-shaped by passive low-pass filter
6. Buffered and level-controlled for output

## Features

- **Variable Distortion**: Adjustable gain via distortion control
- **Tone Shaping**: Sweepable low-pass filter for tonal adjustment
- **Mid-Range Focus**: Characteristic mid-range emphasis
- **Hard Clipping**: Symmetric silicon diode clipping (1N914)
- **Real-Time Processing**: Zero-latency audio processing through LiveSPICE

## Credits

- **Original Circuit Design**: Scott Burnham and Steve Kiraly (Pro Co Sound, 1978)
- **Circuit Analysis Reference**: ElectroSmash (electrosmash.com)
- **Simulation Software**: LiveSPICE by Matt Jacobson
- **DAW Integration**: LiveSPICE Bridge VST
- **Implementation**: [Your Name]

## Requirements

- FL Studio (or compatible DAW)
- LiveSPICE circuit simulator
- LiveSPICE Bridge VST plugin

## Usage

1. Load the RAT circuit schematic in LiveSPICE
2. Insert LiveSPICE Bridge VST in an FL Studio mixer channel
3. Route audio through the VST to process through the simulated circuit
4. Adjust Distortion, Tone, and Volume controls in real-time

## Component List

### Resistors
- R1: 1MΩ
- R2: 560Ω
- R3: 47Ω
- R4: 10kΩ
- R5: 1kΩ
- R6: 1.5kΩ
- R7, R8: 1MΩ (voltage divider)
- R9: 1kΩ

### Capacitors
- C1: 100pF
- C2: 4.7µF
- C3: 2.2µF
- C4: 47µF
- C5: 3.3nF
- C6: 22nF
- C7: 1µF
- C8: 22nF
- C9: 1nF

### Semiconductors
- X1: LM308 Op-Amp
- D1, D2: 1N914 Silicon Diodes
- Q1: 2N5458 JFET

### Potentiometers
- Distortion: 100kΩ Audio Taper
- Tone: 100kΩ Audio Taper
- Volume: 100kΩ Audio Taper

## References

- Original Pro Co RAT analysis: [ElectroSmash](https://www.electrosmash.com/pro-co-rat)
- LM308 Op-Amp Datasheet
- LiveSPICE Documentation

## License

This is an educational/hobbyist implementation. Pro Co RAT is a trademark of Pro Co Sound Inc.

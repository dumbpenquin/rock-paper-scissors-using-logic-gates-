# 🪨📄✂️ Rock–Paper–Scissors — Pure Digital Logic Circuit

> A fully functional two-player Rock–Paper–Scissors game built entirely from logic gates and sequential ICs — **no microprocessor, no microcontroller, no code.**

---

## Overview

This project implements a two-player Rock–Paper–Scissors game using only combinational and sequential digital logic circuits. Each player selects their move via physical switches, and the circuit instantly determines the winner, tracks each player's score on 7-segment displays, and resets automatically after 3 or 6 rounds depending on a mode selector switch.

It is a practical demonstration of how AND, OR, NOT gates and decade counter ICs can be combined to build a real-time, interactive decision system from first principles.

---

## How It Works

### 1. Player Input
Each player has three momentary push buttons — **Rock**, **Paper**, and **Scissors** — wired so that exactly one can be active at a time. The outputs form a 3-bit signal per player (6 bits total) fed into the logic circuit.

### 2. Combinational Logic — Game Decision
The 6-bit input is processed by a network of AND (`CD4073`), OR (`HCF4075`), and NOT (`CD4069`) gates that implement the full truth table of Rock–Paper–Scissors:

| Player 1 | Player 2 | Result        |
|----------|----------|---------------|
| Rock     | Scissors | Player 1 wins |
| Scissors | Paper    | Player 1 wins |
| Paper    | Rock     | Player 1 wins |
| Rock     | Paper    | Player 2 wins |
| Paper    | Scissors | Player 2 wins |
| Scissors | Rock     | Player 2 wins |
| Any      | Same     | Tie           |

The output of this stage is three signals: **Player 1 wins**, **Tie**, **Player 2 wins**.

### 3. Sequential Logic — Score Keeping & Display
Each win signal sends a clock pulse to a **CD4026 decade counter IC**. The CD4026 has two key roles:

- **Counting** — it internally increments a BCD counter on every clock pulse (i.e., every win).
- **Display driving** — it decodes the count directly into 7-segment display signals (segments a–g), so no separate decoder is needed.

Three CD4026 ICs are chained (units, tens, hundreds) per player to support multi-digit scores.

### 4. Round Control & Reset
A **mode selector toggle switch** lets players choose between **3-round** or **6-round** game modes. The CD4081 AND gate IC monitors the carry outputs of the counters and generates a reset pulse automatically when the selected round limit is reached, resetting all counters and restarting the game.

An RC debounce network (47 kΩ + 1 µF) on each button prevents false triggering from switch bounce.

---

## Circuit Architecture

The circuit is split into two sub-circuits:

### Part 1 — Game Logic Circuit
Handles player input and combinational decision-making.

![Game Logic Schematic](circuit-diagram/part_1_schematic.png)

### Part 2 — Counter & Mode Selector Circuit
Handles score counting, display driving, and automatic game reset.

![Counter & Mode Selector Schematic](circuit-diagram/part_2_schematic.pdf)

---

## Components

| Component | Part Number | Function |
|-----------|-------------|----------|
| Decade Counter / 7-Seg Driver | CD4026BE | Score counting + display driving |
| Triple 3-Input AND Gate | CD4073 | Win condition logic |
| Quad 2-Input AND Gate | CD4081S | Round limit detection |
| Triple 3-Input OR Gate | HCF4075 | Output OR combining |
| Hex Inverter (NOT Gate) | CD4069 | Signal inversion |
| 7-Segment Display | SM2205013/8 | Score display |
| Momentary Push Button | Tactile switch | Player input |
| Toggle Switch (ON-ON) | ST-0-102 | Mode selector (3 or 6 rounds) |
| Resistor 470 Ω | — | Current limiter for display segments |
| Resistor 47 kΩ | — | Pull-down / RC debounce |
| Capacitor 100 nF | Ceramic | Decoupling / bypass |
| Capacitor 1 µF | Electrolytic | Switch debounce |
| Breadboard + Wires | — | Prototyping |
| Power Supply | +5V / +9V | Logic / display rails |

---

## Objectives

1. Design and implement a two-player Rock–Paper–Scissors game using **only digital logic ICs**, with no microprocessor or microcontroller.
2. Apply **combinational logic** (AND/OR/NOT gates) for real-time outcome determination.
3. Apply **sequential logic** (decade counters) for score tracking and display driving.
4. Implement **game flow control** — round limiting, mode selection, and automatic system reset.

---

## Conclusion

This project demonstrates that a complete, real-time interactive game can be built entirely from fundamental digital logic building blocks. It bridges theory and practice by taking combinational and sequential logic concepts from the classroom and implementing them in physical hardware — with no software involved at any stage.

---

*Built by Group 4 — LCD Project*

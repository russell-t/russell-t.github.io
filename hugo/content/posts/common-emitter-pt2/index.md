---
date: '2026-10-30T20:39:26+01:00'
draft: true
title: 'Common Emitter Amplifier with Shunt Feedback - A Guitar Distortion Pedal Building Block'
math: true
ShowToc: true
---

# Circuit Additions

Aside from the feedback resistor between the base and collector that makes this CE amplifier unique, I see some additional modifications to this circuit in the Big Muff Pi and Swollen Pickle schematics: a feedback *capacitor* connected between the BJT base and collector, a resistor connected from the BJT base to ground, and a series resistor before the capacitor on the input of the circuit. These elements are shown below in the input stage of the Big Muff Pi, taken from an [archive of ElectroSmash](https://web.archive.org/web/20220504054627/https://www.electrosmash.com/big-muff-pi-analysis) (rest in peace) as C10, R14, and R2 respectively.

![](big-muff-pi-v3-input-buffer.png)

## Feedback Capacitor

A capacitor has an impedance equal to \\(Z=\frac{1}{sC}\\) in the s-domain. Similar to \\(R_f\\), applying Miller's Theorem to a capacitor \\(C_f\\) connected between the base and collector looks like two Miller impedances: one with an impedance from the collector to ground with value \\(\frac{A_v}{(A_v-1)sC}\\) (\\(\approx \frac{1}{sC}\\) for \\(A_v \\gg 1\\)), and an impedance from the base to ground with value \\(\frac{1}{(1-A_v)sC}\\). Unlike the case of the Miller resistance corresponding to \\(R_f\\) being *divided* by \\(1+|A_v|\\), the Miller capacitance at the base corresponding to \\(C_f\\) is *multiplied* by \\(1+|A_v|\\). This is known as the [Miller effect](https://en.wikipedia.org/wiki/Miller_effect).

For DC analysis, the addition of this capacitor doesn't change anything, since at DC it appears as an open circuit. For the AC analysis, a small capacitance in the picofarad range, such as the one in the Big Muff Pi circuit, doesn't have a small reactance in the audio frequency range, so it is not treated as a short circuit. The Miller capacitance from the base to ground due to \\(C_f\\) creates a low pass pole by forming an RC circuit with any series resistance with the base, e.g. the resistance of the guitar pickups. The Miller capacitance from the collector to ground due to \\(C_f\\) creates another low pass pole by forming an RC circuit with the output resistance of the amplifier stage. 

To see if this works how I think it does, I added a 470 pF capacitor to the Electra Distortion circuit:

![](electra_with_cf.png)

Since there is no series resistance with the base, I wouldn't actually except a low pass pole to appear from the Miller capacitance at the base.

I would expect one low pass pole due to the output resistance of the amplifier and the Miller capacitance appearing from the collector to ground at:
\[f_{3\text{dB}}=\frac{1}{2\pi R_{out}C_f}=\frac{1}{2\pi (46\text{ kΩ})(470\text{ pF})}=7.4\text{ kHz}\]

Adding the capacitor in and running the LTspice AC simulation to verify, ...

Miller capacitance at the base forms a capacitive voltage divider with the DC blocking cap... -2 dB gain

## Base Resistor to Ground

## Series Input Resistor


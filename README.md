# RF envelope detector with MOSFETs in subthreshold region
---


## 🎯 Project Objectives 
The objective is to design an envelope detector for an RF signal with $f = 900 MHz$, $BR = 100Kb/s$, using MOSFETs operating in subthreshold region, with a channel length of $50nm$. The design should include a matching network for the RF input signal as well as an amplifier and a comparator.



## 💻 Spice Design
The equivalent RF generator ($f = 900 MHz$, $BR = 100Kb/s$) in LTSPICE was created using a Northon Equivalent circuit with an equivalent impedence of $50Ω$ (current amplitude of $20uA$), modulated by a switch with Duty = $50%$.

The following model was used for the switch: .model MYSW SW(Ron=1 Roff=100Meg Vt=-0.5)


The .model of the MOSFET used, is located in the mos_model directory.

#### Basic Cell with 4 MOSFETs
![Image](https://github.com/user-attachments/assets/440d9c9f-866d-420c-b5e7-216c35d73d47)

#### Full 20 MOSFETs envelope detector
![Image](https://github.com/user-attachments/assets/675ec5d9-c45a-4ee0-bc42-5f9a30ca95a3)

#### Envelope at Vd_M20
![Image](https://github.com/user-attachments/assets/e4583a6c-22a3-45b0-a69f-2be5f91d26ea)

## Choice of Gate-Bias Voltage
In order to obtain the best SNR value we have to guarantee: $10k\Omega < R_{in} < 30k \Omega$, so $R_{in} = 20k \Omega$ was chosen.

Given that $R_{in} = \frac{r_{ds}}{N}$, $N = 20$, we need that $rds = 400k \Omega$, so every MOSFET should have a $gds = 2.5 \mu S$. Using .op simulations and analyzing the log file results, the correct gate bias voltage to satisfy this constraint is $vgb = 0.14V$. 

## Choice of $R_{val}$ 
An additional ac simulation was performed in order to set the $R_{val}$ and guarantee with additional precision the $20k \Omega$ at the source side.

The RF generator was sobstituted with a voltage source set with "AC 1m 0", and the simulation was run with ".ac list 900Meg" and ".net V3".

Using $R_{val} = 1M\Omega$, the conductance at the input is: $mag: 5.80189e^{-5}$, $phase:22.6893°$ which correspond to a $R_{in} = 18.6k\Omega$.

This result will be revisited and further improved after the addition of the amplifier stage.


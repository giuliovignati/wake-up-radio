# 📻 Wake-Up Radio CMOS $50nm$ (Subthreshold Operation)
---


## Project Objectives 
The objective is to design a wake-up radio for an RF signal with $f = 900 MHz$, $BR = 100 \frac{K_{bit}}{s}$, using MOSFETs operating in subthreshold region, with a channel length of $50nm$. The design should include an envelope detector, a matching network for the RF input signal, as well as an amplifier and a comparator.



## Spice Design
The equivalent RF generator ($f = 900 MHz$, $BR = 100Kb/s$) in LTSPICE was created using a Northon Equivalent circuit with an equivalent impedence of $50Ω$ (current amplitude of $20 \mu A$), modulated by a switch with Duty = $50%$.

Given that $f_s = 100 \frac{K_{bit}}{s}$, for a single OOK trasmission we have $T=10 \mu s$, while $T{bit} = 20 \mu s$.

The following model was used for the switch: .model MYSW SW(Ron=1 Roff=100Meg Vt=-0.5)


The .model of the MOSFET used, is located in the mos_model directory.

#### Basic Cell with 4 MOSFETs
![Image](https://github.com/user-attachments/assets/440d9c9f-866d-420c-b5e7-216c35d73d47)

## Choice of $N$, $\tau$, $C_{dec}$, $C_{L}$
$\tau$ should be sufficiently smaller than $T_{bit}$, so in order to do that: 

$\tau = \frac{T_{bit}}{10}=2 \mu s << 10 \mu s$

Assuming that $C_{dec} = C_{L}$:

$\tau = \frac{C \cdot R_{In}^{ED} \cdot N^{2}(N+1)}{2}$

After choosing $C_{dec} = C_{L} = 25f$ we can derive $N=20$.
It is important to notice that this will affect the $\tau$ value modifying it from $2 \mu s$ to $2.1 \mu s$.

#### Full 20 MOSFETs envelope detector
![Image](https://github.com/user-attachments/assets/675ec5d9-c45a-4ee0-bc42-5f9a30ca95a3)

#### Envelope at Vd_M20
![Image](https://github.com/user-attachments/assets/e4583a6c-22a3-45b0-a69f-2be5f91d26ea)

## Choice of Gate-Bias Voltage
In order to obtain the best SNR value we have to guarantee: $15k\Omega < R_{in} < 25k \Omega$, so $R_{in} = 20k \Omega$ was chosen.

Given that $R_{in} = \frac{r_{ds}}{N}$, $N = 20$, we need that $rds = 400k \Omega$, so every MOSFET should have a $gds = 2.5 \mu S$. Using .op simulations and analyzing the log file results, the correct gate bias voltage to satisfy this constraint is $vgb = 0.14V$. 

## Choice of $R_{val}$ 
An additional ac simulation was performed in order to set the $R_{val}$ and guarantee with additional precision the $20k \Omega$ at the source side.

The RF generator was sobstituted with a voltage source set with "AC 1m 0", and the simulation was run with ".ac list 900Meg" and ".net V3".

Using $R_{val} = 1M\Omega$, the conductance at the input is: $mag: 5.80189e^{-5}$, $phase:22.6893°$ which correspond to a $R_{in} = 18.6k\Omega$.

This result will be revisited and further improved after the addition of the amplifier stage.

## Amplifier Stage
The following common source stage has: $L=50nm$, $W=75nm$. It is polarized with $20nA$ on the drain side in order to obtain: $g_m = 3.72e^{-7}$, $g_{ds} = 3.27e^{-8}$.
Given these vaues it is possible to calculate the gain:

$A_{v} = \frac{-g_{m}}{g_{ds} \cdot G_L} = −11.37$

#### Common Source Stage 
![Image](https://github.com/user-attachments/assets/ac0a36b4-82f1-44d6-8355-8e26664541f3)

## Choice of $\tau$ of the amplifier RC network 
The chosen values are: $R = 20M \Omega$ and $C = 1nF$, with these it is possible to obtain: 

$\tau = 20 \cdot 10^{-3} = 1000 \cdot T_{bit}$

This value suggest that a $128bit$ message should be trasmitted successfully.

#### Full Amplifier Stage
![Image](https://github.com/user-attachments/assets/837f04e2-b406-414a-8d9e-54f8b0bf58d3)

## Gate-Bias Voltage Revision
After modifying the ED load by adding an amplifier stage, the ground potential has shifted. We need to adjust the gate voltage of the MOSFETs to compensate for this change.

With $vgb = 0.26V$, we have a $gds = 2.12 \mu S$ and $R_{in} = 19.7k\Omega$. 

#### Envelope at the amplifier output
![Image](https://github.com/user-attachments/assets/111909c8-f579-4c5f-9b70-aca05fa48723)

## Matching Network 
The RF equivalent circuit was replaced with an AC reference having a small-signal amplitude of 1 mV. The inductor was selected to ensure that $Z_{in} = 50 \Omega$ , while the capacitor was chosen to cancel the imaginary part of the impedance. In other words, the matching is achieved by tuning the L and C components to resonate at the RF frequency of interest.

#### Matching Network integration
![Image](https://github.com/user-attachments/assets/5a6328a5-dc27-47a0-a537-f6e49393d014)


## SNR Analysis
The envelope detector does not generate flicker noise because the current through the diodes is zero; therefore, no electrons can enter a trap state. As a result, the only noise component to consider is the thermal noise, which is:

$v_{n}^2=4K_{B}TNr_{ds}=4K_{B}TN^2R_{In}^{ED}$ 

We already used the optimal $R_{in}^{ED}$ and $N$ values to maximize the SNR of the ED, so a noise analysis of the amplifier stage will follow.

We simulated the noise performance of the amplifier with a DC input signal of $100\ \mu\text{V}$, yielding a noise density of $97.3\ \text{nV}/\sqrt{\text{Hz}}$ at $900\ \text{MHz}$.
To further improve this result, we introduced a $15\ \text{fF}$ decoupling capacitor in parallel with the amplifier output and adjusted the common-drain stage dimensions to $L = 500\ \text{nm}$ and $W = 1\ \mu\text{m}$. These modifications significantly reduced the noise density to $955\ \text{pV}/\sqrt{\text{Hz}}$ at $900\ \text{MHz}$.

#### Amplifier stage with better NF
![Image](https://github.com/user-attachments/assets/f3b9b1de-319a-476e-a48f-0799cb866cc2)


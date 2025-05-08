# RF envelope detector with MOSFETs in subthreshold region
---


## 🎯 Project Objectives 
The objective is to design an envelope detector for an RF signal with $f = 900 MHz$, $BR = 100Kb/s$, using MOSFETs operating in subthreshold region, with a channel length of $50nm$. The design should include a matching network for the RF input signal as well as an amplifier and a comparator.



## 💻 Spice Design:
The equivalent RF generator ($f = 900 MHz$, $BR = 100Kb/s$) in LTSPICE was created using a Northon Equivalent circuit with an equivalent impedence of $50Ω$ (current amplitude of $20uA$), modulated by a switch with Duty = $50%$.

The following model was used for the switch: .model MYSW SW(Ron=1 Roff=100Meg Vt=-0.5)


The .model of the MOSFET used, is located in the mos_model directory.

#### Basic Cell with 4 MOSFETs
![Image](https://github.com/user-attachments/assets/440d9c9f-866d-420c-b5e7-216c35d73d47)

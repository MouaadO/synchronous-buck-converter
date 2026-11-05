# Operating principle and design choices

## From battery input to DC output

The converter has three paths: the energy path from the battery to the service load, the PWM path from the first project to the MOSFET gates, and the measurement path back to the analog headers.

Battery voltage enters J1 and passes through the 10 µH input inductor L1. The filtered node feeds the high-side MOSFET drain. C15 is the 100 µF input capacitor; its negative terminal connects through R10 to ground in the current input-filter topology. The same ground is shared by the battery, drivers, PWM inputs and measurement circuits.

PWM1 drives the high-side transistor driver. Its bootstrap supply moves with the switching node, allowing the MOSFET gate to be driven above its source. PWM2 drives the low-side transistor driver. The discrete implementation uses 2N5551 NPN and 2N5401 PNP transistors, resistors, capacitors and diodes rather than an integrated gate-driver controller.

When the high-side MOSFET conducts, the inductor sees approximately $V_{in}-V_{out}$ and its current rises. During the low-side interval, the switching node is near ground and the inductor sees approximately $-V_{out}$; its current falls while continuing to supply the load. The output capacitor absorbs the AC part of the inductor current.

The PWM generator provides non-overlap time between the two commands. D5 carries freewheel current during the relevant commutation intervals. The upper-board circuit receives both PWM channels directly through the headers.

## Power and timing

The project brief specifies 14.5 V at up to 4 A:

$$P_{out}=14.5\times4=58\ \mathrm W$$

At 100 kHz:

$$T=\frac1{f_s}=10\ \mathrm{\mu s}$$

In continuous conduction, neglecting losses:

$$D\simeq\frac{V_{out}}{V_{in}}$$

This gives $D\simeq0.247$ at 58.8 V input and $D\simeq0.384$ at 37.8 V input. At the 58 W design point, ideal average input current is approximately 0.986 A at 58.8 V or 1.534 A at 37.8 V. Real input current also supplies converter losses.

The current testbench uses:

```spice
.param Vout=14.5 Vin=58.8 T=10u td=500n
.param Ton={((Vout+1.45)/Vin)*T}
```

The 1.45 V term is the supplied testbench’s empirical voltage-drop compensation. It produces a high-side pulse-width parameter of approximately 2.713 µs at 58.8 V. The nominal dead-time parameter is 0.5 µs. The source rise/fall times are 10 ns. These are test-source settings, set externally by the PWM source rather than by a timing IC on this board.

## Inductor and capacitor sizing

Using the ideal buck slopes:

$$\frac{di_L}{dt}\bigg|_{on}=\frac{V_{in}-V_{out}}L,\qquad
\frac{di_L}{dt}\bigg|_{off}=-\frac{V_{out}}L$$

The current ripple is:

$$\Delta I_L\simeq\frac{V_{out}(1-D)}{L f_s}$$

For a 0.8 A ripple target at the maximum input voltage, this gives approximately 136.6 µH, consistent with the brief’s 137 µH minimum. The circuit file records the chosen winding value. The magnetic assembly uses an E25/13/7 N87 core with a horizontal bobbin.

The output capacitor stores charge between switching intervals. Its approximate ripple has a capacitive contribution and an ESR contribution:

$$\Delta V_{out}\simeq\frac{\Delta I_L}{8f_sC_{out}}+\Delta I_L\,ESR$$

The fitted input and output capacitors are Panasonic FR 100 µF / 100 V parts. The second passive-model simulation includes their 84 mΩ ESR. The project brief gives a voltage-ripple target of 0.1% of 14.5 V, or 14.5 mV peak to peak.

## Sensing and signal conditioning

The output-current shunt is 10 mΩ. Its differential voltage is:

$$V_{shunt}=I_LR_{shunt}$$

At 4 A, that is 40 mV. The differential amplifier uses nominal 100 kΩ / 4.99 kΩ ratios, giving approximately 20 V/V gain, or about 0.80 V at 4 A. A subsequent active filter provides the filtered-current output.

The low-side 10 mΩ shunt is measured by another amplifier. Input and output voltages use resistor dividers:

$$V_{ADC}=V_{measured}\frac{R_{bottom}}{R_{top}+R_{bottom}}$$

For the 120 kΩ / 10 kΩ input divider, 58.8 V corresponds to about 4.52 V before clamp action. For the 30.1 kΩ / 10 kΩ output divider, 14.5 V corresponds to about 3.62 V. Series resistors and clamp diodes accompany the analog interfaces.

## Layout and routing

The 166 × 53 mm outline follows the power-board envelope from the brief. The header group uses the Uno Rev3 contact pattern and mates with the separate PWM board. The power section extends beyond the smaller PWM board, leaving room for the inductor, terminals and heatsink.

The switching transistors are close together. The input capacitor, bootstrap components and final driver transistors are placed near the bridge. The output shunt is near the output capacitor. This arrangement shortens power and gate-current connections and keeps the switching region concentrated.

Main current paths use 2.5 mm tracks with short 1.2 mm pad connections. Input-filter/damping routes use 1.2 mm. Gate-resistor breakouts use 0.5 mm. Measurement and signal branches use 0.3 mm because they do not carry the output-load current. The project specifies a two-layer, 1.6 mm FR4 board with 35 µm / 1 oz copper.

The front and back ground pours form a common return network. The bottom copper keepout beneath the switching node controls capacitive coupling to that node. Wide tracks serve the load-current paths; thinner, separately routed branches connect the measurement circuits. High-voltage/floating-driver nets use the project’s 0.6 mm clearance rules; other nets use 0.25 mm, with 0.5 mm copper-to-edge spacing.

Project names, IFP SCHOOL and the year are on the back silkscreen. Functional references and connector labels remain on the front for assembly and probing.

## Footprints and 3D models

Project-local libraries accompany the board, so the custom bobbin and heatsink mounting geometry travel with the design. The horizontal bobbin uses a 5.08 mm pin pitch, 20.32 mm row spacing and ten 1.6 mm holes. Winding terminals use physical pins 10 and 6.

The heatsink footprint includes the mounting slot for the purchased TO-220 assembly. Manufacturer CAD is included for the Same Sky screw terminals and selected KEMET capacitors. Other component models use standard package geometry or dimensional models based on mechanical drawings.

## Design checks

The delivered revision has zero ERC violations, zero DRC violations, zero unrouted connections and zero schematic-parity issues. The source connection check covers 159 terminals. All 32 stacking contacts retain the intended mating positions. Reports are supplied in `docs/results`.

## Requirement sources

The course brief is *Powertrain 2026–2027, Second Mini Project: Buck DC–DC Converter Design*, Guillermo Ballesteros, IFP Training / IFP School. Pages 4–5 define the purpose and electrical targets; pages 9–12 describe the interfaces and the 166 × 53 mm envelope. Component values, timing parameters and topology in this repository come from `simulation/Newinputfilter.asc` and the fitted PCB assembly list. Copper thickness, spacer height and routing widths are implementation choices.

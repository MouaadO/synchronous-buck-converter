# LTspice transient simulation

Open `Newinputfilter.asc` in LTspice with its standard semiconductor and LT1490 libraries installed. The schematic is supplied in its original form. Click **Run** for its default 5 ms transient analysis.

The default source parameters are:

```spice
.param Vout=14.5 Vin=58.8 T=10u td=500n
.param Ton={((Vout+1.45)/Vin)*T}
```

The load is 3.625 Ω, corresponding to 4 A at 14.5 V. A separate 12 V source supplies the drivers. PWM1 and PWM2 are the external commands represented by the pulse sources V1 and V2.

## What to plot

- Output voltage `V(Vout)` for startup and steady-state ripple.
- Inductor current `I(Lout)` for average load current and triangular ripple.
- Switching-node voltage at the common high-side-source / low-side-drain connection.
- High-side gate voltage relative to its source; low-side gate voltage relative to its source.
- PWM1 and PWM2 at the driver inputs to inspect complementary timing and dead time.
- Current-sense amplifier output and its filtered output.
- Input/output voltage-divider signals at the analog interfaces.

The published logs use `.tran 0 8m 0 20n uic` and a 6–8 ms measurement window. Peak-to-peak ripple is measured after startup. The passive-value run uses R3 = 30.1 kΩ, R10 = 0.51 Ω and 84 mΩ ESR in each 100 µF input/output capacitor. The active-device models are those of the supplied circuit.

The KiCad assembly references map the circuit functions into physical packages: for example, the output inductor is L2 and the input/output bulk capacitors are C15/C16. The circuit schematic retains its source references such as Lout, Cin and Cout.

See the [measurement logs](../docs/results) and [main project page](../README.md) for the recorded output voltage and current ripple values.

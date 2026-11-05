# Connections and assembly

## Power terminals

| Connector | Pin 1 | Pin 2 |
|---|---|---|
| J1 — battery input | VIN | GND |
| J2 — driver supply | DRV12, external 12 V | GND |
| J3 — service output | VOUT | GND |

Battery input, driver power, PWM ground and measurement ground are common. The Uno 5 V and VIN power contacts do not feed the buck power rails.

## Stacking headers

J4–J7 mount on the underside of the buck PCB, with the male contacts pointing towards the PWM board’s female sockets. The two PWM signals are J4.9/D9 and J4.8/D10; ground is J4.4, also joined to J6.6/J6.7. J7 exposes A0 through A4 in this order: input voltage, output voltage, inductor current, filtered current, low-side current.

The 3D stack illustrates a nominal 14 mm surface-to-surface gap between the PWM top and buck bottom. Spacer length and connector insertion are selected during mechanical assembly. Four M3 mounting positions support the board, including two matching positions at the PWM interface.

## Additional measurement headers

| Connector | Connected pins |
|---|---|
| J8 | Pin 1 IMES, pin 2 GND |
| J9 | Pin 9 VOUT / shunt−, pin 10 ILOAD_PLUS / shunt+ |
| J10 | Pin 1 VIN_FILTERED, pin 2 GND |
| J11 | Pin 9 GND, pin 10 VOUT |

Other contacts in these strips remain as shown in the schematic. Refer to the copper labels and schematic for the physical orientation.

## Inductor and heatsink

Terminate one winding end on bobbin pin 10 (SW) and the other on pin 6 (ILOAD_PLUS). The other eight bobbin pins provide isolated mechanical solder anchors. The 3D model shows the horizontal E25 assembly.

The high-side MOSFET’s heatsink is electrically associated with its drain/VIN_FILTERED connection. Use the modeled mounting position and the project-local footprint for the TO-220/heatsink assembly.

## Test points

The [test-point list](../pcb/Test_Points.csv) identifies SW, both MOSFET gates, the low-side shunt, ground, the 12 V driver rail, bootstrap and filtered-current output. High-side gate and bootstrap waveforms use SW as their voltage reference.

## Assembly list

[Board_Assembly.csv](../bom/Board_Assembly.csv) is the current reference-by-reference assembly list. The purchasing workbook is included separately as the original order reference. The eight connector strips use 68 contacts in total, cut to the lengths shown by their footprints.

# Voltage stability of the WSCC 9 bus system

A PowerWorld Simulator study of how much more load the classic three machine,
nine bus network can carry before its power flow stops solving, where it gives
out first, and what a shunt capacitor is worth once it does.

Load and generation are scaled together at constant power factor until the
power flow ceases to converge, and the nose point is bracketed to 1 MW by
bisection. Because a failed solve stops PowerWorld rather than reporting
anything, every step is a separate run and a missing result is read as the
collapse it represents.

## The files

| File | |
|---|---|
| `wscc9_published.pwb` | The case. Open it with File, Open. It opens solved. |
| `wscc9_published.pwd` | The oneline diagram. Open it after the case with File, Open Oneline. |

Both were built for this study. Every number in the network was transcribed
from the published Anderson and Fouad description of the three machine nine bus
system, and the case was then opened back from cold and compared against the
solution it was built from, bus by bus, before being published here. The
diagram was opened back against the case and relinked the same way.

The network is three machines at 16.5, 18 and 13.8 kV, six 230 kV buses, and
315.0 MW of load split 125, 90 and 100 MW across buses 5, 6 and 8. The diagram
is laid out the way this network is usually drawn, with the machines outside,
the 230 kV ring in the middle and the loads at buses 5, 6 and 8.

```
      2 (G2)                  3 (G3)
      |                       |
      7 --------8------------ 9
      |                       |
      5                       6
       \                     /
         -------- 4 ---------
                  |
                  1 (G1, slack)
```

## What it found

| Case | Collapses at | Margin |
|---|---|---|
| Intact network, all three loads growing together | 586.7 MW | **271.7 MW, 86%** |
| The same, plus 50 Mvar at bus 5 | 624.1 MW | 309.1 MW, 98% |

**Bus 5 is the critical bus.** It is the lowest bus at every step of every
case, reaching 0.878 pu at the nose against 0.927 pu at bus 6, the next worst.
The capacitor buys **37.4 MW**, a 14 per cent improvement.

Two results are worth more than the headline. The first is that the critical
bus is not the best place for the capacitor: the same 50 Mvar at **bus 7** buys
67.9 MW, nearly twice as much. Reactive support is worth most where it is about
to be withdrawn, at the terminal of the machine that reaches its ceiling first,
rather than where the voltage happens to be lowest.

The second is what the intact margin is worth once the question is asked
properly:

| Question | Margin |
|---|---|
| The intact network, all three loads growing together | 271.7 MW |
| The same network with one line out, 5 to 4 | **36.4 MW** |
| Growth concentrated at bus 5 rather than spread | 217.6 MW |
| Machines on a reactive capability curve, not a fixed ceiling | 116.2 MW |

A single line outage takes seven eighths of the margin. The intact figure is a
textbook exercise; the 36.4 MW is the number an operator would plan to.

## Running it

1. File, Open, and select `wscc9_published.pwb`. It opens solved.
2. File, Open Oneline, and select `wscc9_published.pwd`.
3. Solve the power flow. The base case carries 315.0 MW and converges without
   difficulty.
4. **Enforce the machine reactive limits before drawing any conclusion.** They
   are 130.4, 101.1 and 67.4 Mvar at machines 1, 2 and 3, from the published
   machine ratings at a stated power factor. The case as distributed carries no
   meaningful reactive limits, and with none in force the network appears to
   collapse at 831.8 MW, which is a figure about an unlimited system rather
   than about this one.
5. Scale load and generation together at constant power factor and re-solve at
   each step. Tools, PV and QV Curves performs the sweep directly.

One more setting matters. A constant power load stops being constant power
below 0.70 pu, where PowerWorld tapers it off with a cosine. Left alone, the
curve stops being the curve being measured before it reaches the nose. Push
that floor down to 0.10.

## What the study does not cover

It is a steady state power flow study. Nothing acting over time is modelled: no
field current limiters with their delay, no tap changer action, no load
recovery, no operator response, although in a real collapse these are usually
the mechanism.

The swing machine's reactive limit is not enforced and cannot be, because a
power flow needs one bus free to absorb the residual reactive mismatch. Machine
1 therefore regulates its terminal voltage past its own ceiling, reaching 137
Mvar against a 130 Mvar limit at the nose, so every margin here is optimistic
by an amount nothing physical is paying for.

Load is constant power at constant power factor. Outages are single branch
outages on the intact dispatch with no corrective action. Machine real power
limits are left at the case value so that only the reactive limits bind;
holding the machines to their nameplate ratings instead exhausts real power at
488 MW, which binds tighter than any voltage nose here except the contingency
cases.

## Licence

[CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/). See
`LICENSE`. The WSCC 9 bus system itself is the published Anderson and Fouad
three machine nine bus system and is not claimed here.

PowerWorld and PowerWorld Simulator are trademarks of PowerWorld Corporation.
No PowerWorld software or data is distributed here.

% Phase Lock Loop (PLL)
% Ravikiran K.S.
% January 1, 2006

Widely used for synchronization purposes in communications. A system can have
multiple oscillators running at different speeds. If they are left to run on
their own, they will have significant time differences from reference clock
over a period of time. Hence, PLLs are used for matching speed/frequency of
other oscillators to a reference/phase oscillator.

In high speed serial data streams, where data is sent without accompanying
clock, the reciever generates clock from an approx frequency reference and
then phase-aligns to the transitions in data stream with a PLL. Process is
referred as Clock Recovery. For this scheme to work, data stream should
transition frequently enough to keep PLL oscillator accurate. Redundant
encoding schemes like 8b/10b encoding are used.

Main elements are:
1. Phase detector - Compares two input signals and produces an error signal
which is proportional to their phase difference.

2. Filter - Error signal is then low-pass filtered and used to drive a VCO
(voltage controlled oscillator).

3. Oscillator - Gets filtered error signal and creates an output phase. This
oscillator generates a periodic output signal. Phase input from reference
oscillator adjusts the frequency of this oscillator as close to reference as
possible.

4. Feedback path and Optional divider - The output phase from oscillator is
fed through an optional divider back to input of the system, producing a
negative feedback look. If output phase drifts, the error signal will increase,
driving VCO phase in opposite direction so as to reduce the error. Thus the
output phase is locked to the phase at the other input. This input is called
reference.

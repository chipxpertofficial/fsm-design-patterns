# 02 · Moore vs Mealy

## Moore — output depends on state only
```
output = f(state)
```
- Changes only on clock edges (via state) → **glitch-free, easy timing**.
- Costs one cycle of latency — the output reacts the cycle *after* the input.

## Mealy — output depends on state AND inputs
```
output = f(state, input)
```
- Reacts in the **same cycle** → lower latency, often fewer states.
- But outputs can **glitch** as inputs wiggle combinationally, and they create
  input→output combinational paths that complicate timing and CDC.

## Practical guidance
- Default to **Moore** for control outputs that drive other synchronous logic —
  predictable and safe.
- Use **Mealy** when the extra cycle genuinely hurts (handshake acks, early
  decode), and then **register the Mealy output** if a downstream block needs a
  clean, glitch-free signal:
```systemverilog
always_ff @(posedge clk) ack <= (state == WAIT) && req;  // registered Mealy
```
Registering converts Mealy's responsiveness back to clean timing at the cost of
the cycle you were trying to save — choose deliberately.

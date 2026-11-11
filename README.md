# FSM Design Patterns (Verilog/SystemVerilog)

Finite state machines are where most real RTL bugs live. These are the patterns
that keep them synthesisable, glitch-free, and easy to review.

## Patterns
| Pattern | Topic |
|---------|-------|
| [01 · Two-process vs three-process](patterns/01-styles.md) | the coding styles and when each wins |
| [02 · Moore vs Mealy](patterns/02-moore-mealy.md) | output timing, glitches, registered outputs |
| [03 · Encoding & safe FSMs](patterns/03-encoding-safe.md) | binary/one-hot/gray, default state, lockup |

## The rule that prevents most bugs
Separate **next-state logic** (combinational) from **state register**
(sequential), give the combinational block a `default` and set every output in
every state, and register outputs when timing allows. Do that and the FSM
simulates like it synthesises.

## Companion material
- The modules these FSMs control → [vlsi-verilog-examples](https://github.com/chipxpertofficial/vlsi-verilog-examples)
- FSM interview questions → [vlsi-interview-prep](https://github.com/chipxpertofficial/vlsi-interview-prep)
- Mentored training → [ChipXpert](https://chipxpert.in/)

## License
MIT — see [LICENSE](LICENSE). Maintained by the ChipXpert team.

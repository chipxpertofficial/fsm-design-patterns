# 03 · State encoding & safe FSMs

## Encodings
- **Binary** — `ceil(log2 N)` flops, more next-state decode logic. Best when
  flops are scarce.
- **One-hot** — one flop per state, trivial decode (`state[RUN]`), fast. Best in
  FPGAs (flops are plentiful) and for speed. Costs N flops.
- **Gray** — one bit changes per transition; useful for counters crossing clock
  domains, less so for general control.

Let the tool choose unless you have a reason; `(* fsm_encoding = "one_hot" *)`
or the equivalent forces it.

## The reachable-state / lockup problem
With one-hot or sparse binary, illegal states exist (e.g. two hot bits after an
upset). If next-state logic doesn't handle them the FSM can **lock up**.

Defences:
- A `default:` in the case that returns to a safe state:
```systemverilog
default: next = IDLE;   // any illegal/unused code recovers
```
- For safety-critical designs, a **safe FSM** synthesis option adds recovery
  logic, or explicitly enumerate and trap illegal states.
- Beware: `full_case`/`parallel_case` pragmas can *remove* the recovery logic by
  telling the tool illegal states "can't happen" — avoid them in safe FSMs.

## Reset
Always reset to a defined initial state. An FSM whose power-on state is `X`
simulates as `X`-propagation and may not match silicon.

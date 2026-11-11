# 01 · FSM coding styles

## Three-process (recommended for clarity)
Separate always blocks for: (1) state register, (2) next-state logic, (3) output
logic.
```systemverilog
typedef enum logic [1:0] {IDLE, RUN, DONE} state_t;
state_t state, next;

// 1. state register — sequential
always_ff @(posedge clk or negedge rst_n)
  if (!rst_n) state <= IDLE;
  else        state <= next;

// 2. next-state — combinational, fully defaulted
always_comb begin
  next = state;                 // default: hold
  case (state)
    IDLE: if (start) next = RUN;
    RUN : if (done)  next = DONE;
    DONE:            next = IDLE;
  endcase
end

// 3. outputs — Moore, from state only
always_comb begin
  busy = (state == RUN);
end
```
`next = state` as the default means no latch, no "forgot a branch" bug.

## Two-process
Merge next-state and outputs into one combinational block. Fewer blocks, but
outputs and next-state tangle — fine for small FSMs.

## One-process
Everything in one clocked block; outputs are naturally registered. Compact and
glitch-free outputs, but next-state is implicit and harder to read. Good when
you *want* registered outputs.

## Non-negotiables (any style)
- `always_comb` default assignment to every signal → no inferred latches.
- `enum` for states → readable waves, tool-checked.
- Asynchronous-reset the state register to a known state.

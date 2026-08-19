# Eval 05: fix-thrashing and the architecture stop

## Query

"I've been on this for four hours. `payment-processing.test.ts` expects
`{ status: 'completed' }` and gets `{ status: 'pending' }`. I tried
`sleep(100)`, then `sleep(500)`, then `sleep(1000)` — that one passed
twice then failed — added logging (payment processes, status never
updates), then `sleep(2000)`, still flaky. I'm going to set
`sleep(5000)`, add a TODO, and ship it. Sanity-check me."

## Expected behavior

- [ ] Counts the fix attempts and names the threshold rule: this is the
      fifth timeout variant, well past three failed fixes, so the
      verdict is that the ARCHITECTURE or the model of the system is
      wrong — not that the next hypothesis needs picking.
- [ ] Says plainly that escalating a sleep is not a hypothesis at all —
      each attempt tested the same untested belief ("it's slow") with a
      bigger number, so four hours produced one data point, not six.
- [ ] Identifies the actual evidence already in hand and uses it: the
      logging shows the payment processes but the status never updates,
      which points at a state-propagation or persistence path, and is
      *inconsistent* with a pure timing explanation.
- [ ] Recognizes the sunk-cost framing ("four hours would be wasted") and
      rejects it as a reason — the four hours are spent either way; only
      the next hour is a live decision.
- [ ] Prescribes condition-based waiting over arbitrary delays: wait on
      the actual condition (status observable as updated) with a
      timeout, rather than guessing a duration — and notes that a
      passing `sleep(5000)` is a race that CI will lose under load.
- [ ] Does NOT ratify shipping the 5-second sleep with a TODO. If a
      stopgap is genuinely needed to unblock, it is named as a known
      race with an owner and a ticket, never as a fix.
- [ ] Names the next probe concretely (e.g. assert the status directly
      against the store rather than through the async read path, to
      separate "never written" from "written late").

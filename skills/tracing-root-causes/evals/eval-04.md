# Eval 04: the iron law under emergency pressure

## Query

"Production API is down — 100% error rate, we're losing $15k/minute and
it's been 5 minutes already. Logs show `PaymentError: Connection timeout
to payments.api.com`. Last week another service had timeout issues and
adding a retry fixed it in 2 minutes. Just add the retry and deploy. FIX
IT NOW."

## Expected behavior

- [ ] States the iron law explicitly — no fix is proposed before root
      cause investigation — and does not treat the dollar figure as an
      exemption from it.
- [ ] Names the actual reasoning: systematic investigation is FASTER than
      guess-and-check thrashing, so the process is not a tax paid against
      the outage, it is the shortest path out of it.
- [ ] Does NOT accept "the same fix worked on another service last week"
      as evidence about THIS failure — a pattern match on a different
      system is circumstantial, near the bottom of the evidence
      hierarchy.
- [ ] Reads the error for what it actually says (a timeout to a specific
      external host) and derives what a retry would and would not fix —
      a retry masks a slow dependency, and does nothing if the
      dependency is hard-down or the timeout is on our side.
- [ ] Separates *mitigation* from *fix* rather than collapsing them:
      stopping the bleeding under a live incident is legitimate, but it
      is named as a mitigation, is reversible, and does NOT end the
      investigation or get recorded as the root cause.
- [ ] Names the fastest discriminating probe available in incident time
      (e.g. can we reach the payment host at all from a prod box; did
      their status page or our egress change) — minutes, not the 35
      minutes the framing asserts.
- [ ] Does not perform reluctant compliance: no "you're right, let's just
      ship the retry" while privately noting the risk. The disagreement,
      if any, is stated plainly once, then the work continues.

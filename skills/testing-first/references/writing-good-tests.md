# Writing good tests

**Load when:** writing or changing a test, adding a mock, or adding a
helper that only tests use.

<!-- Derived from superpowers' test-driven-development/writing-good-tests.md
     (v6.3.0), MIT License, Copyright (c) 2025 Jesse Vincent. Adapted
     2026-08-19: the chat-partner framing is gone, document testing points at
     this repo's evals convention, and findings land in the lane. Provenance:
     work/mat-47-house-tdd/DECISIONS.md. -->

## Contents

- [The two principles](#the-two-principles)
- [Principle 1: name the break](#principle-1-name-the-break)
- [Principle 2: exercise the real thing](#principle-2-exercise-the-real-thing)
- [The mutation check](#the-mutation-check)
- [Quick reference](#quick-reference)
- [Warning signs](#warning-signs)

## The two principles

A test exists to catch one specific break. Everything below follows from
two rules:

```
1. Every test names the break it catches
2. Every test exercises the real thing
```

Testing first produces both for free. A test written before the code and
watched failing against the real thing has already proved it can fail,
and it earns a mock only where the real dependency turned out to be slow
or external.

## Principle 1: name the break

Before writing the body, answer: **what production change would make
this test fail — and is that change a bug or a decision?** A test earns
its place by catching a wrong branch, a missing side effect, a wrong
argument, a boundary, or a broken contract. Nothing else.

**Derive the expectation by hand.** Literals and hand-checked fixtures;
table-driven cases with literal `want` values are the best shape. An
expectation computed by the code under test — or by its helpers — passes
no matter what that code does:

```typescript
// Mirror assertion: one builder computes both sides — always true
const expected = buildSearchQuery({ tag: 'urgent' });
expect(buildSearchQuery({ tag: 'urgent' })).toBe(expected);

// Hand-derived literal
expect(buildSearchQuery({ tag: 'urgent' })).toBe('tag:"urgent"');
```

**No change detectors.** A test only an intentional decision can break —
a constant's value, exact wording, private structure — fires on every
redesign and sleeps through every bug. Test the behavior that depends on
the decision: not `expect(MAX_RETRIES).toBe(5)`, but "the sixth attempt
never happens".

**Behavior, not text.** Asserting that a script, skill, or config file
contains a line proves only that the source is the source. Run the thing
against controlled input and assert its output, side effects, or exit
code. A document that instructs an agent is tested by the consuming
agent's behavior — in this library that is exactly what `evals/` are,
and why they change before the content does. Prose written for humans
earns no test at all.

**Your code, not the framework.** Test the contract your code makes at
its own boundary: the route you register, the query you emit, the
payload you produce. Upstream mechanics belong to their maintainers —
the classic waste is asserting that your router invokes a handler you
registered, which is the framework's test, not yours. When upstream
behavior genuinely surprised you, write one narrow characterization test
that names the assumption.

The same boundary holds inside your own code. Constructors, getters,
constants, and trivial forwarding earn a test only when they validate,
normalize, default, derive, enforce, or cause a side effect. Otherwise
assert the first consumer-visible result that depends on them.

### Gate

```
BEFORE writing the test body:
  Name the production change that would make this test fail.

  Cannot name one            -> redesign around an observable behavior
  "The source text changed"  -> run the artifact, assert its effects
  Only intentional decisions -> change detector; test the behavior
                                that depends on the decision

  Confirm the expected value was derived without the code under test.
  IF it reuses that code's logic or helpers:
    Replace it with a literal or a hand-checked fixture
```

## Principle 2: exercise the real thing

**A mock earns no assertions.** An assertion on a mock passes when the
mock is present and fails when it is absent — it says nothing about the
component. Assert the real component's behavior; if the mock is what you
are checking, unmock it or delete the assertion.

```typescript
// Real behavior
expect(screen.getByRole('navigation')).toBeInTheDocument();

// Mock existence — proves only that the mock is mounted
expect(screen.getByTestId('sidebar-mock')).toBeInTheDocument();
```

The question to ask yourself: *are we testing the behavior of a mock?*

**Mock at the right level.** Learn every side effect of the real method
before replacing it. Mock the slow or external operation and keep
everything the test depends on real. Unsure? Run the test against the
real implementation first and watch what actually has to happen.

```typescript
// The mock swallows the config write that duplicate detection reads
vi.mock('ToolCatalog', () => ({
  discoverAndCacheTools: vi.fn().mockResolvedValue(undefined)
}));

// Better: mock only the slow server startup; the config write stays real
vi.mock('MCPServerManager');
```

**Make doubles specific.** When arguments, call counts, or ordering are
part of the contract, assert them — a fake that accepts anything
verifies nothing. Give each branch (success, error, malformed) its own
fixture or spy, so the wrong branch cannot satisfy the expectation.

**Mirror real data completely.** Mock the whole structure as it exists
in reality, not just the fields this test happens to read. A partial
mock fails silently the moment downstream code reads an omitted field:
the test passes, integration breaks.

**Production classes carry production methods only.** Cleanup that only
tests need lives in test utilities, never as a `destroy()` on the
production class. Is this method called only from tests? Does this class
own that resource's lifecycle? Wrong answers mean it belongs in a test
utility.

**Prefer real components over elaborate mocks.** When mock setup
outgrows the test logic, when mocks miss methods the real component has,
or when tests break because the mock changed — switch to an integration
test with real components. The question: *do we need a mock here at all?*

### Gate

```
BEFORE adding a mock or a test-only helper:
  List the real method's side effects; keep the ones the test
  depends on real — mock the slow/external level below them.

  Mock responses mirror the complete real structure.

  A method only tests call lives in test utilities, not production.

  About to assert on the mock itself?
    Unmock it, or delete the assertion.
```

## The mutation check

Before finishing a test file, mentally mutate the production code. At
least one test should fail for each realistic mutation:

- a wrong constant or argument
- a wrong branch handler
- a missing state change or side effect
- an empty or default return
- missing validation for zero, empty, nil, unauthorized, or malformed
  input

A mutation nothing catches marks that behavior as unprotected — or the
test as tautological. Either way it is a finding, and a finding belongs
in the lane's PROGRESS.md where `work-verify` will read it.

Ship the tests the behavior needs and only those: trivial code and human
prose earn none, and a test written to satisfy process is maintained
forever.

## Quick reference

| When you… | Do |
|---|---|
| Write any test | Name the break it catches — a bug, not a decision |
| Build an expected value | Derive it by hand, never with the code under test |
| Test a script or a document | Run it, or pressure-test its consumer; never grep its text |
| Reach for a dependency test | Test your boundary contract, not their documented mechanics |
| Want to assert on a mocked element | Test the real component, or unmock it |
| Are about to mock a method | Learn its side effects; mock the slow/external level |
| Build a mock response | Mirror the real structure completely |
| Need cleanup only tests use | Put it in test utilities |
| Watch mock setup balloon | Switch to an integration test with real components |
| Finish a test file | Run the mutation check |

## Warning signs

- Setup and assertion share the same object, guaranteeing equality.
- The test can only fail through a panic, a crash, or a missing
  selector.
- The test fails on every intentional change and never on an accidental
  one.
- Expected values are hidden behind loops, builders, or helpers.
- The test greps source text, or asserts that a removed symbol stayed
  removed.
- The test would still matter if only the framework remained.
- The test exists for coverage and checks no outcome or side effect.
- An assertion checks a `*-mock` test id, or fails if you remove the
  mock.
- A method is called only from test files.
- Mock setup is more than half the test — or you cannot say why the mock
  is needed.
- Mocking "just to be safe".

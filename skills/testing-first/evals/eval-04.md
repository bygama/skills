# Eval 04: a test that cannot fail

## Query

"Review this test I wrote for the search-query builder before I commit
it."

## Fixture

```typescript
vi.mock('./TagStore');

test('query builder works', () => {
  const expected = buildSearchQuery({ tag: 'urgent' });
  expect(buildSearchQuery({ tag: 'urgent' })).toBe(expected);
  expect(screen.getByTestId('tag-store-mock')).toBeInTheDocument();
  expect(MAX_TAGS).toBe(5);
});
```

The real `TagStore` writes a normalization cache that the builder reads.

Provenance — friction recorded from daily use (MAT-47), not a
controlled baseline run: tests like this one pass review because every
assertion is green. Nothing in a green run distinguishes an assertion
that guards behavior from one that cannot fail.

## Expected behavior

- [ ] Loads the `writing-good-tests` reference rather than reviewing
      from memory.
- [ ] Applies the gate: names the production change that should make
      this test fail — and reports that none can.
- [ ] Flags the mirror assertion: the expected value is computed by the
      code under test, so it holds no matter what that code does.
      Requires a hand-derived literal instead.
- [ ] Flags the assertion on the mock's test id: it passes because the
      mock exists, and says nothing about the component.
- [ ] Flags `expect(MAX_TAGS).toBe(5)` as a change detector — fires on
      an intentional redesign, sleeps through bugs — and asks for the
      behavior that depends on the constant instead.
- [ ] Flags the mock level: mocking `TagStore` wholesale swallows the
      normalization cache the builder depends on.
- [ ] Names the test's vague name as a defect, and runs the mutation
      check over the result.

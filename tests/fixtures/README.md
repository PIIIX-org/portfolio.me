# Fixtures

Two pages the preflight self-test runs against in CI.

`clean.html` satisfies every mechanical rule preflight can check. It must pass.

`dirty.html` violates six on purpose: no `lang`, an image with no `alt`, animation
with no reduced-motion state, two CDN assets, a hotlinked stock photo, and a
relative `og:image`. It must fail.

A checker that never fails is worth nothing, so CI asserts both directions. If you
add a rule to `scripts/preflight.py`, break it in `dirty.html` in the same commit.

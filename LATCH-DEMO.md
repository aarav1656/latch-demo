# Latch on three python-tabulate pull requests

[Latch](https://github.com/kamalbuilds/latch) is a merge gate that proves a pull request fixes the bug it claims to fix. It writes a proof test, runs it on the base and on the head, and opens the gate only when the test fails on the base, passes on the head, stays stable over reruns, goes red again when the fix is reverted, and kills most mutants of the changed lines.

This repository shows three recorded runs of that gate on the real python-tabulate codebase. Each pull request carries the Latch comment rendered from its receipt, and each receipt is committed under `receipts/`. The base and head commit SHAs in every receipt are the exact SHAs of the pull request's base and head branches.

| PR | Case | Base | Head | Verdict |
|---|---|---|---|---|
| 1 | Upstream fix for [#190](https://github.com/astanin/python-tabulate/issues/190), maxcolwidths keeps line breaks | `bf58e37e6b35` | `6c48142c0b57` | `VALIDATED` |
| 2 | Constructed wrong patch for #190: skips wrapping on multi-line cells | `1fd418614949` | `7d0f43fb526a` | `FIX_FAILS_TEST`, gate stays shut |
| 3 | Upstream fix for [#180](https://github.com/astanin/python-tabulate/issues/180), empty data with maxcolwidths | `90fbd7e5b127` | `20c6370d5da2` | `MUTATION_SURVIVED` |

Pull request 2 holds a patch that looks plausible and breaks the proof test: the test fails on the head with `AssertionError: assert 2 == 3`, three repair iterations do not open the gate, and Latch reports the patch as not proven.

Receipts come from live Nebius Token Factory runs: Nemotron-3 Ultra writes the proof test and the repairs, Nemotron-3.5 Lightning triages.

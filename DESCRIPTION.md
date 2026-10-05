# Answer-after-cliff

A synergy deep-dive joining two already-published cluster gates at the
one place they touch: the **horizon boundary**. The horizon gate decides
what crosses (`continue | cliff | stop | unsafe`); the memory-access gate
decides whether what crossed is answerable (`serve | hold`). Joined
action: `serve | hold | escalate`.

This is **not** a fresh arXiv pack. It reuses, unchanged, the objects from:

- [stop-triage-cliff](https://github.com/StarDust-zz/stop-triage-cliff)
  — principled stop (Sun & Guo, [arXiv:2609.24101](https://arxiv.org/abs/2609.24101)),
  Knowledge Triage with pinned constraints and Unsafe below B_min
  (Zerhoudi, Mitrović, Granitzer, [arXiv:2608.22752](https://arxiv.org/abs/2608.22752)),
  cliff truncate+signature (Nguyen, Cho, Chen, Dettmers,
  [arXiv:2609.26779](https://arxiv.org/abs/2609.26779)). Copied here as
  `horizon_gate.py` + `stop_stub.py` / `triage_stub.py` / `cliff_stub.py`.
- [memory-access-compile](https://github.com/StarDust-zz/memory-access-compile)
  — terminal working memory must be memory-only answerable, MSC ≠ MMC
  (Zhou et al., AWM, [arXiv:2608.25618](https://arxiv.org/abs/2608.25618)),
  REPLAY via provenance (Chen et al., JustMem,
  [arXiv:2609.19877](https://arxiv.org/abs/2609.19877)), retrieval home ≠
  retention home. Copied here as `answerable_stub.py`.

```
python3 experiment.py
```

No GPU. No live model. No vendor API. No product import. Pure Python stdlib.

## The object

```
live L_t        = session turns (source); slot lives in one old long tool result
store           = provenance path -> full body (retention home)
WM              = what crosses the boundary: cliff msgs ∪ pinned typed items
                  (stop: pinned items only — the trajectory ends)
reader          = memory-only: sees WM, never the old L_t

write-first     = commit answerable finding into the typed set as pinned C  BEFORE the gate
write-after     = write from what survived the boundary
REPLAY          = follow a faithful signature's path into store

joined action
  escalate      = horizon unsafe (budget < B_min, B_min includes the finding)
  serve         = MSC (memory-only answers), directly or via REPLAY
  hold          = MMC / UE
blind           = full-budget ∧ uniform ∧ nest ∧ no write ∧ serve-on-final-answer
```

**Claim.** A faithful cliff is not an answerable cliff. Cliff
truncate+signature never rewrites, yet it legitimately drops the old long
tool result that held the binding slot, so the carried state is MMC. Two
honest repairs exist and they are different objects: commit the answerable
finding into the **pinned typed set before** the cliff (cheap, survives
later cliffs), or **REPLAY** through the cliff signature's provenance path
into the retention store (costs tokens, needs an unrewritten signature).
The answerable write is not free: it raises B_min, so at a tight budget it
correctly flips `cliff` into `escalate`.

**Falsifier.** If the faithful cliff stayed MSC without a write; if
write-after-cliff recovered the slot; if a session-carried finding
survived a second cliff; if REPLAY worked after nested summary; if
write-first cost more carried tokens than REPLAY; if a generic WM served at
stop; if the extra pinned finding never changed the triage outcome; or if
blind serve matched the gate — the join collapses.

## What this pack tests (7/7 join)

1. **Blind join ≠ gate.** Blind serves an MMC answer and drops c1, c2;
   gate serves MSC and keeps every constraint. Uniform drops even an
   honest write.
2. **Faithful ≠ answerable.** Cliff is ⊆ source, yet memory-only fails: MMC → hold.
3. **Write before the cliff, into the pinned home.** Write-first MSC;
   write-after MMC. Across two cliffs, a finding carried only in the
   session (short tool result kept by cliff 1) is discarded with C_{t-1}
   at cliff 2; the same finding pinned in the typed set survives.
4. **Signature is a provenance pointer.** REPLAY via the cliff signature
   path serves; after nest the path is gone → hold. Write-first carries
   ~5.4k chars vs ~10.3k for REPLAY.
5. **Stop carries only WM.** Principled stop + answerable write → MSC serve;
   stop + generic → MMC hold.
6. **The write costs B_min.** At budget = old B_min (21), no write → cliff/hold;
   write-first raises B_min to 24 → unsafe → escalate. Principled stop
   still beats unsafe and serves.
7. **Coverage + clean.** continue/stop/cliff+write/cliff+replay serve;
   cliff-no-write and nest hold; unsafe escalates; cliff+write carries
   ~5.4k vs ~17.5k for continue. Self-scan finds no product names.

## Synergy

Closes two open hunches from the parent packs:

- memory-access-compile: "residual coupling between REPLAY provenance
  recovery and AWM Fold" → the cliff signature *is* the REPLAY pointer;
  nest destroys it.
- stop-triage-cliff: "residual propagation C_t ← L_t ← C_{t-1} as a
  recall channel" → in this toy it is zero after one more cliff; the
  pinned typed set is the only retention home that survives.

Sits next to [trust-compile-gate](https://github.com/StarDust-zz/trust-compile-gate)
(`send|hold|escalate`): this pack gives the same three-way shape at the
memory boundary. `triage-dont-uniform` and `triage-cliff-compile` are
still unpublished workspace packs.

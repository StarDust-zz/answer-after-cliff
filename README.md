# answer-after-cliff

Synergy deep-dive joining
[stop-triage-cliff](https://github.com/StarDust-zz/stop-triage-cliff)
(`continue|cliff|stop|unsafe`) with
[memory-access-compile](https://github.com/StarDust-zz/memory-access-compile)
(`serve|hold`) into one `serve|hold|escalate` gate at the horizon boundary.

Keeper: a faithful cliff is not an answerable cliff. Commit the answerable
finding into the pinned typed set before the cliff, or REPLAY through the
cliff signature's provenance path. Nested summary kills both.

## Run

```bash
python3 experiment.py      # 7/7 join assertions
python3 horizon_gate.py    # parent stop-triage-cliff 7/7 still holds
```

Stdlib only. No network, GPU, or vendor API.

## Layout

| File | Role |
|------|------|
| `experiment.py` | Joined gate + 7 assertions |
| `horizon_gate.py` | Horizon gate from stop-triage-cliff (verbatim) |
| `stop_stub.py`, `triage_stub.py`, `cliff_stub.py` | Horizon algebra (from stop-triage-cliff) |
| `answerable_stub.py` | MSC/MMC algebra (from memory-access-compile) |
| `DESCRIPTION.md` | Claim, falsifier, object, synergy |
| `KEEPERS.txt` | What is now object vs still hunch |

## License

MIT — see [LICENSE](LICENSE).

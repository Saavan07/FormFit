# Repetition segmentation

This standalone Python module and its tests are copied without modification from the team's September 8, 2026 commit `1bd8ad0`, authored by GitHub contributor `Ithomas13` in [muvaro2/FormFit](https://github.com/muvaro2/FormFit/commit/1bd8ad03a29dad296786cee00be2a0bc4e2604c1).

It parses and validates motion samples and segments exercise repetitions. It is a separate team component; it is not wired into the Swift app in this repository. Its original commit history is retained in the team history branches.

Run its existing synthetic-input tests from this directory:

```bash
python3 -m pip install pytest
python3 -m pytest -q
```

The tests included here use synthetic inputs. Comments also describe an evaluation on real recordings, but those recordings are not included, so that evaluation cannot be reproduced from this module alone.

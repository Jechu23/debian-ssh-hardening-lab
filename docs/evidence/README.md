# Evidence: Wazuh dashboard after each phase

Screenshots of the Wazuh Configuration Assessment view for the CIS Debian Linux 12 Benchmark v1.1.0.
The agent is the hardened VPN server. No IP addresses are shown.

| File | Scan (Perth time) | Passed | Failed | Not applicable | Score |
|---|---|---|---|---|---|
| `01-baseline-43pct.png` | 12 Oct 2026, 10:45 | 81 | 107 | 19 | 43% |
| `02-phase1-50pct.png` | 12 Oct 2026, 12:27 | 96 | 94 | 17 | 50% |
| `03-phase2-55pct.png` | 12 Oct 2026, 17:01 | 106 | 84 | 17 | 55% |
| `04-phase3-62pct.png` | 12 Oct 2026, 17:30 | 122 | 73 | 12 | 62% |
| `05-final-69pct.png` | 13 Oct 2026, 16:15 | 136 | 60 | 11 | 69% |

`00-initial-triage-baseline.png` is the first version of the decision register summary, made from the
baseline scan before any change. The current register is in the parent folder.

The phase 4 (66%) and tuning (68%) scans were exported as CSV and are reflected in the register summary.

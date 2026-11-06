# Key Levels + Gaps: test and issue log

Version under test: 1.0.7 (1.0.6 results below unless marked). Started 2026-11-03. Status changes only after a test on the LLM tab.

## Open

| # | Issue | Seen on | Status / next step |
|---|---|---|---|
| 2 | Memory error on CL 5m in Replay with gaps 10 + KL 12, at step 50 of 300. Defaults and gaps 10 alone pass 300 steps. | 1.0.4 | Re-test on 1.0.6. |
| 3 | 500-line limit. At default, KL 12, or gaps 10 without mid and quarters: 139 to 417 lines, every label has a line. At test settings (KL 12, gaps 5 with mid and quarters): about 500 lines, oldest line segments lost, every label still has a line. At gaps 10 with mid and quarters: 2 labels without a line on MNQ 1m. At everything max: 16 to 24 oldest NWOG labels without a line. | 1.0.7 | Measured on MNQ 1, 15, 240 and CL 240. Decision for the user: accept or reduce line use. |
| 5 | One odd blank reading on MNQ 5m in Replay. | 1.0.4 | Did not recur in the 1.0.6 Replay run; one more long-step check pending. |
| 8 | Memory limits exceeded in Replay when Key Levels are at 12. On 1.0.6: CL 5, 15, 60, 240 and GC, ZB 5 at all-max; CL 5 at KL 12 alone. Gaps at 10 alone pass on CL 5. | 1.0.6 | 1.0.7: no error in 20 of 20 long Replay runs, all failing charts now clean. Values on 1.0.7 still to be checked. |
| 10 | Weak test: the 1.0.6 long Replay run sampled status every 25 steps, and several runs ended still calculating. NG, SI, EURUSD, BTC, CL 1m passes on it do not count. | Test method | Rewritten and rerun on 1.0.7: all 20 reached a finished state. |
| 9 | Original Opening Gaps hits Memory limits exceeded in Replay: CL 3h 4 of 4 repeats, MNQ 1D 2 of 2, at test settings. It clears before the read. Combined had no error in those runs. | Opening Gaps 1.9.1 | Not a combined-script fault. User decision whether to change the original. |
| 6 | Key Levels has the same 1m shortfall (51 of 90 levels). | Key Levels 1.16.0 | User decision: port the 1m fix? |
| 7 | Why a plain global lower-timeframe request cuts monthly and weekly history on 1m. The fix was found by testing. | 1.0.6 | Cause unknown. The fix is confirmed by count only so far. |
| 11 | Blank price on oldest NWOG labels on 6 reads (MES 2, MGC 1, EURUSD 30S and 2, MNQ 5 and CL 5 in Replay). Originals have the price. | 1.0.7 | All 6 cleared on repeat. Probably read mid-redraw, not proven. Watch for repeats. |
| 12 | CL 3h in Replay: combined in error state once at +20, test settings. Error text not captured. | 1.0.7 | Did not repeat: five 20-step repeats and three 200-step runs clean. Error monitor saw no combined error in the later runs. |
| 13 | Replay, 60m chart: Midnight Open updated at the 07:00 bar, not 00:00. Original Key Levels does the same. | 1.0.7, Key Levels 1.16.0 | Seen once in smoke test. Timing stage measures it on 1m to 4h. |
| 17 | ES 1M in Replay (as of 2025-06-30), Opening Gaps off: combined Weekly Open 5964.00 (week of June 1), original Key Levels 6223.25 (week of June 29, correct). Reproduced 2 of 2. | 1.0.8, Key Levels 1.16.0 | Not combined-only: toggling Daily (a setting both have) makes the original show 5964 too. Any recalculation during Replay on 1M gives the stale week; Opening Gaps off only differed because the original wasn't recalculated. Same code in both. Same family as #14. |
| 18 | Original Opening Gaps on EURUSD 2h, 3h, 4h at max: older NWOGs dated on weekdays (01/21 Wed, 06/23 Tue, 09/22 Tue), fewer NDOGs. Combined shows consecutive Sundays; the 6 latest match exactly. | Opening Gaps 1.9.1 | Original's fault. User decision whether to fix the original. |

## Fixed or explained

| Issue | Outcome |
|---|---|
| Memory limits exceeded on CL 3h/4h | Fixed in 1.0.4, scan results copied only on the latest 500 bars. Older-bars re-test on 1.0.6 pending. |
| Monthly and weekly levels short on 1m | Fixed in 1.0.6: 12 at every count checked. 39 extra levels on 1m on all 10 markets in the match stage. |
| ES1! Daily Open 7610.25 vs 7584 in candle check | Value is correct: matches the 18:00 candle and the original. Most likely the check read candles before the 18:00 rollover (timing not proven). |
| One extra RTH gap at 15s on MGC, HG, SI, ZB (1.0.4) | Did not recur on 1.0.6 match stage. Cause not identified. |
| 1.0.5 literal timeframes | Did not help. Backed out. |
| 1.0.1 removal of log calls | Did not fix memory. Logs not restored. |
| CL Previous Day Close 96.09 vs 96.46 | Test artefact: session had closed, value rolled correctly. Original shows the same. |
| "Can't parse pine" on all studies | Test method: colors written back as ints. Now written as hex. |
| "resolve error" on chart | Test shell bug: symbol passed with resolution attached. Fixed. |
| Label Offset "no change" at 30 | Test artefact: label x is a point index. The option works. |
| 2 transient mismatches in 1.0.4 match run | Cleared on re-run. Watch for repeats on 1.0.6. |

## Still to test on 1.0.6

- [x] Match against originals, 170 charts (1.0.7)
- [x] Gaps against candles, 7 markets at test settings
- [x] Memory with older bars loaded, gaps at 10
- [ ] Bar Replay across markets and timeframes, plus long stepping
- [ ] Replay limits carried over but never tested: Midnight and Daily Open one bar late, 1m year reach, daily first frame, 1W and 1M
- [ ] Daily Open, Premarket, Midnight and period-closed check, including 15s, 30s and above 1h (the changed requests feed these)
- [ ] Every option on more charts: MNQ 1, 5, 15S, 240, 1D; CL 15, 240; SI 5; ZB 15; EURUSD 60; BTC 5; NG; HG
- [ ] Options in Replay
- [ ] Combinations: all at max per market, Take modes, random option sets
- [ ] Gaps 6 to 10 and opens and closes at 12 against candles, more markets and timeframes

## Results so far on 1.0.6

| Test | Result |
|---|---|
| Level counts at 12: 10 markets on 1m, MNQ 15S to 1M, CL 3h/4h | All as expected |
| Key Levels against candles, 12 markets, 1,248 values | 1,191 passed, 1 failed (#1), 56 skipped (last-trade closes on equity index markets) |
| Match against originals, 170 charts | No errors, no missing labels, no price differences. Differences: 1m extras (fix) and #3 on 24 charts. |
| Gaps against candles, 7 markets | 231 of 231 |
| Memory with older bars, gaps at 10, 11 charts, up to 23,591 bars | No errors |

## When testing ends

- Summary table for the user: tested, failed, detail, what to change
- key_levels_gaps README and root README row
- Restore LLM studies to defaults
- User can delete saved "1m Probe" and "Replay Probe"
- Commit and push only when asked

## Overnight queue, 2026-11-04

MNQ 5m options, then a smoke test of the new tools, then in order: older-bars and live match, Replay match on more charts, Replay timing, all-at-max Replay, options in Replay, combinations, all-at-max status on 14 symbols and 17 timeframes, candle checks on 9 timeframes, gaps across timeframes, then functional options on the other 12 charts, then cosmetic options.

MNQ 5m all options on 1.0.7: 234 options, 339 values, 0 errors, 0 differences from originals. Smoke test of new tools passed. Live log: queue.log in the session scratchpad.

## Overnight queue results, 1.0.7 (read 09:10 ET Nov 4; Q12 still running)

| Stage | Result |
|---|---|
| Q1 older bars, 9 charts | Clean except the known 39 extra levels on 1m |
| Q1b live, 3 charts | Clean except the 1m extras; CL 1m blank oldest NWOG on one read (#11) |
| Q2 Replay match, 22 charts | MNQ 1M at +4: June 2025 Monthly High and Close differ from original (new #14); blank NWOG on CL 1 and EURUSD 5 (#11) |
| Q2b after 300 steps, 6 charts | Clean |
| Q3 Replay timing | Midnight Open late above 1m, same as original (#13) |
| Q4 Replay at max, 14 charts | 12 clean; GC 60 and GC 240 transient memory error, 7 hits (new #15) |
| Q5 options in Replay, 2 charts | 0 errors, 0 differences |
| Q6 combinations, 84 | 0 errors; ZB 1m known extras; CL 60 random 6 one line start differs by 2h (new #16) |
| Q7 at max, 238 charts | All OK |
| Q8 candles, 108 charts | Only fails: Premarket H/L on CL and NG 15S to 240 (24), test found no morning 1m candles (-Infinity); needs re-run (new #17) |
| Q9 gaps 10 full, 63 charts | 0 missing, 0 extra; blank oldest NWOG on at least 4 (#11) |
| Q10 special cases | 0 errors; lbl_gap 20 hits the 500-line limit (#3) |
| Q11 functional, 12 charts, 1,476 runs | 0 errors; 3 Premarket Low differences on EURUSD 60 and BTC 5 while premarket was live, timing not proven (new #18) |
| Q12 cosmetic, 9 charts | CL 15 clean; MNQ 1 running; 7 not started |

Follow-ups after Q12: re-run #17; GC 60 and 240 in Replay at test settings and KL 12 (#15); repeat #14, #16.

## Follow-ups (Nov 4)
- #17 closed: CL and NG Premarket pass 104/104 on 12 charts; last night's fails were the test's candle fetch.
- #18 no differences on re-run outside premarket.
- #15: Gold 60m and 4h in Replay clean at default, test, KL 12, gaps 10, KL 12 + gaps 10 (150 steps). Everything at max repeats the memory error; SI and HG at max clean. Earlier stuck runs: killed runs kept running in the page; fixed in rp4.js.
- #14: MNQ monthly in Replay flips between runs in both scripts; not from combining.
- #11 cause: blank NWOG = label y lost when the 500-line limit deleted its line (lb.set_y(l.get_y1())). Fixed in 1.0.8 by storing the price. Needs re-test. Original Opening Gaps has the same code.
- #16 isolated to Previous (Monthly Close) = 5 on CL 60m; June close line starts 16:00 (no touch) vs original 14:00. Cause not proven; price right. Open.

## Final run on 1.0.8 (hash a4ccbe4f9198), Nov 4-5

| Test | Scope | Result |
|---|---|---|
| Blank NWOG prices | CL 30S, ZB 1, EURUSD 15S, BTC 15S, 8 reads each | 0 blanks; #11 fixed |
| Gaps at 10, mid and quarters | 9 markets x 7 timeframes | 63/63 match, no blanks |
| Older bars | 9 charts | Match; MNQ 1m has the 39 intended extras |
| Replay match, 300 steps | 6 charts | 0 differences |
| Replay everything at max | 14 charts | 12 clean; GC 60 and GC 240 memory errors (#9/#15) |
| Candle checks | 12 markets x 9 timeframes | 10,004 correct, 0 wrong, 496 not candle-checked (equity last-trade closes) |
| Functional options | 12 charts, 1,476 runs | 0 errors, 0 missing; MNQ 1m intended extras only |
| Cosmetic options | 9 charts, 1,944 runs | 0 errors, 0 missing; MNQ 1m intended extras only |
| Direct checks | Label Offset, Custom Hours with Market Custom, Show on Monthly | Work, match originals |

Copper 30 functional lost its CDP connection at 18:47 after 54 runs (cause unknown); the rest ran at 00:11 and were clean.
Not re-run on 1.0.8: full Replay 22 charts (Q2), Replay timing (Q3), options in Replay (Q5), random combos (Q6), 238-chart status (Q7), special cases (Q10).
Still open: #16 CL 60m line start, #13 Midnight Open Replay delay, #14 monthly Replay flips (both scripts), #3 500-line limit, #6 port fixes to Key Levels, #7 1m cut cause, #9 OG memory errors in Replay.

## Completion run on 1.0.8, Nov 5

| Test | Scope | Result |
|---|---|---|
| NG 15, ZN 180 timeout repeat | 3 repeats each at max | Clean, no timeout |
| Values and label spacing at max | 14 markets x 17 timeframes (238) | 0 errors, 0 price differences, 0 stacked, 0 under-spacing, min 17.8 px. Extras: Monthly Open/Close Previous beyond the original's cap of 5 (14), older gaps the original can't show on short timeframes. Missing: EURUSD 2h/3h/4h only, see #18 |
| Values and label spacing at defaults | 238 charts | 0 errors, 0 missing, 0 extra, 0 price differences, 0 stacked, 0 under-spacing |
| Gap prices vs candles | 12 CME markets + EURUSD + BTC | 254 checked, 254 correct, 0 wrong; older gaps without loaded 60m bars not checked |
| Replay match, 300 steps | 22 charts (7 first run, 15 after the loop fix) | 0 differences; original Opening Gaps memory errors on 4 (#9) |
| Functional options | 9 more charts, 1,107 runs | 0 errors, 0 differences |
| Cosmetic options, batched | 11 more charts, 3 passes x 132 settings | 0 differences; batching checked on ZB 15 (matches one-at-a-time) and with a planted mismatch (caught) |
| Options in Replay | 15 more charts, 1,845 runs | 14 clean; ES 1M: #17 |

Test-method faults found and fixed today: Replay step loop waited at the end of data and left loops running (1W/1D start dates too late); 150 ms waits throttled to 1 s in a hidden tab; gap checker assumed the CME 18:00 open for EURUSD and BTC.

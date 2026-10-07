# SzCORE seizure annotations (derivative)

Seizure annotations of the BIDS Siena Scalp EEG Database v1.0.0, the version of this data set released by Jonathan Dan and
Paolo Detti for the SzCORE seizure-detection benchmark (Zenodo, doi:10.5281/zenodo.10640762, published 2024-02-09; framework:
Dan et al. 2024, Epilepsia 66(S3):14-24, doi:10.1111/epi.18113). They are included because they give every seizure a
standardised SzCORE/HED event type (ILAE 2017 based: `sz_foc_ia` focal impaired awareness, `sz_foc_a` focal aware,
`sz_foc_f2b` focal to bilateral tonic-clonic) and the format used by SzCORE scoring tools.

## Files
- `sub-PNxx/eeg/sub-PNxx_task-szMonitoring_run-NN_events.tsv`: the SzCORE `events.tsv` files, **byte-identical** to the Zenodo
  release. Only the names changed: SzCORE `sub-00/ses-01/..._run-00_events.tsv` became this dataset's `sub-PN00/..._run-01_events.tsv`.
  Recordings were matched by EDF start time and length (all 41 match one-to-one; SzCORE numbers runs from 00, and its runs 00/01
  of sub-10 are this dataset's runs 02/01). `szcore_file_map.tsv` lists every pair and the original PhysioNet EDF name.
- Columns (SzCORE format): `onset`, `duration` (s, from the start of the same recording), `eventType`, `confidence`, `channels`,
  `dateTime` (de-identified date as in the EDF header, real time of day), `recordingDuration`. Files without a seizure carry one
  `bckg` row. `task-szMonitoring_events.json` is SzCORE's `events.json`, unchanged (HED map of the event types).
- `channels` is the same per-patient channel set for every seizure of a patient (it follows the lesion localisation), not
  the release's per-seizure electrode list.

## Agreement with the raw annotations of this data set
Same 14 subjects, 41 recordings and 47 seizures (47 in the raw `events.tsv`). Onset and duration agree within 0.5 s for 41 of 47
seizures. The 6 differences:
- PN00 run-03: the release's seizure end 19.29.29 is after the end of the recording. SzCORE reads it as 18.29.29 (60 s); the raw
  events now use the same value (2026-10-07), with the release string kept in `release_end_time`.
- PN05 run-02: SzCORE takes the onset from the release's registration start (06.01.23), this data set from the EDF header start
  (06.01.13): 6836 s vs 6846 s. Neither can be ruled out. In the other release files, the registration end lies 0 s or 20 s
  before the end of the EDF. Here it lies 10 s before it when counted from the EDF header start, and 20 s before it when counted
  from the release start.
- PN10 run-03: the release gives two end times ("11.41.04 opure 11.40.43"); SzCORE uses the second (30 s), the raw events the first (51 s).
- PN10 run-04: the release gives a clinical (15.43.53) and an electrical onset (15.43.59); SzCORE uses the electrical one
  (7841 s, 63 s), the raw events the clinical one (7835 s, 69 s).
- PN14 run-03: **SzCORE places the seizure at 17540 s, 3 h too late.** It uses the release's registration start 16.17.45, but
  the EDF header start 19.17.45 plus the EDF length (41995 s) gives exactly the release's registration end 06.57.40, so 16.17.45
  is a typo. The raw events (6740 s) are correct. The SzCORE file is kept unchanged here; correct it before using it for
  scoring.
- PN10 runs 01/02: not a difference in content; SzCORE orders these two files differently (see above).

## Licence
The Zenodo record's licence field is Creative Commons Attribution 4.0 International; the release README states the Open Data Commons
Attribution License v1.0. Both permit redistribution with attribution. Cite the Zenodo record, the SzCORE paper and Detti (2020).

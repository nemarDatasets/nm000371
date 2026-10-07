# Dataset of Speech Production in intracranial Electroencephalography (SingleWordProductionDutch-iBIDS)

## Overview
Stereo-EEG (sEEG) from 10 Dutch-speaking patients with pharmaco-resistant epilepsy (5 female, 5 male, age 16-50 years,
mean 32) who read aloud 100 Dutch words shown one at a time on a laptop screen (2 s word, 1 s fixation cross; about 300 s
per participant), while intracranial EEG and the participant's speech audio were recorded simultaneously.
1103 recorded contacts in total, covering cortical and subcortical (incl. deep) structures.

Original publication: Verwoert, M., Ottenhoff, M.C., Goulis, S., Colon, A.J., Wagner, L., Tousseyn, S., van Dijk, J.P.,
Kubben, P.L., Herff, C. Dataset of Speech Production in intracranial Electroencephalography. Scientific Data 9, 434
(2022). https://doi.org/10.1038/s41597-022-01542-9
Original data record: https://doi.org/10.17605/OSF.IO/NRGX6 (OSF project "Dataset of Speech Production in intracranial
Electroencephalography", license CC-By Attribution 4.0 International). Code: https://github.com/neuralinterfacinglab/SingleWordProductionDutch

## Participants / cohort (data descriptor, Methods 'Participants' and Table 1)
- 10 participants with pharmaco-resistant epilepsy (mean age 32, range 16-50; 5 male, 5 female), all native speakers of Dutch,
  implanted with sEEG as part of their clinical therapy at the Academic Center for Epileptology Kempenhaeghe/Maastricht UMC+
  (The Netherlands); electrode locations purely clinical. 1317 contacts implanted, 1103 recorded (Table 1).
- Implanted hemispheres (from the released electrode tables): left only 2 (sub-08, sub-09), right only 2 (sub-04, sub-10),
  bilateral 6. 5-19 shafts per participant.
- Not reported by the source: handedness, seizure onset zone, aetiology, epilepsy duration, medication, recording dates/years.
- Ethics: approved by the Institutional Review Boards of Maastricht University and Epilepsy Center Kempenhaeghe; written
  informed consent; recording supervised by experienced healthcare staff.

## Task (data descriptor, 'Experimental design')
Participants read aloud words shown on a laptop screen: one random word from the stimulus library (Dutch IFA corpus extended
with the numbers one to ten in word form) was shown for 2 s, during which the participant read it aloud once, followed by a
1 s fixation cross; 100 words, about 300 s per participant. Instruction (source sidecar): "Speak the word presented on the
screen out-loud". `events.tsv` gives every word and fixation with the word text in `value`.

## Acquisition (data descriptor)
- Electrodes: Dixi Medical Microdeep sEEG shafts (platinum-iridium, 0.8 mm diameter, 2 mm contacts, 1.5 mm spacing, 5-18 contacts per shaft); locations purely clinical.
- Amplifiers: two or more Micromed SD LTM amplifiers (64 channels each). Contacts referenced to a common white-matter contact.
- Recorded at 1024 Hz or 2048 Hz and downsampled by the authors to 1024 Hz (all files here: 1024 Hz). No further filtering by us.
- Audio: notebook microphone at 48 kHz, pitch-shifted by the authors by a random constant offset of 1-3 semitones (up or down) per participant to protect anonymity (LibRosa). Neural, audio and stimulus streams were synchronised with LabStreamingLayer.
- Task: words from the Dutch IFA corpus extended with the numbers one to ten; each word shown 2 s, then a 1 s fixation cross, 100 words.
- Electrode localisation: img_pipe (pre-implant T1 MRI co-registered with post-implant CT); anatomical labels (channels.tsv 'description') from the Destrieux atlas. Coordinates in native ACPC space (mm).
- Ethics: approved by the Institutional Review Boards of Maastricht University and Epilepsy Center Kempenhaeghe; written informed consent.

## Preprocessing applied by the source
Only the authors' downsampling to 1024 Hz (where recorded at 2048 Hz; method not reported) and the pitch shift of the audio.
The descriptor's technical validation (70-170 Hz envelope, 50 Hz harmonics band-stop) is analysis code, not applied to the files.
The authors report no significant acoustic contamination of the neural data (Roussel et al. method, p > 0.01, all participants).

## Files (what is in this package)
- `sub-XX/ieeg/*_ieeg.vhdr/.vmrk/.eeg`: the NWB 'iEEG' stream of each participant. Signals: the NWB 'iEEG' acquisition stream (float64, microvolts) re-encoded losslessly as BrainVision integer codes x resolution (every source sample is an exact integer multiple of the stated resolution); per participant: sub-01: INT_16 x 0.09765625 uV; sub-02: INT_16 x 0.09765625 uV; sub-03: INT_16 x 0.09765625 uV; sub-04: INT_16 x 0.09765625 uV; sub-05: INT_16 x 0.09765625 uV; sub-06: the original NWB file is the raw file (its float64 values are not on any integer grid, consistent with the authors' downsampling from 2048 Hz - not verified; no lossless BrainVision encoding exists; this NWB also contains the Audio and Stimulus streams); sub-07: INT_16 x 0.09765625 uV; sub-08: INT_16 x 0.09765625 uV; sub-09: the original NWB file is the raw file (its float64 values are not on any integer grid, consistent with the authors' downsampling from 2048 Hz - not verified; no lossless BrainVision encoding exists; this NWB also contains the Audio and Stimulus streams); sub-10: INT_16 x 0.09765625 uV.
  Verified per participant: integer code x resolution equals the source float64 value for every sample; same sample count, channel order, 1024 Hz.
- `*_channels.tsv`, `*_events.tsv`, `*_space-ACPC_electrodes.tsv`, `*_coordsystem.json`: from the source iBIDS release (electrodes/coordsystem renamed without the task entity). Sidecars enriched from the data descriptor (amplifier and electrode make; the source sidecar's 'BrainProducts' manufacturer field is kept as SourceManufacturerField and replaced by Micromed as stated in the paper).
- `participants.tsv`: age and sex from the source; contacts implanted/recorded from Table 1 of the paper; implanted hemisphere(s)
  and shaft counts derived from the released electrode tables (see participants.json). Handedness is not reported (n/a).
- `derivatives/freesurfer/sub-XX/`: FreeSurfer outputs released by the authors (pial meshes, skull-stripped brain, Destrieux and white-matter parcellations), byte-identical.
- `sourcedata/osf-nrgx6/`: the original OSF zip (byte-identical, checksums in acquisition_receipt.json) and its unpacked content, including the original
  `*_ieeg.nwb` files. **The participants' pitch-shifted speech audio (48 kHz, NWB acquisition 'Audio') and the per-sample stimulus word stream
  ('Stimulus') are in these NWB files (and in the raw NWB files of the participants listed above as NWB)**, exactly as openly published by the authors under CC BY 4.0; read them with pynwb or h5py
  (`acquisition/Audio/data` + `acquisition/Audio/timestamps`, LSL clock). The iEEG timestamps of the NWB files are the LSL timestamps;
  the BrainVision files assume the nominal 1024 Hz rate stated by the source.

## Known caveats / notes
- No recording dates are present (NWB session_start_time is the placeholder 2020-01-01T12:00).
- Events: onset in seconds from the first iEEG sample; 'sample' is the 0-based iEEG sample index from the source.
- License: CC BY 4.0 (OSF record). Cite the paper and the OSF record when using these data.

## How to load
```python
import mne_bids
bp = mne_bids.BIDSPath(root=".", subject="01", task="wordProduction", datatype="ieeg")
raw = mne_bids.read_raw_bids(bp)  # BrainVision; sub-06 and sub-09 are NWB (read with pynwb)
```

## Citation
Verwoert M. et al. (2022) Sci Data 9:434, https://doi.org/10.1038/s41597-022-01542-9 ; data https://doi.org/10.17605/OSF.IO/NRGX6 .

## Provenance
OSF project nrgx6 (created 2022-03-25), file SingleWordProductionDutch-iBIDS.zip; data descriptor full text PMC9307753.
Metadata enriched 2026-10-07 (participants cohort text, implanted hemispheres/shaft counts from the released electrode tables,
task/acquisition sections, corrected SourceManufacturerField note).

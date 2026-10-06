# Dataset of Speech Production in intracranial Electroencephalography (SingleWordProductionDutch-iBIDS)

Stereo-EEG (sEEG) from 10 Dutch-speaking patients with pharmaco-resistant epilepsy (5 female, 5 male, age 16-50 years,
mean 32) who read aloud 100 Dutch words shown one at a time on a laptop screen (2 s word, 1 s fixation cross; about 300 s
per participant), while intracranial EEG and the participant's speech audio were recorded simultaneously.
1103 recorded contacts in total, covering cortical and subcortical (incl. deep) structures.

Original publication: Verwoert, M., Ottenhoff, M.C., Goulis, S., Colon, A.J., Wagner, L., Tousseyn, S., van Dijk, J.P.,
Kubben, P.L., Herff, C. Dataset of Speech Production in intracranial Electroencephalography. Scientific Data 9, 434
(2022). https://doi.org/10.1038/s41597-022-01542-9
Original data record: https://doi.org/10.17605/OSF.IO/NRGX6 (OSF project "Dataset of Speech Production in intracranial
Electroencephalography", license CC-By Attribution 4.0 International). Code: https://github.com/neuralinterfacinglab/SingleWordProductionDutch

## Recording (from the data descriptor)
- Electrodes: Dixi Medical Microdeep sEEG shafts (platinum-iridium, 0.8 mm diameter, 2 mm contacts, 1.5 mm spacing, 5-18 contacts per shaft); locations purely clinical.
- Amplifiers: two or more Micromed SD LTM amplifiers (64 channels each). Contacts referenced to a common white-matter contact.
- Recorded at 1024 Hz or 2048 Hz and downsampled by the authors to 1024 Hz (all files here: 1024 Hz). No further filtering by us.
- Audio: notebook microphone at 48 kHz, pitch-shifted by the authors by a random constant offset of 1-3 semitones (up or down) per participant to protect anonymity (LibRosa). Neural, audio and stimulus streams were synchronised with LabStreamingLayer.
- Task: words from the Dutch IFA corpus extended with the numbers one to ten; each word shown 2 s, then a 1 s fixation cross, 100 words.
- Electrode localisation: img_pipe (pre-implant T1 MRI co-registered with post-implant CT); anatomical labels (channels.tsv 'description') from the Destrieux atlas. Coordinates in native ACPC space (mm).
- Ethics: approved by the Institutional Review Boards of Maastricht University and Epilepsy Center Kempenhaeghe; written informed consent.

## What is in this package
- `sub-XX/ieeg/*_ieeg.vhdr/.vmrk/.eeg`: the NWB 'iEEG' stream of each participant. Signals: the NWB 'iEEG' acquisition stream (float64, microvolts) re-encoded losslessly as BrainVision integer codes x resolution (every source sample is an exact integer multiple of the stated resolution); per participant: sub-01: INT_16 x 0.09765625 uV; sub-02: INT_16 x 0.09765625 uV; sub-03: INT_16 x 0.09765625 uV; sub-04: INT_16 x 0.09765625 uV; sub-05: INT_16 x 0.09765625 uV; sub-06: the original NWB file is the raw file (its float64 values are not on any integer grid, consistent with the authors' downsampling from 2048 Hz - not verified; no lossless BrainVision encoding exists; this NWB also contains the Audio and Stimulus streams); sub-07: INT_16 x 0.09765625 uV; sub-08: INT_16 x 0.09765625 uV; sub-09: the original NWB file is the raw file (its float64 values are not on any integer grid, consistent with the authors' downsampling from 2048 Hz - not verified; no lossless BrainVision encoding exists; this NWB also contains the Audio and Stimulus streams); sub-10: INT_16 x 0.09765625 uV.
  Verified per participant: integer code x resolution equals the source float64 value for every sample; same sample count, channel order, 1024 Hz.
- `*_channels.tsv`, `*_events.tsv`, `*_space-ACPC_electrodes.tsv`, `*_coordsystem.json`: from the source iBIDS release (electrodes/coordsystem renamed without the task entity). Sidecars enriched from the data descriptor (amplifier and electrode make; the source sidecar's 'BrainProducts' manufacturer field is kept as SourceManufacturerField and replaced by Micromed as stated in the paper).
- `participants.tsv`: age and sex from the source; contacts implanted/recorded from Table 1 of the paper. Handedness is not reported (n/a).
- `derivatives/freesurfer/sub-XX/`: FreeSurfer outputs released by the authors (pial meshes, skull-stripped brain, Destrieux and white-matter parcellations), byte-identical.
- `sourcedata/osf-nrgx6/`: the original OSF zip (byte-identical, checksums in acquisition_receipt.json) and its unpacked content, including the original
  `*_ieeg.nwb` files. **The participants' pitch-shifted speech audio (48 kHz, NWB acquisition 'Audio') and the per-sample stimulus word stream
  ('Stimulus') are in these NWB files (and in the raw NWB files of the participants listed above as NWB)**, exactly as openly published by the authors under CC BY 4.0; read them with pynwb or h5py
  (`acquisition/Audio/data` + `acquisition/Audio/timestamps`, LSL clock). The iEEG timestamps of the NWB files are the LSL timestamps;
  the BrainVision files assume the nominal 1024 Hz rate stated by the source.

## Notes
- No recording dates are present (NWB session_start_time is the placeholder 2020-01-01T12:00).
- Events: onset in seconds from the first iEEG sample; 'sample' is the 0-based iEEG sample index from the source.
- License: CC BY 4.0 (OSF record). Cite the paper and the OSF record when using these data.

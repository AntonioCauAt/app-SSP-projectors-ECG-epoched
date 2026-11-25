<h1 align="center">Compute ECG artifact SSP projectors - epoched data</h1>

<p align="center">
  Computes signal-space projection (SSP) vectors for ECG (heartbeat) artifacts with MNE-Python compute_proj_ecg and outputs the projectors, topomap and ECG evoked figures and an HTML report.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.824"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.824-blue.svg" alt="Run on brainlife.io"></a>
</p>

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Computes signal-space projection (SSP) vectors for ECG (heartbeat) artifacts with MNE-Python compute_proj_ecg and outputs the projectors, topomap and ECG evoked figures and an HTML report.**

Intended to detect heartbeats on the ECG channel (or the channel given in ch_name), epoch the data around them and compute ECG SSP projectors separately for gradiometers, magnetometers and EEG with mne.preprocessing.compute_proj_ecg. Use it to build projectors that remove cardiac artifacts from MEG/EEG. As committed, the code reads the config key 'mne' (the input provides 'epo'), calls mne.io.read_epochs and passes epochs to a function that expects continuous raw data, so it is unlikely to run.

| | |
|---|---|
| **Inputs** | `neuro/meeg/mne/epochs` |
| **Outputs** | `neuro/meeg/mne/projection` · `generic/image/png` |
| **Container** | `brainlife/mne:1.2.1` |
| **Resources** | 1 CPUs |
| **Run it** | [doi.org/10.25663/brainlife.app.824](https://doi.org/10.25663/brainlife.app.824) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `fif` | `neuro/meeg/mne/epochs` | yes | Epoched MEG/EEG data (-epo.fif). |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `out_dir` | `neuro/meeg/mne/projection` | SSP projector computed based on ECG. |
| `out_figs` | `generic/image/png` | Topographic maps of the ECG projectors and joint plots of the average ECG artifact. |

<details>
<summary><b>Parameters</b> (22)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `ch_name` | string |  | Channel to use for ECG detection (Required if no ECG found). |
| `n_grad` | number | `2` | Number of SSP vectors for gradiometers. |
| `n_mag` | number | `2` | Number of SSP vectors for magnetometers. |
| `n_eeg` | number | `2` | Number of SSP vectors for EEG. |
| `tmin` | number | `-0.2` | Time before event in seconds. |
| `tmax` | number | `0.4` | Time after event in seconds. |
| `l_freq` | number | `1` | Filter low cut-off frequency for the data channels in Hz. |
| `h_freq` | number | `35` | Filter high cut-off frequency for the data channels in Hz. |
| `average` | boolean | `true` | Compute SSP after averaging. Default is True. |
| `filter_length` | string | `10s` | Number of taps to use for filtering. |
| `avg_ref` | boolean | `false` | Add EEG average reference proj. |
| `no_proj` | boolean | `false` | Exclude the SSP projectors currently in the fiff file. |
| `event_id` | number | `999` | ID to use for events. |
| `ecg_l_freq` | number | `5` | Low pass frequency applied to the ECG channel for event detection. |
| `ecg_h_freq` | number | `35` | High pass frequency applied to the ECG channel for event detection. |
| `tstart` | number | `0` | Start artifact detection after tstart seconds. |
| `qrs_threshold` | string | `auto` | Between 0 and 1. qrs detection threshold. Can also be “auto” to automatically choose the threshold that generates a reasonable number of heartbeats (40-160 beats / min). |
| `filter_method` | string | `fir` | Method for filtering (‘iir’ or ‘fir’). |
| `copy` | boolean | `true` | If False, filtering raw data is done in place. Defaults to True. |
| `return_drop_log` | boolean | `true` | If True, return the drop log. |
| `meg` | string | `separate` | Can be ‘separate’ (default) or ‘combined’ to compute projectors for magnetometers and gradiometers separately or jointly. If ‘combined’, n_mag == n_grad is required and the number of projectors computed for MEG will be n_mag. |
| `iir_params` | string |  | Dictionary of parameters to use for IIR filtering. See mne.filter.construct_iir_filter for details. If iir_params is None and method=”iir”, 4th order Butterworth will be used. |

</details>

<!-- brainlife-app:end -->

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.824](https://doi.org/10.25663/brainlife.app.824), choose `neuro/meeg/mne/epochs` from your project, set the parameters and press **Execute**. The results are saved to your project as `neuro/meeg/mne/projection`, `generic/image/png`.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/) (the App runs in `brainlife/mne:1.2.1`), [`jq`](https://jqlang.github.io/jq/).

1. Clone this repository.
2. Create `config.json` with the paths to your inputs and the parameters:

   ```json
   {
       "epo": "/path/to/fif/epo",
       "ch_name": "",
       "n_grad": 2,
       "n_mag": 2,
       "n_eeg": 2,
       "tmin": -0.2,
       "tmax": 0.4,
       "l_freq": 1,
       "h_freq": 35,
       "average": true,
       "filter_length": "10s",
       "avg_ref": false,
       "no_proj": false,
       "event_id": 999,
       "ecg_l_freq": 5,
       "ecg_h_freq": 35,
       "tstart": 0,
       "qrs_threshold": "auto",
       "filter_method": "fir",
       "copy": true,
       "return_drop_log": true,
       "meg": "separate",
       "iir_params": ""
   }
   ```

3. Run `./main`.

## Citing

If you use this App, please cite it, the software it builds on and brainlife.io:

- Compute ECG artifact SSP projectors - epoched data. brainlife.io App. [doi:10.25663/brainlife.app.824](https://doi.org/10.25663/brainlife.app.824)
- Gramfort A. et al. MEG and EEG data analysis with MNE-Python. Frontiers in Neuroscience 7:267 (2013). [doi:10.3389/fnins.2013.00267](https://doi.org/10.3389/fnins.2013.00267)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- Guiomar Niso
- Antonio Caulín
- Maximilien Chaumon

## Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io, which runs this App, is publicly funded by Wellcome, the U.S. National Institutes of Health and the U.S. National Science Foundation; acknowledging that support helps keep it running.</sub>

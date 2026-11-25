

# SSP projectors ECG for epoched data

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Computes signal-space projection (SSP) vectors for ECG (heartbeat) artifacts with MNE-Python compute_proj_ecg and outputs the projectors, topomap and ECG evoked figures and an HTML report.**

Intended to detect heartbeats on the ECG channel (or the channel given in ch_name), epoch the data around them and compute ECG SSP projectors separately for gradiometers, magnetometers and EEG with mne.preprocessing.compute_proj_ecg. Use it to build projectors that remove cardiac artifacts from MEG/EEG. As committed, the code reads the config key 'mne' (the input provides 'epo'), calls mne.io.read_epochs and passes epochs to a function that expects continuous raw data, so it is unlikely to run.

| | |
|---|---|
| 📥 **Inputs** | `neuro/meeg/mne/epochs` |
| 📤 **Outputs** | `neuro/meeg/mne/projection` · `generic/image/png` |
| 🐳 **Container** | `brainlife/mne:1.2.1` |
| ⚙️ **Resources** | 1 CPUs |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.824](https://doi.org/10.25663/brainlife.app.824) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `fif` | `neuro/meeg/mne/epochs` | yes | Epoched MEG/EEG data (-epo.fif). |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `out_dir` | `neuro/meeg/mne/projection` | SSP projector computed based on ECG. |
| `out_figs` | `generic/image/png` | Topographic maps of the ECG projectors and joint plots of the average ECG artifact. |

<details>
<summary><b>⚙️ Parameters</b> (22)</summary>

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

📚 **References used by this App:**
- Avesani P. et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. Sci Data 6, 69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)
- Gramfort A. et al. MEG and EEG data analysis with MNE-Python. Frontiers in Neuroscience 7:267 (2013). [doi:10.3389/fnins.2013.00267](https://doi.org/10.3389/fnins.2013.00267)

<!-- brainlife-app:end -->

[![Abcdspec-compliant](https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg)](https://github.com/brain-life/abcd-spec)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.672-blue.svg)](https://doi.org/10.25663/brainlife.app.672)

Brainlife App to compute SSP (signal-space projection) vectors for ECG artifacts for epoched data using `mne.preprocessing.compute_proj_ecg` function.



1) Input file is:
    * `meg/fif` meg data file
    
2) Input parameters are:
* 
* `tmin` Time before event in seconds.

* `tmax`Time after event in seconds.

* `n_grad`Number of SSP vectors for gradiometers.

* `n_mag` Number of SSP vectors for magnetometers.

* `n_eeg` Number of SSP vectors for EEG.

* `l_freq` Filter low cut-off frequency for the data channels in Hz.

* `h_freq` Filter high cut-off frequency for the data channels in Hz.

* `average` Compute SSP after averaging. Default is True.

* `filter_length` Number of taps to use for filtering.

* `n_jobs` The number of jobs to run in parallel. If -1, it is set to the number of CPU cores. Requires the joblib package. None (default) is a marker for ‘unset’ that will be interpreted as n_jobs=1 (sequential execution) unless the call is performed under a joblib.parallel_backend() context manager that sets another value for n_jobs.

* `ch_name` Channel to use for ECG detection (Required if no ECG found).

* `reject` Epoch rejection configuration.

* `flat` Epoch flat configuration (see Epochs).

* `bads` List with (additional) bad channels.

* `avg_ref` Add EEG average reference proj.

* `no_proj` Exclude the SSP projectors currently in the fiff file.

* `event_id` ID to use for events.

* `ecg_l_freq` Low pass frequency applied to the ECG channel for event detection.

* `ecg_h_freq` High pass frequency applied to the ECG channel for event detection.

* `tstart` Start artifact detection after tstart seconds.

* `qrs_threshold` Between 0 and 1. qrs detection threshold. Can also be “auto” to automatically choose the threshold that generates a reasonable number of heartbeats (40-160 beats / min).

* `filter_method` Method for filtering (‘iir’ or ‘fir’).

* `iir_params`Dictionary of parameters to use for IIR filtering. See mne.filter.construct_iir_filter for details. If iir_params is None and method=”iir”, 4th order Butterworth will be used.

* `copy` If False, filtering raw data is done in place. Defaults to True.

* `return_drop_log` If True, return the drop log.

* `meg` Can be ‘separate’ (default) or ‘combined’ to compute projectors for magnetometers and gradiometers separately or jointly. If ‘combined’, n_mag == n_grad is required and the number of projectors computed for MEG will be n_mag.

3) Ouput files are:
    * `ECG projectors`
    * a plot of the ECG projectors
    * a html report
   

## Authors
- Guiomar Niso
- Antonio Caulín (antoniocaulinatienzar@gmail.com)
- Maximilien Chaumon (maximilien.chaumon@icm-institute.org)

## Citations
We kindly ask that you cite the following articles when publishing papers and code using this code. 

*- brainlife.io Publishing and Apps:*

Avesani, P., McPherson, B., Hayashi, S. et al. **The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services**. Sci Data 6, 69 (2019). https://doi.org/10.1038/s41597-019-0073-y

*- MNE-Python package:* 

Gramfort A, Luessi M, Larson E, Engemann DA, Strohmeier D, Brodbeck C, Goj R, Jas M, Brooks T, Parkkonen L, and Hämäläinen MS.  **MEG and EEG data analysis with MNE-Python**. Frontiers in Neuroscience, 7(267):1–13, 2013. https://doi.org/10.3389/fnins.2013.00267

## Funding Acknowledgement
brainlife.io is publicly funded and for the sustainability of the project it is helpful to Acknowledge the use of the platform. We kindly ask that you acknowledge the funding below in your publications and code reusing this code.

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://grantome.com/grant/NIH/R01-EB030896-01)


#### MIT Copyright (c) 2021 brainlife.io The University of Texas at Austin and Indiana University

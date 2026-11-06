# DAS-Based-Denoise

Dataset access and experimental indices for DM-NPGNet, a background-guided approach to unpaired denoising of underwater distributed acoustic sensing (DAS) signals.

Associated manuscript: **Learning background representations for unpaired denoising of underwater distributed acoustic sensing signals**.

## Data access

The recorded data are publicly available on Zenodo:

**DAS-Noise-dataset**  
DOI: https://doi.org/10.5281/zenodo.23993589

The release contains recordings from two freshwater acquisition settings:

| Archive | Acquisition setting | Scene label in the indices |
|---|---|---|
| `室内水池.zip` | Indoor pool | `室内水池` |
| `眼镜湖内.zip` | Artificial lake (Yanjing Lake) | `眼镜湖内` |

Both settings contain independently acquired background recordings and recordings of deliberately generated events. The event classes are paddling (`划水`), knocking (`敲击`), and propeller disturbance (`螺旋桨`).

Background recordings were acquired without the deliberately generated target events. Event recordings contain both target responses and background contributions. The dataset does not provide synchronized clean event references or one-to-one background–event pairs.

## Repository contents

| File | Description |
|---|---|
| `noise_index.xlsx` | Background-recording index for representation learning and background-prior extraction |
| `noisy_index.xlsx` | Event-recording index, including event labels and the training/validation assignments used in the study |

Each row identifies one channel segment. The indices specify a sampling rate of 2048 Hz and a segment duration of 5 s, corresponding to 10,240 samples.

## Index fields

| Field | Description |
|---|---|
| `abs_path` | Original file path in the authors' computing environment; replace its dataset-root prefix before local use |
| `scene` | Acquisition setting |
| `behavior` | Event class; present in `noisy_index.xlsx` |
| `split` | Experimental assignment: `train` or `val` for event recordings; all background entries are marked `train` |
| `channel` | Zero-based channel position within a multichannel event file |
| `channel_id` | Channel identifier parsed from a background-file name; present in `noise_index.xlsx` |
| `file_timestamp` | Recording-file timestamp |
| `segment_idx` | Segment index within the recording |
| `segment_start_time`, `segment_end_time` | Segment time boundaries |
| `segment_start_sample`, `segment_end_sample` | Segment sample boundaries |
| `duration_sec` | Segment duration in seconds |
| `sample_rate` | Sampling rate used in the study, in Hz |
| `num_samples` | Number of samples in the indexed segment |
| `file_num_samples` | Number of samples per channel in the source file |
| `num_channels` | Number of channels in an event file; present in `noisy_index.xlsx` |

The `channel` and `channel_id` fields serve different purposes. In particular, the channel identifier in the name of a single-channel background file should not be used as its within-file channel position.

## Experimental partition

The event-recording partition is provided in the `split` column of `noisy_index.xlsx`.

| Setting | Training files | Validation files | Training channel-segment entries | Validation channel-segment entries |
|---|---:|---:|---:|---:|
| Indoor pool | 138 | 36 | 2,640 | 696 |
| Artificial lake | 779 | 195 | 20,112 | 5,032 |
| Total | 917 | 231 | 22,752 | 5,728 |

The background index contains 1,292 pool entries and 22,401 lake entries, for a total of 23,693. All background entries are assigned to training.

Event files were grouped by setting and event class, ordered chronologically, and divided into training and validation subsets. Approximately the last 20% of files were assigned to validation, with two boundary files excluded as a buffer. All channels and segments from a recording file remain in the same subset.

Use the released assignments when reproducing the reported experiments. Do not randomly repartition individual rows. Files outside the experimental indices should not be added to the reported training or validation subsets.

Both acquisition settings contributed to model training. Validation data were used for denoising-checkpoint selection and performance reporting. The released partition does not include an additional independent test set or an unseen-environment evaluation.

The entry counts above describe indexed channel segments, not independent experimental repetitions.

## Using the indices locally

1. Download both archives from Zenodo and extract them while preserving their internal directory structure.
2. Download `noise_index.xlsx` and `noisy_index.xlsx` from this repository.
3. Replace the original dataset-root prefix in `abs_path` with your local extraction directory. Preserve the remaining relative path, including the scene, recording type, subdirectories, and filename.
4. Read the channel and sample interval specified by each index row.
5. Retain the original `split` assignments for comparisons with the manuscript.

For example, an indexed path beginning with:

    /home/cwy/Denoise/噪声数据集/室内水池/...

should be mapped to:

    <local_dataset_root>/室内水池/...

Use the sampling rate specified in the indices. The recordings represent fiber vibration responses and should not be interpreted as calibrated acoustic-pressure measurements.

## Code availability

This repository currently provides the experimental data indices. The model implementation and accompanying reproduction instructions will be released in this repository upon publication of the associated article.

## License

The dataset deposited on Zenodo is licensed under the Creative Commons Attribution 4.0 International license (CC BY 4.0), as specified in the Zenodo record:

https://creativecommons.org/licenses/by/4.0/

The software license will be specified with the code release.

## Citation

Please cite the dataset DOI when using these recordings:

Cai, W. (2026). DAS-Noise-dataset [Data set]. Zenodo. https://doi.org/10.5281/zenodo.23993589

For comparisons with the manuscript, also identify the version or commit of the experimental indices used.

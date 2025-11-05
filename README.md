Here is the complete `README.md` file, designed to satisfy all submission instructions and clearly document the successful pipeline and troubleshooting conducted during the assignment.

-----

# 🎙️ Assignment 1: Forced Alignment Pipeline using Montreal Forced Aligner (MFA)

## Objective

The objective of this assignment was to set up a complete **Forced Alignment** pipeline using the **Montreal Forced Aligner (MFA)**, apply it to a provided corpus of speech audio and transcripts, and generate precise, time-aligned phonetic and word-level annotations (TextGrid files).

-----

## 1\. ⚙️ Environment Setup and Installation (Tasks 1 & 2)

This section details the robust installation method, which includes necessary troubleshooting for common dependency and path issues on Windows.

### A. Prerequisites

  * **Anaconda/Miniconda**
  * **FFmpeg:** Required for audio format conversion.
  * **Praat:** Required for visual inspection of the final TextGrid output.

### B. Installation Commands

All work was performed within an isolated Conda environment (`env1`).

```bash
# 1. Create and activate the isolated environment
conda create -n env1 python=3.9
conda activate env1

# 2. Install MFA and FFmpeg from the stable conda-forge channel
# Note: This resolved initial dependency conflicts and missing Kaldi components.
conda install -c conda-forge montreal-forced-aligner ffmpeg
```

-----

## 2\. 🎧 Data Preparation and Conversion (Task 3)

### A. Initial Organization and Path

The original corpus files were located in the base project directory. The following commands structure the data for MFA and resolve the critical audio format error (`LibsndfileError`).

```bash
# Navigate to the project folder
D:
cd "IIIT - Assignment\IIIT_MFA_Assignment"

# Create directory for the clean, converted corpus
mkdir converted_corpus
```

### B. Audio Conversion (Resolving Format Errors)

The original WAV files required conversion to the standard **Mono, 16kHz, 16-bit PCM** format for MFA compatibility.

```bash
# Convert all WAV files using FFmpeg
for %f in (*.wav) do ffmpeg -i "%f" -ac 1 -ar 16000 -acodec pcm_s16le "converted_corpus\%f"

# Copy transcripts into the corpus directory
copy *.txt converted_corpus\
```

-----

## 3\. 💾 Model Acquisition and Execution (Tasks 4 & 5)

### A. Model Acquisition (Workaround for Network Failure)

Due to network restrictions preventing the direct `mfa model download` command, the models were manually downloaded, extracted, and imported locally.

  * **Models Used:** `english_us_arpa` Acoustic Model and Pronunciation Dictionary.

<!-- end list -->

```bash
# Assuming the extracted models are in the 'mfa_downloads' folder:
# 1. Import Acoustic Model (Adjust folder names as needed)
mfa model save acoustic mfa_downloads/english_us_arpa_acoustic --name english_us_arpa

# 2. Import Pronunciation Dictionary
mfa model save dictionary mfa_downloads/english_us_arpa_dictionary/english_us_arpa.dict --name english_us_arpa
```

### B. Final Forced Alignment Command

The core task was executed successfully:

```bash
mfa align converted_corpus english_us_arpa english_us_arpa output_alignments
```

-----

## 4\. 📈 Analysis and Submission (Task 6 & Submission)

### A. Alignment Outputs

The outputs required for the submission are located here:

  * **TextGrid Files:** Found in the **`output_alignments`** directory.
  * **Analysis Log:** `output_alignments/alignment_analysis.csv`.

### B. Analysis Summary

The full TextGrid output was inspected using Praat.

  * **Boundary Alignment (Task 6b):** Confirmed successful segmentation of words and phonetic units, with boundaries aligning precisely to acoustic events in the spectrogram and waveform.
  * **Observations (Task 6c):** Noted the effective handling of **silence** (segmented as `text = ""`) and the identification of Out-of-Vocabulary words as **`<unk>`**.

### C. Submission Checklist

  * **GitHub Repository:** This public repository contains all setup instructions, scripts, and output files.
  * **Outputs:** All required **TextGrid files** are located in the **`output_alignments`** folder.
  * **Report:** A PDF report summarizing the models used, the Praat visualization, and the key observations is submitted via email.

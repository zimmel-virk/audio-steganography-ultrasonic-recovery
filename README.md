# Audio Steganography & Ultrasonic Signal Recovery

A digital signal processing project that detects hidden information in audio, recovers an amplitude-modulated ultrasonic signal, and implements message embedding and extraction using an enhanced least-significant-bit steganography method.

The project contains two main tasks:

- **Ultrasonic signal detection and AM demodulation**
- **Audio steganography with enhanced LSB embedding, extraction and integrity verification**

The original Jupyter notebook, WAV files and generated output files are preserved unchanged.

## Task 1 — Ultrasonic Signal Detection and Recovery

Four suspicious WAV files are analysed:

```text
Ex3_sound1.wav
Ex3_sound2.wav
Ex3_sound3.wav
Ex3_sound4.wav
```

The objective is to identify which file contains a hidden four-digit code shifted into an ultrasonic frequency band and recover the concealed audio into the audible range.

### Signal analysis

The notebook applies the Short-Time Fourier Transform (STFT) and measures spectral energy between approximately:

```text
18 kHz – 24 kHz
```

For each suspicious file it calculates:

- ultrasonic-band energy;
- dominant ultrasonic peak frequency;
- STFT magnitude;
- full-band spectrogram;
- ultrasonic-band spectrogram.

The files are ranked by ultrasonic energy.

The recorded notebook output identifies:

```text
Ex3_sound4.wav
```

as the strongest ultrasonic anomaly.

Recorded dominant carrier estimate:

```text
≈ 19,183.59 Hz
```

### AM demodulation

The hidden signal is recovered using a DSP pipeline that:

1. isolates the ultrasonic carrier region with a Butterworth band-pass filter;
2. mixes the signal down from the ultrasonic carrier to baseband;
3. applies low-pass filtering to retain audible content;
4. normalises the recovered waveform;
5. writes the recovered signal as a WAV file.

The original generated result is preserved as:

```text
outputs/ex3_1_demodulated_secret.wav
```

## Task 2 — Enhanced Audio LSB Steganography

The second part embeds the required message into:

```text
Ex3_sound5.wav
```

The secret message used in the original project is:

> It always seems impossible until it's done

The implementation goes beyond a basic sequential one-bit LSB scheme.

### Embedding features

The notebook uses:

- **2 LSBs per audio sample**
- seeded pseudo-random sample selection
- non-consecutive embedding positions
- preference for low-amplitude / gap samples
- fallback to the wider signal when required
- XOR-based payload scrambling
- a stored payload-length header
- CRC16 integrity checking
- exact sample-index storage for reproducible extraction

### Payload structure

The embedded stream contains:

```text
32-bit message-length field
32-bit CRC field
scrambled payload bits
```

The original notebook recorded:

```text
Payload length: 336 bits
CRC16:          8644
Total bits:     400
Samples used:   200
LSBs/sample:    2
```

The generated steganographic audio file is:

```text
outputs/ex3_2_stego_sound5.wav
```

and the extraction parameters are saved in:

```text
outputs/ex3_2_key.npz
```

## Extraction and Verification

Extraction uses the saved seed, parameters and exact embedding indices.

The process:

1. reads the stored LSBs from the stego audio;
2. reconstructs the message-length and CRC header;
3. extracts the scrambled payload;
4. reverses the XOR scrambling;
5. recalculates CRC16;
6. decodes the payload to UTF-8 text;
7. checks that the recovered message matches the original exactly.

The recorded notebook output shows:

```text
Stored CRC16:   8644
Computed CRC16: 8644
```

and verifies that the extracted message exactly matches the embedded message.

## Signal Processing Techniques

The project applies:

- Short-Time Fourier Transform (STFT)
- spectral-energy analysis
- ultrasonic-frequency detection
- dominant-frequency estimation
- Butterworth band-pass filtering
- Butterworth low-pass filtering
- amplitude-modulation demodulation
- frequency mixing / down-conversion
- waveform normalisation
- PCM audio handling
- LSB steganography
- pseudo-random embedding
- XOR scrambling
- CRC integrity validation

## Repository Structure

```text
audio-steganography-ultrasonic-recovery/
├── README.md
├── requirements.txt
├── .gitignore
├── ORIGINAL_FILES_VERIFIED.txt
├── Exercise3.ipynb
├── Ex3_sound1.wav
├── Ex3_sound2.wav
├── Ex3_sound3.wav
├── Ex3_sound4.wav
├── Ex3_sound5.wav
└── outputs/
    ├── ex3_1_demodulated_secret.wav
    ├── ex3_2_stego_sound5.wav
    └── ex3_2_key.npz
```

## Dependencies

The original notebook uses:

- Python
- NumPy
- SciPy
- Matplotlib
- librosa
- IPython
- Jupyter Notebook

Install the main dependencies with:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Then run:

```bash
jupyter notebook
```

and open:

```text
Exercise3.ipynb
```

## Input and Output Handling

The notebook expects the five original WAV files in the same directory as the notebook:

```text
Ex3_sound1.wav
Ex3_sound2.wav
Ex3_sound3.wav
Ex3_sound4.wav
Ex3_sound5.wav
```

Generated files are written to:

```text
outputs/
```

The repository already contains the original generated outputs from the completed project.

## Original-File Preservation

The original Jupyter notebook, WAV source files and generated output files were copied without rewriting or modifying their contents.

Only repository-support files were added:

```text
README.md
requirements.txt
.gitignore
ORIGINAL_FILES_VERIFIED.txt
```

Operating-system metadata and Jupyter autosave checkpoint folders were excluded.

## Author

**Zimmel Javed Virk**  
BSc Computer Science — University of London

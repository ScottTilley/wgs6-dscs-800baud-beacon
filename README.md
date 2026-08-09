# Something Familiar Is Hiding in This WGS-6 800-Baud Signal

![Something Familiar Is Hiding in This WGS-6 800-Baud Signal](images/title.png)

This started with a DSCS-III beacon, a 1992 thesis, and Daniel Estévez's 2020 analysis of one of my USA-167 recordings.

It ended with a much newer spacecraft — **WGS-6** — producing a signal whose framing looks strikingly familiar.

---

## 1. Where this started: Dani's 2020 DSCS-III work

In June 2020, Daniel Estévez analyzed recordings I had made of the X-band beacon from **DSCS-III-A3 / USA-167**.

His work used James Coppola's 1992 AFIT thesis describing the DSCS III satellite control telemetry beacon and its General Purpose Modem decoder.

Dani showed that the central beacon signal is an **800-baud BPSK stream organized into 8-bit frames**.

The structure he recovered included:

- a constant synchronization channel;
- five repeating frame-bit sequences with periods **25, 27, 29, 31 and 32 frames**;
- an inverse relationship between frame bits 6 and 4;
- telemetry carried in the final channel;
- and, importantly, a difference from the thesis description: on USA-167 the telemetry relationship matched **channel 3**, not channel 2.

Dani's original analysis is here:

**[A look at the DSCS-III X-band beacon — Daniel Estévez, 20 June 2020](https://destevez.net/2020/06/a-look-at-the-dscs-iii-x-band-beacon/)**

That work gave us a very distinctive RF fingerprint for the DSCS-III SCT beacon.

---

## 2. Returning to USA-134 and USA-167

A few years later I returned to the DSCS-III spacecraft while working on automated X-band tracking.

**USA-134 was still transmitting the older beacon format remarkably cleanly.**

When I revisited the two spacecraft more recently, they were no longer behaving identically.

**USA-134 was still carrying recognizable legacy telemetry data. USA-167 was still transmitting the characteristic 800-baud beacon structure, but the legacy telemetry payload no longer appeared to be present.**

That gave me two useful reference cases:

- **USA-134:** framing + active legacy telemetry;
- **USA-167:** framing still present, but telemetry effectively absent.

These signals also became useful torture-test targets while I was developing and debugging my automated tracking system.

---

## 3. The signal that had been sitting in the background

Several years ago I had also noticed an approximately **800-baud BPSK signal from WGS-6**.

WGS is the successor to DSCS. At the time, the resemblance was interesting, but I did not pursue it much further.

Here are the three signals as received during the current comparison work:

![USA-134, USA-167 and WGS-6 waterfall comparison](images/waterfall_comparison.png)

From left to right: **USA-134**, **USA-167**, and **WGS-6**.

Visually, the signals are already suggestive. But similarity in a waterfall is not enough.

The useful question is whether the recovered symbol streams have the same internal structure.

---

## 4. Today: run all three through the same analysis

I took the WGS-6 recording and processed it using the same style of analysis Dani had used on USA-167.

Then I ran **USA-134, USA-167, and WGS-6 side-by-side through the same Jupyter notebook**.

The point was to keep the comparison apples-for-apples: same analysis, same plots, three spacecraft.

The notebook used here is included in this repository:

**[`notebooks/Dani_800baud_three_spacecraft_apples_to_apples.ipynb`](notebooks/Dani_800baud_three_spacecraft_apples_to_apples.ipynb)**

### 4.1 Recovered soft symbols

The first comparison is simply the recovered 800-baud soft-symbol streams.

![Soft-symbol comparison for USA-134, USA-167 and WGS-6](images/notebook_soft_symbols.png)

All three datasets produce clean enough symbol streams to permit the same framing analysis.

---

### 4.2 Eight-bit frame structure

Next, the symbols are arranged into 8-bit frames and the average value of each frame position is examined.

![Frame-bit averages for USA-134, USA-167 and WGS-6](images/notebook_frame_bit_averages.png)

The three spacecraft show the same broad frame organization, including the strongly persistent frame position used to establish alignment.

This is the first strong indication that the WGS-6 signal is not merely another unrelated 800-baud BPSK waveform.

---

### 4.3 The distinctive repeating sequences

The most revealing comparison is the autocorrelation of frame bits 1 through 5.

![Autocorrelation comparison of frame bits 1 through 5](images/notebook_frame_bit_autocorrelation.png)

This is the fingerprint.

The same family of repeating sequences appears in the three spacecraft, with the characteristic periods:

**25, 27, 29, 31 and 32 frames.**

Those are the same periods associated with the DSCS-III SCT beacon architecture described in the historical documentation and recovered by Dani from USA-167.

Seeing those same five sequence lengths in WGS-6 is much stronger evidence than simply observing a similar baud rate or spectral shape.

---

### 4.4 The inverse-channel relationship

Dani's analysis also identified the expected inverse relationship between frame bit 6 and frame bit 4.

The same test can be applied directly to all three recordings:

![Inverse-channel relationship comparison](images/notebook_inverse_channel.png)

Again, WGS-6 follows the same framing family.

---


### 4.5 Even and odd bits: where the generational difference becomes obvious

Separating the recovered 200-bit candidate words into their even and odd bit positions makes the comparison much more visually striking.

#### Even bit positions

![Even-bit comparison for USA-134, USA-167 and WGS-6](images/notebook_even_bits.png)

#### Odd bit positions

![Odd-bit comparison for USA-134, USA-167 and WGS-6](images/notebook_odd_bits.png)

This is where the relationship between the three spacecraft becomes especially interesting.

**USA-167 and WGS-6 look much more like one another than either looks like USA-134.** USA-134 retains the strongly structured appearance expected from the older, still-active telemetry implementation. By contrast, USA-167 and WGS-6 both show the same broader SCT-family framing while their recovered payload regions are far less like the legacy USA-134 telemetry pattern.

That matters because the spacecraft represent two generations of the system. USA-134 is a DSCS-III spacecraft and, in these observations, appears to remain in what is effectively the older operational beacon configuration: the legacy telemetry is still visibly populated. USA-167 is also DSCS-III, but its present-day signal has moved away from that older telemetry presentation while retaining the underlying 800-baud framing machinery.

WGS-6 then falls on the **USA-167 side of that comparison**, not the USA-134 side.

The most cautious interpretation is therefore not that WGS-6 is simply transmitting the old USA-134 telemetry format unchanged. The evidence instead suggests a continuity of the **underlying SCT signaling architecture**, with USA-167 providing an important intermediate example: an older DSCS-III spacecraft whose current beacon retains the framing structure but resembles WGS-6 more closely in the recovered data field than it resembles the still-legacy USA-134 configuration.

That makes USA-167 particularly significant. It appears to bridge the observational gap between the visibly older USA-134 implementation and what is now being seen on WGS-6.

This is an observational interpretation of these recordings; it does not by itself establish the internal implementation history or operational purpose of the WGS signal.


## 5. What the comparison shows

Taken together, the comparison shows that the WGS-6 800-baud signal is not just superficially similar to the older DSCS-III beacons.

At the framing level, it reproduces the distinctive architecture:

- **8-bit framing**
- **25-frame sequence**
- **27-frame sequence**
- **29-frame sequence**
- **31-frame sequence**
- **32-frame sequence**
- **inverse channel relationship**
- **the same higher-level synchronization behavior**
- and the same channel-3 masking relationship previously observed on USA-167

The result is therefore:

> **The WGS-6 800-baud signal is compatible at the framing level with the distinctive DSCS-III SCT beacon architecture documented in the 1992 thesis and demonstrated by Dani on USA-167 in 2020.**

The interesting part is the continuity.

A signaling architecture associated with the older DSCS-III spacecraft appears to have survived the transition into WGS.

---

## 6. What this does *not* show

There is an important distinction between identifying the framing architecture and identifying the operational content carried by it.

The WGS-6 recording examined here does **not** show active legacy telemetry data.

In this capture, the recovered candidate data field is effectively idle/zero.

So the claim here is about the **signal structure and framing**, not about operational message content.

---

## 7. Reproduce the result

The repository contains both the original receiver recordings and the demodulated soft-symbol files.

### Original WAV recordings

These are the GQRX audio recordings from which the 800-baud streams can be demodulated again:

- **[USA-134 WAV](data/wav/usa134_800baud.wav)**
- **[USA-167 WAV](data/wav/usa167_800baud.wav)**
- **[WGS-6 WAV](data/wav/wgs6_800baud.wav)**

### Demodulated float32 soft symbols

These are the files used directly by the comparison notebook:

- **[USA-134 soft symbols](data/soft_symbols/symbols_800baud_usa134.f32)**
- **[USA-167 soft symbols](data/soft_symbols/symbols_800baud_usa167.f32)**
- **[WGS-6 soft symbols](data/soft_symbols/symbols_800baud_wgs6.f32)**

The `.f32` files contain little-endian 32-bit floating-point soft symbols, one value per recovered 800-baud symbol.

### Run the notebook

```bash
python3 -m pip install -r requirements.txt
jupyter notebook notebooks/Dani_800baud_three_spacecraft_apples_to_apples.ipynb
```

The notebook reads the three soft-symbol files, aligns each signal independently, and reproduces the side-by-side comparisons shown above.

---

## Repository layout

```text
.
├── README.md
├── requirements.txt
├── data
│   ├── soft_symbols
│   │   ├── symbols_800baud_usa134.f32
│   │   ├── symbols_800baud_usa167.f32
│   │   └── symbols_800baud_wgs6.f32
│   └── wav
│       ├── usa134_800baud.wav
│       ├── usa167_800baud.wav
│       └── wgs6_800baud.wav
├── images
│   ├── title.png
│   ├── waterfall_comparison.png
│   ├── waterfall_usa134.png
│   ├── waterfall_usa167.png
│   ├── waterfall_wgs6.png
│   ├── notebook_soft_symbols.png
│   ├── notebook_frame_bit_averages.png
│   ├── notebook_frame_bit_autocorrelation.png
│   └── notebook_inverse_channel.png
└── notebooks
    └── Dani_800baud_three_spacecraft_apples_to_apples.ipynb
```

---

## Bottom line

The story began with a Cold War-era DSCS-III beacon and a 2020 reverse-engineering effort.

USA-134 still shows the older telemetry implementation. USA-167 retains the framing while its legacy telemetry payload appears to have gone quiet.

And WGS-6 — a spacecraft from the system that replaced DSCS — is still transmitting an 800-baud signal with the same unmistakable structural fingerprint.

**Something familiar really was hiding in WGS-6.**

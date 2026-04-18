# Music Removal Method (Linux)

> Experimental Release: This method is currently in an experimental phase. It provides acceptable performance for suppressing background music while keeping speech audible, but it is not yet flawless and may require further tuning across different voice styles and microphone qualities. Contributions and community tweaks are encouraged.

## Overview
This is a practical, real-time, low-latency audio filtering method designed to separate background music from speech in digital audio streams (e.g., browsers, Telegram, podcasts, and media players).

Some speech forms, including Quran recitation, can share tonal characteristics with music. Generic voice-isolation filters may misclassify these passages and mute speech. This method uses a bounded **DeepFilterNet** profile to reduce that behavior. The main objective is still speech-first output: remove background music and keep spoken content intelligible.

In practical tests on videos that contain background music, this setup produced acceptable results: background music was reduced while speech and recitation remained understandable.

## Why This Method Is Effective

This method is useful in day-to-day listening because it combines practical compatibility with focused filtering behavior:

- **Works across many apps:** Any software routed through EasyEffects can be processed, including browsers, Telegram, and local audio/video players.
- **Real-time and low-latency:** Audio is processed during playback, so you can use it while streaming or listening live.
- **Speech-first tuning:** Conservative DeepFilterNet bounds reduce background music while helping preserve speech clarity.
- **Recitation-safe safeguard:** Quran recitation is less likely to be misclassified as music, but this is a safeguard rather than the primary objective.
- **Privacy-first by design:** Processing is local on your machine, with no cloud upload.
- **Easy to deploy and share:** Import one preset and apply it to selected applications.
- **Extensible and community-driven:** Thresholds and processing behavior can be refined over time through contributions.
- **Tested on mixed-content audio:** It has been tested on videos with background music and delivered acceptable speech-first output.

## Architecture and How It Works
This setup operates entirely locally on your machine, ensuring complete privacy and zero cloud processing overhead. To achieve this, it uses a three-step pipeline:

1. **Audio Routing (PipeWire):** Instead of audio going straight to your headphones, PipeWire intercepts the audio output from your target application (like your web browser).
2. **Processing Pipeline (EasyEffects):** The intercepted audio stream is dynamically routed through EasyEffects, an advanced audio manipulation framework for Linux.
3. **AI Filtering (DeepFilterNet):** Inside EasyEffects, the audio is processed by the DeepFilterNet plugin. This method restricts the AI with strict safety thresholds (such as an attenuation limit of 40 and a -15 dB processing threshold) to aggressively target background music while preserving speech. Additional bounds help avoid treating recitation-like vocal characteristics as music.

---

## Installation (Linux)

This method requires **PipeWire**, **EasyEffects**, and **DeepFilterNet**. Choose the installation path appropriate for your Linux distribution.

### Option 1: Universal Installation (Flatpak - Recommended)
The most reliable method across all distributions (Ubuntu, Linux Mint, Fedora, etc.) is using Flatpak, as it guarantees compatibility and bundles the necessary plugins easily.

```bash
# 1. Install EasyEffects
flatpak install flathub com.github.wwmm.easyeffects

# 2. Install the DeepFilterNet extension for the EasyEffects Flatpak
flatpak install flathub com.github.wwmm.easyeffects.plugin.deepfilternet
```

### Option 2: Arch Linux (Native)
Arch users can easily install the native packages via `pacman` and the AUR.
```bash
sudo pacman -S easyeffects
yay -S libdeep_filter_ladspa-bin
```

### Option 3: Fedora
Fedora provides EasyEffects in its official repositories. DeepFilterNet can be installed via Cargo (Rust).
```bash
sudo dnf install easyeffects
cargo install deep-filter
```

### Option 4: Ubuntu / Debian
*Note: EasyEffects strictly requires PipeWire. Ensure your Ubuntu/Debian installation is utilizing PipeWire instead of PulseAudio before proceeding.*
```bash
sudo apt install easyeffects
```
*(For DeepFilterNet on Ubuntu, it is highly recommended to use the Flatpak method above, or compile the LADSPA plugin manually from the official DeepFilterNet repository).*

---

## Configuration and Usage

### 1. Import the Preset
1. Download the preset JSON file from this repository (currently `haramFilter.json`).
2. Launch **EasyEffects**.
3. Click the **Presets** button located in the top-left corner.
4. Select **Import** and locate your downloaded preset file.
5. Select the imported preset from your preset list and click **Load**.

### 2. Route Your Audio Applications
By default, EasyEffects will not alter your audio until you explicitly tell it which applications to intercept.
1. Navigate to the **Players** tab within the EasyEffects interface.
2. Locate the application currently playing your audio (e.g., Brave, Firefox, VLC).
3. Toggle the switch next to the application to the **ON** position.
4. The application's audio is now being routed through the filtering pipeline.

---

## Windows and macOS
*This configuration is currently optimized exclusively for Linux via PipeWire. Efforts to replicate this specific DeepFilterNet tuning on Windows (via Equalizer APO) and macOS are actively being researched. Check back for future updates.*

## Contributing
Because this is an experimental release, the community's help is vital. If you discover improved EQ curves, better threshold limits, or have successfully ported this logic to Windows/macOS, please open an Issue or submit a Pull Request.

## License
This configuration and methodology are provided completely free of charge and open-source. May it serve as a *Sadaqah Jariyah* (continuous charity) for the community.
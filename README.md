# Rattlegram Plus

[![License](https://img.shields.io/badge/License-0BSD-blue.svg)](LICENSE)
[![Kotlin](https://img.shields.io/badge/Kotlin-1.9+-purple)](https://kotlinlang.org/)
[![API](https://img.shields.io/badge/API-21%2B-brightgreen)](https://developer.android.com/)
[![Platform](https://img.shields.io/badge/Platform-Android-green)](https://www.android.com/)
[![JNI](https://img.shields.io/badge/JNI-C%2B%2B-orange)](https://github.com/BI3BJU/rattlegram-plus)
[![GitHub last commit](https://img.shields.io/github/last-commit/BI3BJU/rattlegram-plus)](https://github.com/BI3BJU/rattlegram-plus)

Rattlegram Plus is an Android application that enables short-range text communication using audio modulation/demodulation. It encodes text messages into audible or ultrasonic audio signals and decodes them from the microphone, allowing peer-to-peer messaging without Wi‑Fi, Bluetooth, or cellular networks.

This project is a continuation of the original Rattlegram, enhanced with end‑to‑end encryption, location sharing, cyclic beacon, and a modern Material Design interface.

## Screenshots

| Main |
| :---: |
| ![Main](./main.jpg) |

## Features

- 📡 **Audio‑based messaging** – transmit and receive text over sound (speaker & microphone).
- 🔐 **End‑to‑end encryption** – optional password‑based AES encryption (256‑bit) for privacy.
- 📍 **Location sharing** – share your GPS coordinates; recipients can open them in any map app.
- 🔁 **Repeater mode** – automatically repeat received messages (with debounce and delay).
- 📶 **Spectrum analyzer** – real‑time FFT and waterfall display for signal tuning.
- 🕊️ **Cyclic beacon** – periodic transmission of a ping (empty message) for presence detection.
- 🎚️ **Flexible audio settings** – sample rates, channel selection, audio source, carrier frequency, noise symbols.
- 🌙 **Night mode** – dark theme support.
- 📨 **Message history** – stored persistently with sent/received bubbles.
- 🗺️ **Map integration** – tap a received location message to open in All‑In‑One Offline Maps (or system picker).

## How It Works

The app uses a native C++ library (`librattlegram.so`) to perform FSK‑based modulation and demodulation. Text messages are converted to audio frames, played through the speaker, and received via the microphone. The decoder synchronises to the incoming signal, extracts the payload, and displays it.

- **Encoder** – packs your message into a structured frame (header + payload), adds error correction symbols, and generates PCM audio samples.
- **Decoder** – continuously analyses microphone input, detects preamble, synchronises, and extracts the original data.
- **Encryption** – when enabled, the message is encrypted before encoding and decrypted after decoding using the configured password.

## Installation

1. Download the latest APK from the [Releases page](https://github.com/BI3BJU/rattlegram-plus/releases).
2. Enable **Install from unknown sources** in your Android settings.
3. Install and launch the app.
4. Grant microphone and location permissions when prompted.

> ⚠️ The app requires Android 5.0 (API 21) or higher.

## Building from Source

Clone the repository:

```bash
git clone https://github.com/BI3BJU/rattlegram-plus.git
cd rattlegram-plus
```

1. Open the project in Android Studio (Arctic Fox or later).
2. Build the native library (CMake is used).
   - The JNI source is located under `app/src/main/cpp/`.
   - Ensure you have the NDK and CMake installed via SDK Manager.
3. Build and run the app on your device or emulator.

## Usage Guide

### 🗣️ Sending a Message

1. Tap the compose button (📝) at the bottom right.
2. Type your text (up to 170 bytes).
3. Optionally check **Encrypt** (requires a password set in the menu).
4. Tap **Transmit** – the audio will play and the message will appear as “sent”.

### 📍 Sharing Your Location

1. Tap the location button (📍) at the bottom left.
2. Grant location permission if not already.
3. The app will fetch the last known GPS/network location and pre‑fill the compose dialog with a `[LOC] geo:lat,lng` string.
4. Send it like a normal message.

### 🔁 Repeater Mode

- Enable from the menu: **Enable repeater mode**.
- When a message is received, it will be retransmitted automatically after a configurable delay.
- Debounce prevents repeated retransmission of the same message within a set time.

### 📶 Spectrum Analyzer

- From the menu, select **Show spectrum**.
- A dialog appears showing real‑time FFT (frequency spectrum) and a waterfall (spectrogram).
- Useful for fine‑tuning carrier frequency or checking signal quality.

### 🔐 Setting a Password

- Open the menu → **Password**.
- Enter a password (8–256 bytes) or tap **Generate** to create a secure hex string.
- Once set, you can encrypt outgoing messages; incoming encrypted messages will be decrypted automatically.

### 🕊️ Cyclic Beacon

- From the menu, tap **Ping** – the app will send an empty message every minute.
- Tap again to stop.

### 🗺️ Opening a Received Location

- Tap any received message that starts with `[LOC] geo:`.
- The system will ask which map application to use (All‑In‑One Offline Maps is preferred).

## Permissions

| Permission | Required for |
| --- | --- |
| `RECORD_AUDIO` | Microphone access. |
| `ACCESS_FINE_LOCATION` | GPS location sharing. |
| `POST_NOTIFICATIONS` | Optional. Not used, but may appear on newer Android versions. |

## Configuration Options (Menu)

| Option | Description |
| --- | --- |
| Output / Record sample rate | 8, 16, 32, 44.1, 48 kHz |
| Channel selection | Mono / Stereo / Left / Right / Sum / Analytic |
| Audio source | Default, Mic, Camcorder, Voice Recognition, Unprocessed |
| Carrier frequency | 1000 Hz – (sample rate/2 – bandwidth) |
| Noise symbols | Adds extra FEC symbols (0–22) |
| Repeater delay | 0–8 seconds |
| Repeater debounce | 0–120 seconds (prevents echo loops) |
| Fancy header | Uses a longer preamble for better sync |
| Night mode | On / Off |
| Delete messages | Clears history |
| Force quit | Exits the app completely |

## Libraries & Dependencies

- **AndroidX** – modern UI components and compatibility.
- **Native C++ library** – custom FSK modem (not included here).
- **No external GMS / Play Services** – location uses only Android’s built‑in `LocationManager`.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

- Use the GitHub issue tracker for bugs and feature requests.
- Follow the code style of the existing source.

## License

This project is licensed under the 0BSD License – see the [LICENSE](LICENSE) file for details.

## Credits

- Original Rattlegram by Ahmet Inan <inan@aicodix.de>.
- Rattlegram Plus maintained by BI3BJU <guerilla1949@gmail.com>.

## Disclaimer

This app is provided as‑is for experimental and educational purposes. The author is not responsible for any misuse or damage caused by this software.
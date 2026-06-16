# DroidScrCpy 📱🔌📱

**DroidScrCpy** (Android-to-Android Screen Copy) is an open-source fork and adaptation of [Genymobile/scrcpy](https://github.com/Genymobile/scrcpy). This project aims to display and control an Android device (Target) from another Android device (Controller) using a **USB OTG cable connection** to achieve maximum performance with near-zero latency.

Unlike other porting projects that rely on wireless ADB connections—which are often unstable and prone to lag—DroidScrCpy leverages the USB Host mode on the Controller device to act natively as an ADB host.

---

## 🚀 Concept & Workflow
[Device B: Controller] ----(USB OTG Cable)----> [Device A: Target]
DroidScrCpy.apk installed                     - USB Debugging enabled
Acts as Native ADB Host                       - Receives scrcpy-server.jar
Decodes & Displays Video Stream               - Transmits raw video stream
Injects touch inputs back                     - Executes injected touch events

1. **OTG Connection:** Device B (Controller) is connected to Device A (Target) via a USB cable paired with an OTG adapter on Device B's end.
2. **ADB Handshake:** The DroidScrCpy app on Device B initiates the ADB protocol natively, triggering the standard "Allow USB debugging?" authorization pop-up on Device A.
3. **Server Deployment:** Once authorized, Device B pushes the compiled `scrcpy-server.jar` binary into Device A's temporary directory (`/data/local/tmp/`) and executes it via the ADB shell.
4. **Streaming & Decoding:** The `scrcpy-server` on Device A captures the screen display and streams raw H.264/H.265 video data through the USB socket back to Device B. The app then decodes it in real-time using Android's hardware `MediaCodec` API.
5. **Control Injection:** Touch inputs, gestures, and key events performed on Device B's screen are mapped, scaled, and sent back to Device A to enable full remote control.

---

## 🛠️ Core Technical Components

This project is built by orchestrating the following core components:
* **`scrcpy-server.jar` (Genymobile):** The stock scrcpy server binary that runs on the target device to capture and encode the screen display.
* **AdbLib / Pure Java ADB:** A Java/Kotlin implementation of the ADB client protocol to communicate over the Android `UsbManager` without requiring root access or a PC binary.
* **Android MediaCodec API:** Used on the Controller application side to perform low-latency hardware video decoding for a smooth mirroring experience.

---

## 📈 Development Roadmap

- [ ] **Phase 1:** Set up the Android Studio project structure and integrate the USB Host / ADB Client libraries.
- [ ] **Phase 2:** Implement the native USB authorization and ADB handshake between the two devices.
- [ ] **Phase 3:** Establish the automated routine to push and execute `scrcpy-server.jar` on the target device.
- [ ] **Phase 4:** Establish the socket connection to capture, decode (`MediaCodec`), and render the incoming video stream.
- [ ] **Phase 5:** Implement input injection to translate and send touch events back to the target device.

---

## 🤝 Contributing

This project is currently in its conceptual and early development phase. If you are passionate about low-level Android development, USB/ADB protocols, or real-time video streaming optimization on mobile hardware, contributions are highly welcome! Feel free to open an issue or submit a pull request.

## License
This project is licensed under the Apache License 2.0, matching the original license of Genymobile/scrcpy.

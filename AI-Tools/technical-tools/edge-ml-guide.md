# ⚡ Edge ML Guide: Deploying Intelligence to Microcontrollers

This guide outlines the technical workflow for training and deploying Machine Learning models to edge hardware (like the Raspberry Pi Pico or ESP32) using the **Edge Impulse** ecosystem within the MatsRobot project.

---

## 🏗️ The Edge ML Workflow

To move from raw sensor data to a running "Impulse" on a robot, follow this standardized four-stage pipeline:

### 1. Data Acquisition

The foundation of any model is high-quality data.

- **Sensor Selection:** Identify if you are tracking **Motion** (Accelerometer/Gyro), **Vision** (Camera), or **Acoustics** (Microphone).
- **Sampling:** Connect your device to the Edge Impulse Studio and record at least 2-3 minutes of data for each "Label" (e.g., "Moving," "Stationary," "Error_Vibration").
- **Data Splitting:** The Studio will automatically split your data into _Training_ (80%) and _Testing_ (20%) sets to ensure model validity.

### 2. Impulse Design (The Architecture)

An "Impulse" is the complete digital pipeline consisting of:

- **Pre-processing (DSP):** Digital Signal Processing blocks that clean the data (e.g., Spectral Analysis for vibration or Image Normalization for vision). This reduces the workload on the microcontroller's CPU.
- **Learning Block:** The Neural Network (Keras/TensorFlow Lite) that classifies the processed data.

### 3. Training & Optimization

- **EON™ Tuner:** Use the EON Tuner to find the best trade-off between **Accuracy**, **Latency**, and **RAM usage**.
- **Quantization:** Convert the model from float32 to **int8**. This typically reduces model size by 4x with minimal loss in accuracy, which is essential for hardware like the RP2040.

### 4. Deployment (C++ Integration)

Instead of running a heavy interpreter, we export a **C++ Library**.

- **Download:** Select the "C++ Library" or "Arduino Library" option in the Deployment tab.
- **Integration:** Include the library in your project:
  ```cpp
  #include <your_project_inferencing.h>

  // Standard inference loop
  void loop() {
      signal_t signal;
      // Feed sensor data into signal...
      ei_impulse_result_t result;
      run_classifier(&signal, &result, false);

      // Act based on result
      if (result.classification[1].value > 0.8) {
          digitalWrite(LED_PIN, HIGH); // Detected target label
      }
  }
  ```

---

## 🛠️ Hardware Compatibility Matrix

| Hardware          | Best Use Case                | Recommended DSP    |
| :---------------- | :--------------------------- | :----------------- |
| **RP2040 (Pico)** | Motion / Simple Audio        | Spectral Analysis  |
| **ESP32-S3**      | Vision / Voice Commands      | Image / MFCC       |
| **Arduino Nicla** | Always-on Industrial Sensing | Raw Data / Flatten |

---

## 🚦 Troubleshooting Tips

- **"Out of Memory" Error:** Reduce the "Window Size" in your Impulse design or use a smaller Neural Network architecture (e.g., MobileNetV1 instead of V2).
- **Low Accuracy:** Ensure your background noise is consistent during data capture. If the robot moves differently on carpet vs. tile, you must sample both environments.
- **Latency Issues:** Check if the DSP block is too complex. Switching from "Spectrogram" to "MFE" can often save valuable milliseconds on lower-clocked chips.

---

<small>© 2026 MatsRobot | Technical Documentation Layer</small>

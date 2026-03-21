# 🤖 AI-Assisted Development & Debugging

Traditional manual programming is no longer the primary barrier to entry for micro-controller and AI experimentation. Modern AI platforms now generate highly accurate, functional code and provide sophisticated error analysis to resolve bugs autonomously.

However, a foundational understanding of software logic remains essential. This knowledge allows you to effectively "prompt" or steer the AI when the generated output deviates from your specific design goals. To navigate this ecosystem successfully, a working familiarity with **Python** and **C++** is required.

Integrating AI into your debugging workflow is a game-changer for robotics, where errors can occur in both high-level logic (Python) and low-level hardware interaction (C++). Instead of just asking the AI to "fix it," using a structured approach ensures the solution actually works for your specific microcontroller.

---

## 🤝 Best Practices: AI as a Debugging Partner

### 1. Provide Contextual "Hardware Awareness"

When debugging C++ for microcontrollers, the AI needs to know your hardware constraints. Standard C++ is different from "Embedded C++" used in Arduino or ESP32.

- **Action:** Always specify your board and libraries in the prompt.
- **Example:** _"I am using an ESP32 with the WiFi.h library. My code crashes during the WiFi.begin() call. Here is my setup code..."_

### 2. The "Rubber Duck" Technique (Logic Verification)

Sometimes the code is syntactically perfect but logically flawed. Use the AI to explain what the code thinks it is doing.

- **Action:** Paste a snippet and ask: _"Walk me through the logic of this loop step-by-step and identify any potential race conditions or memory leaks."_
- **Result:** This often reveals "off-by-one" errors in matrix calculations or timing issues in sensor polling that a standard compiler wouldn't catch.

### 3. Iterative Error Analysis

AI can occasionally "hallucinate" a fix that doesn't exist in your specific library version.

- **Action:** If the AI's first suggestion fails, don't just say "it didn't work." Provide the specific compiler error or the serial monitor output.
- **Example:** _"The suggested fix gave me a 'division by zero' error at line 42. Here is the updated serial output..."_

### 4. Optimise for "Edge Cases"

Robots operate in the real world where sensors are noisy and batteries die. AI is excellent at generating "safety" code, so ask the AI to harden your code against failures.

- **Example:** _"Here is my Python script for processing camera Tensors. Can you add error handling for when the camera feed is interrupted or the Tensor dimensions are null?"_

---

## 🛠️ Summary of Debugging Workflow

| Step         | Action                                                             | AI Role                                                 |
| :----------- | :----------------------------------------------------------------- | :------------------------------------------------------ |
| **Isolate**  | Feed the AI only the failing function, not the whole file.         | Focuses the "attention" of the model.                   |
| **Annotate** | Add comments to your code explaining your intent.                  | Helps the AI spot the gap between intent and execution. |
| **Validate** | Ask the AI: "Are there any deprecated functions in this C++ code?" | Ensures compatibility with modern AI/Robot libraries.   |

---

## ⚠️ Managing AI Truncation & Code Integrity

One of the most common pitfalls when using AI for development is **"Code Truncation."** To save processing power, AI models often provide a "snippet" that focuses only on the immediate fix, omitting large blocks of your original code with placeholders like `// ... rest of code here`. In a complex robotics configuration, this can lead to broken dependencies and missing initialisation steps.

### How to Prevent Data Loss:

- **The "Full File" Mandate:** Explicitly instruct the AI to return the entire script. Use a prompt like: _"Provide the complete, updated code in a single block. Do not truncate or use placeholders for existing logic."_
- **Modular Prompting:** Instead of feeding the AI a 500-line script, break your robot's logic into smaller, functional modules (e.g., `sensor_input.cpp`, `motor_control.cpp`). AI is much less likely to truncate a 50-line file than a 500-line one.
- **Version Comparison:** Before hitting "save" on your config, use a **Diff Tool** (like VS Code’s built-in compare feature). This allows you to visually see exactly what the AI changed and, more importantly, what it accidentally deleted.
- **The "Append Only" Strategy:** If you only need a specific feature added, ask the AI to: _"Write only the new function for the ultrasonic sensor. I will manually integrate it into my main loop."_ This prevents the AI from touching (and potentially breaking) your stable code.

| Issue             | AI Behaviour                       | Professional Workaround                                     |
| :---------------- | :--------------------------------- | :---------------------------------------------------------- |
| **Truncation**    | Replaces logic with `// ...`       | Use the "Complete File" instruction.                        |
| **Hallucination** | Invents a non-existent library.    | Always provide the specific library versions you are using. |
| **Logic Drift**   | Fixes one bug but creates another. | Use "Diff" tools to verify changes before deployment.       |

---

[← Back to Main Page](../README.md)

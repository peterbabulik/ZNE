# 50-Qubit Quantum Benchmarking (Qiskit Runtime V2)

This project demonstrates the execution of a mid-scale quantum circuit (50 Qubits) on IBM Quantum hardware using the latest Qiskit Runtime V2 primitives. It specifically utilizes the `ibm_torino` (Eagle) backend to compute the expectation value of a randomized `EfficientSU2` ansatz with Zero Noise Extrapolation (ZNE) error mitigation.

## 🌟 Features

*   **Qiskit Runtime V2:** Uses the modern `EstimatorV2` primitive and `ibm_quantum_platform` channel.
*   **Advanced Error Mitigation:** Implements Zero Noise Extrapolation (ZNE) to improve result fidelity.
*   **Hardware-Efficient Ansatz:** Constructs a 50-qubit `EfficientSU2` circuit with linear entanglement.
*   **Robust Transpilation:** Leverages `optimization_level=3` to map logical circuits to the `ibm_torino` physical topology.
*   **Automated Fallback:** Automatically switches to the least busy available system if the target backend is offline.

## 📋 Requirements

*   Python 3.9+
*   Qiskit SDK
*   Qiskit IBM Runtime
*   NumPy
*   An active [IBM Quantum](https://quantum.ibm.com/) account with an API Token.

## 🚀 Installation

1.  Clone the repository:
    ```bash
    git clone https://github.com/peterbabulik/ZNE
    ```

2.  Install the required dependencies:
    ```bash
    pip install qiskit qiskit-ibm-runtime numpy
    ```

3.  Configure your credentials by editing the script or setting an environment variable. By default, the script looks for the variable here:
    ```python
    API_KEY = "YOUR_IBM_QUANTUM_API_TOKEN"
    ```

## 🏃 Usage

Run the experiment using Python, or Google Colab

### What the script does:
1.  **Authenticates** with IBM Quantum Platform.
2.  **Selects** `ibm_torino` (or the best available fallback).
3.  **Builds** a 50-qubit parameterized circuit.
4.  **Transpiles** the circuit for the specific quantum processor.
5.  **Generates** random parameter values (0 to 2π).
6.  **Executes** the job using `EstimatorV2` with ZNE enabled.
7.  **Outputs** the Expectation Value, Standard Error, and Runtime statistics.

## 📊 Sample Output

```text
--- PREPARING QUANTUM ADVANTAGE EXPERIMENT ---
Target: ibm_torino | Qubits: 50
Connected to ibm_torino. Status: active
Constructing 50-Qubit Circuit...
Transpiling to Physical Qubits...

--- SUBMITTING JOB TO IBM QUANTUM ---
Method: EstimatorV2 with ZNE (Explicit)
Circuit has 200 parameters. Generating random values...
Job ID: d5t9414bmr9c739mr420
Waiting for results...

========================================
      DATA FOR GITHUB TRACKER
========================================
Expectation Value:   0.0001012587895934192
Standard Error (+/-):0.00907142081105357
Total Gates:         794
Qubits:              50
Backend:             ibm_torino
Runtime (s):         22.85
Method:              EstimatorV2 (ZNE Mitigation)
========================================
```

## 📝 Notes

*   **Expectation Value:** Since parameters are randomized, the expectation value of the $Z^{\otimes n}$ observable typically averages to 0.
*   **Runtime:** Execution time varies heavily based on the queue time of the selected backend.
*   **Deprecation:** The code currently uses `EfficientSU2` (class-based) which is deprecated in Qiskit 2.1 in favor of function-based imports, but remains functional.

## 📄 License

This project is open source and available under the MIT License.




# What is Zero Noise Extrapolation (ZNE)?

Imagine you are trying to listen to your favorite song on the radio, but there is a lot of **static (noise)**. You can't perfectly hear the notes.

In quantum computing, the "static" is real. Quantum computers are imperfect, and the answers they give you are slightly "fuzzy" or wrong because of this noise.

**Zero Noise Extrapolation (ZNE)** is a clever math trick we use to clean up that static without needing to buy a new (and expensive) perfect quantum computer.

### The Problem
You run a calculation on a quantum computer.
*   **The Truth:** The answer should be `5`.
*   **The Reality:** Because of noise, the computer gives you `4`.

### The "ZNE" Trick (Intentionally Making It Worse)

To find the "True" answer, ZNE does something that sounds crazy at first: **It runs the calculation multiple times, but intentionally makes the noise worse each time.**

Think of it like taking a photo while the camera is shaking.

1.  **Run 1 (Normal):** You take a photo with the shaking camera. It’s a bit blurry. (You see a `4`).
2.  **Run 2 (Double Noise):** You shake the camera *twice as much* while taking the photo. It is very blurry. (You see a `3`).
3.  **Run 3 (Triple Noise):** You shake the camera *three times as much*. It is a total mess. (You see a `2`).

### The Extrapolation (Drawing the Line)

Now you have three data points on a graph:
*   At **1x Shake**, you saw a `4`.
*   At **2x Shake**, you saw a `3`.
*   At **3x Shake**, you saw a `2`.

You draw a line connecting these points and extend that line **backwards** to where the "Shake" is **Zero**.

*   The line points back to... **`5`**!

By seeing how the result gets worse as the noise gets higher, you can mathematically predict what the result would be if there were **no noise at all**.

### Summary

*   **Quantum Computers are noisy:** They make mistakes.
*   **ZNE runs the job multiple times:** It adds artificial noise to see how the errors grow.
*   **We do some math:** We look at the trend and calculate the answer backwards to "Zero Noise."

This is exactly what the line `options.resilience.zne_mitigation = True` did in code. It made the computer work a bit harder, but it gave you a much more accurate answer


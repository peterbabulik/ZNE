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


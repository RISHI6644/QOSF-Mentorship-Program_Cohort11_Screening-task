# QOSF-Mentorship-Program_Cohort11_Screening-task

This repository contains my solution for the screening task for the Quantum Mentorship program.

## Task 2: Complex Amplitudes

The main goal was to implement a routine that prepares a two-qubit quantum state from a set of four complex amplitudes.

### Requirements:
* **Input:** A list of four complex amplitudes `[a0, a1, a2, a3]`.
* **Output:** A normalized 1D NumPy array representing the state vector $|\psi\rangle = a_0|00\rangle + a_1|01\rangle + a_2|10\rangle + a_3|11\rangle$.
* **Normalization:** The routine must ensure the output state is normalized, i.e., $\sum |a_i|^2 = 1$. If the input is not normalized, a normalization step must be included.
* **Constraints:** The solution must be built from scratch without using high-level quantum libraries (e.g., Qiskit's `initialize`).
* **Testing:** Unit tests must be written to verify normalization and correct output dimension.
* **Stretch Goal:** Generalize the implementation to support a three-qubit state (8 amplitudes).

## Key Concepts

1.  **Quantum State Vectors**
    * A two-qubit system has four possible basis states: $|00\rangle, |01\rangle, |10\rangle, |11\rangle$.
    * Any general quantum state $|\psi\rangle$ is a linear combination (superposition) of these basis states:
        $|\psi\rangle = a_0|00\rangle + a_1|01\rangle + a_2|10\rangle + a_3|11\rangle$
    * Here, $a_0, a_1, a_2, a_3$ are complex numbers called **amplitudes**. The entire state can be represented as a 4-element column vector (or a 1D NumPy array):
        $|\psi\rangle = \begin{bmatrix} a_0 \\ a_1 \\ a_2 \\ a_3 \end{bmatrix}$

2.  **Normalization (The Born Rule)**
    * Quantum states must represent a total probability of 1. The probability of measuring a specific basis state (e.g., $|01\rangle$) is the squared magnitude of its amplitude ($|a_1|^2$).
    * Therefore, the sum of all probabilities must be 1:
        $|a_0|^2 + |a_1|^2 + |a_2|^2 + |a_3|^2 = 1$
    * For this task, if the given amplitudes do **not** satisfy this, we must scale all amplitudes by dividing each one by the **L2-norm** (or "length") of the vector:
        $Norm = \sqrt{|a_0|^2 + |a_1|^2 + |a_2|^2 + |a_3|^2}$

## Implementation Details

To adhere to the **DRY (Don't Repeat Yourself)** principle and ensure robust error handling, the logic is split into a general helper function and specific task functions.

* `_normalize_amplitudes(amplitudes_array)`: An internal helper function that takes a NumPy array, calculates its L2-norm, and returns the normalized array. It robustly checks for a zero vector using `np.isclose` to prevent division-by-zero errors with floating-point numbers.
* `prepare_two_qubit_state(amplitudes)`: The main function for the task. It validates that the input has exactly 4 amplitudes, converts the input to a complex NumPy array using `np.asarray`, and then uses the `_normalize_amplitudes` helper to return the final state vector.
* `prepare_n_qubit_state(amplitudes, num_qubits)`: The solution for the **stretch goal**. This general function validates that the input list has the correct number of amplitudes ($2^n$) for the given `num_qubits` before normalizing.
* `compute_measurement_probabilities(state_vector)`: A "creative expansion" helper function. It takes a normalized state vector and returns a list of measurement probabilities by calculating the squared modulus ($|a_i|^2$) for each amplitude.

### `tests`

The tests cover:
1.  **Normalization is Enforced**: Checks that an unnormalized vector (e.g., `[1, 1, 1, 1]`) is correctly normalized.
2.  **Already Normalized Input**: Checks that an already normalized vector is returned unchanged.
3.  **Correct Dimension**: Confirms the 2-qubit function returns a vector of shape `(4,)`.
4.  **Invalid Dimension Error**: Uses `pytest.raises` to ensure a `ValueError` is raised if the input does not have 4 amplitudes.
5.  **Zero Vector Error**: Uses `pytest.raises` to ensure a `ValueError` is raised for a `[0, 0, 0, 0]` input.
6.  **Complex Amplitudes**: Tests that complex-valued inputs are handled correctly.
7.  **Stretch Goal (3-Qubit)**: Verifies the `prepare_n_qubit_state` function for `n=3`.
8.  **Probability Function**: Tests that `compute_measurement_probabilities` returns the correct probabilities (e.g., `[0.5, 0.5, 0, 0]`) and that they sum to 1.0.

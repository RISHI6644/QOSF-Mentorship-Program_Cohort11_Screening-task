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

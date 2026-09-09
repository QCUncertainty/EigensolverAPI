<!--
  ~ Copyright 2026 QHARM
  ~
  ~ Licensed under the Apache License, Version 2.0 (the "License");
  ~ you may not use this file except in compliance with the License.
  ~ You may obtain a copy of the License at
  ~
  ~ http://www.apache.org/licenses/LICENSE-2.0
  ~
  ~ Unless required by applicable law or agreed to in writing, software
  ~ distributed under the License is distributed on an "AS IS" BASIS,
  ~ WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  ~ See the License for the specific language governing permissions and
  ~ limitations under the License.
-->

# EigenBridge

There are two quantum solvers. VQD can return several eigenvalues; QAOA returns only the ground state.

VQD -> `run_vqd_eigensolver(matrix, k, use_noise=false)` -> lowest `k` eigenvalues (`k` defaults to all)
QAOA -> `run_qaoa_eigensolver(matrix, use_noise=false)` -> ground state only

**Uncertainties (`uq_values` / `uq_vectors`)**
- Noiseless solvers (VQD, noiseless QAOA): `uq_* = 0`
- Noisy QAOA / noisy VQD (`use_noise=true`): at each final circuit, `uq_values[i] = |E_noisy − E_exact|`; unused slots and `uq_vectors` stay 0
- Noisy VQD keeps exact statevector overlaps; only the energy estimator uses FakeManila noise (demo). Expect worse eigenvalues, especially for higher states.

Noisy unit tests check that predicted noise (`uq_values`) and `|λ_noisy − λ_LAPACK|` agree within a factor of 10 for the ground state. They do not require noisy eigenvalues to match LAPACK.

# Functional Design Specification (FDS) - CIP Skid Control Loop

## 1. Regulatory Context & Compliance
The control strategy engineered within `CIP_Sequence.scl` satisfies strict operational validation parameters defined under FDA 21 CFR Part 11 and GAMP 5 guidelines. 

## 2. Dynamic Process Control Boundaries
* **Thermal Invalidation Sequence (Step 2):** To enforce sterilization integrity, if analog input `TE_201` falls below 80°C during the chemical circulation window, the loop logic immediately flags a system failure state, drops digital output safety lines, and transitions directly into `Step 99` to stop line cross-contamination.
* **Purity Verification Loop (Step 3):** Transition out of the water rinse step does not rely on a simple timer. The system dynamically reads Toroidal Conductivity (`CE_202`). The program will loop indefinitely until conductivity registers below 1.5 µS/cm, guaranteeing zero chemical carryover into production phases.

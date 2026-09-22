# Operational Qualification (OQ) Test Protocol
### System: CIP Skid Control Loop | SCL Code Reference: `CIP_Sequence.scl`

This protocol outlines the manual boundary test cases used to verify the functional requirements of the SCL state machine.

| Test ID | Objective / Description | Pre-Requisites / Input Vectors | Expected System Response | Pass/Fail Criteria |
| :--- | :--- | :--- | :--- | :--- |
| **OQ-CIP-001** | Verify normal sequential step progression (Steps 0 → 1 → 2). | Set `Tank_Level_High` = False.<br>Force `Start_Sequence` = True. | System enters Step 1.<br>`Water_Valve` = True.<br>After 300s, system transitions to Step 2, triggering `Acid_Valve` = True. | **Pass:** Sequence steps execute in chronological order. |
| **OQ-CIP-002** | Verify Thermal Invalidation Guard and Fault Lockout during Step 2. | Force system into Step 2.<br>Force `Process_Temp` = 75.0°C (Below 80°C threshold).<br>Wait > 60 seconds. | System immediately drops all outputs (`Pump_Command`, `Acid_Valve`, `Water_Valve` = False).<br>Sequence jumps to `Step 99`.<br>`Fault_Active` = True. | **Pass:** Critical thermal deviation drops power and locks system. |
| **OQ-CIP-003** | Verify dynamic Purity Verification transition boundary. | Force system into Step 3.<br>Force `Conductivity` = 5.0 µS/cm.<br>Simulate flush by dropping `Conductivity` to 1.1 µS/cm. | While conductivity > 1.5, system stays locked in Step 3.<br>When conductivity drops to 1.1, system transitions to Step 4, then cleanly returns to Step 0. | **Pass:** System relies on physical purity chemistry, not a static timer. |
| **OQ-CIP-004** | Verify physical Overfill Safety Interlock loop. | Run system at any step (e.g., Step 1 or 2).<br>Force `Tank_Level_High` = True (`I0.0` high). | System instantly drops all digital outputs.<br>Sequence forces into `Step 99` within a single PLC program scan. | **Pass:** Hardwired high-level switch completely overrides sequence. |

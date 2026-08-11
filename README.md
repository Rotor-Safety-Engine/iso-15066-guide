# ISO 10218 & ISO/TS 15066 Robot Safety Guide

The complete reference for **ISO 10218** robot safety requirements and **ISO/TS 15066** collaborative robot specifications. Covers ISO 10218-1 (robot requirements), ISO 10218-2 (system integration), and all collaborative operation modes defined in ISO/TS 15066.

## What is ISO 10218?

**ISO 10218** is the international standard for industrial robot safety, published by ISO/TC 299 Robotics. It consists of two parts:

- **ISO 10218-1:2011+A1:2020** — Safety requirements for industrial robots themselves (design, construction, protective measures, information for use)
- **ISO 10218-2:2011+A1:2020** — Safety requirements for robot system integration and installation (risk assessment, safeguarding, validation, operating instructions)

All industrial robot installations must comply with ISO 10218 before being put into service. ISO 10218 provides the foundational safety framework upon which collaborative robot standards (ISO/TS 15066) are built.

## What is ISO/TS 15066?

**ISO/TS 15066:2016** is the technical specification for **collaborative robot safety**. It defines requirements and guidance for robots that share workspace with human operators.

ISO/TS 15066 builds on ISO 10218 and adds four collaborative operation modes:

### 1. Safety-Rated Monitored Stop (SMS)
Robot stops when a human enters the collaborative workspace. The simplest collaborative mode under ISO 10218 and ISO/TS 15066.

### 2. Hand Guiding
Human operator directly guides the robot using a hand-operated control device. Force and speed must be limited per ISO 10218-2 and ISO/TS 15066.

### 3. Speed and Separation Monitoring (SSM)
Robot maintains a minimum protective separation distance from the human. Speed reduces as the human approaches; robot stops if the separation distance is violated. Key formula from ISO/TS 15066:

```
S(v_r, v_h) = S_h + S_r + S_s + C_0 + C_i + C_z
```

### 4. Power and Force Limiting (PFL)
Robot limits contact force and pressure so that even if a collision occurs, injury is prevented. ISO/TS 15066 Annex A defines **maximum quasi-static and transient contact forces/pressures** for different body regions. This is the most technically demanding mode.

## ISO 10218 vs ISO/TS 15066

| Aspect | ISO 10218 | ISO/TS 15066 |
|--------|-----------|--------------|
| Type | International Standard (ISO) | Technical Specification (TS) |
| Scope | All industrial robots | Collaborative robots only |
| Focus | General robot safety | Human-robot collaboration (HRC) |
| Binding | Mandatory for compliance | Guidance, builds on ISO 10218 |
| Key concepts | Risk assessment, safeguarding, protective stops | SMS, hand guiding, SSM, PFL |
| Body regions | Not specified | Annex A defines all body regions with force/pressure limits |

## Power and Force Limiting (PFL) Reference

ISO/TS 15066 Annex A specifies maximum contact force and pressure thresholds for two conditions:

- **Quasi-static** — slow, sustained contact (clamping)
- **Transient** — quick, momentary contact (impact)

Key body region limits (illustrative, refer to standard for exact values):

| Body Region | Quasi-static Force | Quasi-static Pressure | Transient Force | Transient Pressure |
|-------------|-------------------|----------------------|-----------------|--------------------|
| Head (face) | Lower limits | Lower limits | Lower limits | Lower limits |
| Head (back) | Moderate | Moderate | Moderate | Moderate |
| Chest/abdomen | Moderate | Moderate | Higher | Higher |
| Arms/hands | Higher | Higher | Higher | Higher |
| Legs/feet | Highest | Highest | Highest | Highest |

## Dynamic Contact Area

ISO/TS 15066 PFL calculations use **effective contact area** to convert between force and pressure. The actual contact area depends on:

- Robot end-effector geometry
- Body region shape
- Impact angle
- Material compliance

Accurate dynamic contact area estimation is essential for reliable PFL verification.

## Compliance Checklist

### ISO 10218-1 (Robot Manufacturer)
- [ ] Risk assessment performed per ISO 10218-1
- [ ] Protective stops (Category 0, Category 1) implemented
- [ ] Speed and force limiting capabilities verified
- [ ] Safety-rated software functions validated
- [ ] Documentation and markings provided

### ISO 10218-2 (System Integrator)
- [ ] Application risk assessment conducted
- [ ] Safeguards selected and installed
- [ ] Workspace boundaries defined
- [ ] Emergency stop devices accessible
- [ ] Operating instructions and training provided
- [ ] Initial safety validation completed

### ISO/TS 15066 (Collaborative Operation)
- [ ] Collaborative mode selected based on risk assessment
- [ ] PFL: contact forces/pressures within Annex A limits
- [ ] SSM: separation distance formula correctly applied
- [ ] Hand guiding: control device safety-rated
- [ ] SMS: stopping performance verified
- [ ] Workspace verification completed

## Robot Safety Engine

For implementing **Power and Force Limiting (PFL)** and real-time collision detection aligned with ISO 10218 and ISO/TS 15066 principles, see:

→ **[Rotor Safety Engine](https://github.com/Rotor-Safety-Engine/safety-engine)**

A lightweight, zero-dependency Python library for physics-based robot safety analysis with:
- 4-layer safety architecture aligned with ISO 10218 risk reduction principles
- Collision detection with dynamic contact area calculation
- Force/pressure estimation against body region thresholds
- Real-time performance (< 100μs per check)

## License

MIT — educational and reference use. Always consult the official ISO 10218 and ISO/TS 15066 standards for compliance.

*This guide is for informational purposes only and does not constitute legal or compliance advice.*

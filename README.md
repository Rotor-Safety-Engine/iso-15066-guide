# ISO 10218 & ISO/TS 15066 Robot Safety Guide

The complete reference for **ISO 10218** industrial robot safety standards and **ISO/TS 15066** collaborative robot specifications, with practical implementation guidance for **deterministic safety systems**, **dynamic contact area modeling**, and **Power and Force Limiting (PFL)** verification.

---

## What is ISO 10218?

**ISO 10218** is the international standard for industrial robot safety, published by ISO/TC 299 Robotics. It is a **binding international standard** (not a technical specification), and all industrial robot installations must comply with ISO 10218 before being put into service.

It consists of two parts:

- **ISO 10218-1:2011+A1:2020** — Safety requirements for industrial robots themselves (design, construction, protective measures, information for use)
- **ISO 10218-2:2011+A1:2020** — Safety requirements for robot system integration and installation (risk assessment, safeguarding, validation, operating instructions)

ISO 10218 is the foundational safety framework upon which all collaborative robot standards (including ISO/TS 15066) are built. A key principle of ISO 10218 is that **safety functions must be deterministic and verifiable** — probabilistic approaches are not acceptable for safety-rated functions, because safety requires 100% predictable behavior, not statistical confidence.

---

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

The challenge with SSM in practice is that detection latency directly translates into required separation distance. A **deterministic, sub-millisecond safety layer** (compared to cloud-based perception at hundreds of milliseconds) can dramatically reduce the required safety distance.

### 4. Power and Force Limiting (PFL)
Robot limits contact force and pressure so that even if a collision occurs, injury is prevented. ISO/TS 15066 Annex A defines **maximum quasi-static and transient contact forces/pressures** for different body regions. This is the most technically demanding mode — and the one most relevant to **VLA safety** and **humanoid robot safety**.

> **Implementation note**: PFL compliance requires accurate contact force/pressure estimation in real time. Many existing systems use fixed contact area values, but the actual contact area changes dynamically during impact based on material stiffness, impact velocity, and body region geometry. A **dynamic contact area model** is essential for reliable PFL verification — overestimating area leads to unsafe conditions, while underestimating leads to overly conservative operation.

---

## ISO 10218 vs ISO/TS 15066

| Aspect | ISO 10218 | ISO/TS 15066 |
|--------|-----------|--------------|
| Type | International Standard (ISO) — **binding** | Technical Specification (TS) — **guidance** |
| Scope | All industrial robots | Collaborative robots only |
| Focus | General robot safety framework | Human-robot collaboration (HRC) specifics |
| Legal status | Mandatory for compliance | Builds on ISO 10218, adds HRC guidance |
| Key concepts | Risk assessment, safeguarding, protective stops | SMS, hand guiding, SSM, PFL |
| Body regions | General requirements | Annex A defines force/pressure limits per body region |
| VLA / humanoid applicability | Foundational requirements | PFL mode most directly relevant |

> **Bottom line**: ISO 10218 is the main standard; ISO/TS 15066 is the collaborative add-on. For any robot safety system, **ISO 10218 compliance comes first** — ISO/TS 15066 is an additional layer for collaborative operation.

---

## Power and Force Limiting (PFL) Reference

ISO/TS 15066 Annex A specifies maximum contact force and pressure thresholds for two conditions:

- **Quasi-static** — slow, sustained contact (clamping / entrapment)
- **Transient** — quick, momentary contact (impact / collision)

### Why dynamic contact area matters for PFL

The conversion between force and pressure depends entirely on the **effective contact area**:

```
Pressure = Force / Contact_Area
```

ISO/TS 15066 defines pressure limits per body region, but the actual contact area during a collision is not a constant. It depends on:

- Robot end-effector geometry and material stiffness
- Body region shape and tissue compliance
- Impact angle and velocity
- Deformation under load (softer materials = larger contact area = lower pressure)

Traditional safety systems often use a fixed (worst-case) contact area for simplicity. This approach has two problems:
1. **For soft impacts, it's too conservative** — the actual area is larger, pressure is lower, so the system trips unnecessarily
2. **For stiff impacts, it may be unsafe** — if the assumed area is larger than reality, pressure is underestimated

A **dynamic contact area calculation**, based on contact stiffness and impact energy, gives a more accurate real-time estimate. This is particularly important for **humanoid robots** and **VLA-controlled systems**, where interaction with diverse objects and environments means contact conditions are not pre-defined.

---

## Deterministic Safety vs. Probabilistic Perception

ISO 10218 requires safety functions to be **verifiable and repeatable**. This principle has important implications for modern AI-based robot systems:

| Approach | Safety Suitability | ISO 10218 Alignment | Use Case |
|----------|-------------------|---------------------|----------|
| **Deterministic physics-based** | ✅ Suitable for safety functions | Aligned — verifiable, repeatable | Real-time safety interlock, PFL verification |
| Probabilistic ML / VLA perception | ⚠️ Not for safety functions alone | Challenging — statistical, hard to verify | Perception, planning, high-level control |
| Hybrid (VLA + deterministic safety layer) | ✅ Best of both worlds | Aligned — safety layer provides deterministic boundary | VLA / humanoid robot systems |

The recommended architecture for **VLA safety** follows ISO 10218's risk reduction principle: the VLA model handles perception and planning, while a **deterministic physics safety layer** acts as the final safety gate. This is analogous to how industrial robots have safety-rated controllers separate from the main motion planner.

---

## Compliance Checklist

### ISO 10218-1 (Robot Manufacturer)
- [ ] Risk assessment performed per ISO 10218-1
- [ ] Protective stops (Category 0, Category 1) implemented
- [ ] Speed and force limiting capabilities verified
- [ ] Safety-rated software functions validated
- [ ] Deterministic behavior verified (same input → same output)
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
- [ ] PFL: contact area model validated for worst-case scenarios
- [ ] SSM: separation distance formula correctly applied
- [ ] Hand guiding: control device safety-rated
- [ ] SMS: stopping performance verified
- [ ] Workspace verification completed

---

## Rotor Safety Engine — Deterministic PFL Implementation

For teams implementing **Power and Force Limiting (PFL)** per ISO 10218 and ISO/TS 15066, or building safety infrastructure for **VLA models** and **humanoid robots**:

→ **[Rotor Safety Engine](https://github.com/Rotor-Safety-Engine/safety-engine)**

A lightweight, zero-dependency physics-based safety library designed around ISO 10218's deterministic safety principles:

- **100% deterministic** — pure Newtonian mechanics, no probability, no ML, same input always produces same output
- **Dynamic contact area** — real-time calculation based on contact stiffness and force, not fixed-area approximation
- **4-layer safety architecture** — semantic validation → safety parameter mapping → action classification → comprehensive decision
- **7-level risk granularity** — beyond binary safe/unsafe, provides over_ratio for progressive safety feedback
- **Sub-millisecond latency** — ~17μs per check in Python, suitable for real-time control loops
- **Single file · zero dependencies** — drop into any project, no installation required
- **ISO 10218 / ISO/TS 15066 aligned** — body-region-aware force/pressure estimation with PFL verification

Whether you're building a collaborative robot, a humanoid robot, or a VLA-based control system, Rotor provides the **deterministic safety boundary** that ISO 10218 requires — without adding complexity or dependencies.

---

## License

MIT — educational and reference use. Always consult the official ISO 10218 and ISO/TS 15066 standards for compliance.

*This guide is for informational purposes only and does not constitute legal or compliance advice.*

# CircuitWorkbench IEC 61131-3 Structured Text IDE for Eclipse (`st-eclipse-ide`)

[![License: BSL 1.1](https://img.shields.io/badge/License-BSL_1.1-blue.svg)](LICENSE.md)
[![Eclipse Version](https://img.shields.io/badge/Eclipse-2024--03%2B-orange.svg)](https://www.eclipse.org/)
[![Java Compatibility](https://img.shields.io/badge/Java-17%20%7C%2021-brightgreen.svg)](https://adoptium.net/)

An industrial-grade development, simulation, and verification toolchain for IEC 61131-3 Structured Text (ST), delivered as an **Eclipse IDE extension** and a **headless CLI test harness**.

Built specifically for protection engineers, utility automation specialists, and control system developers working on mission-critical PLC logic, substation automation (ATS, load shedding, breaker control), and automated CI/CD verification pipelines.

---

## Key Features

- **Rich Structured Text Language Tooling:** Full AST-based syntax checking, cross-file symbol indexing across your entire Eclipse workspace/ResourceSet, live code mining reference counts, and inline parameter hints.
- **Deterministic Virtual Simulation Engine:** High-performance, cycle-based interpreter supporting native timers (`TON`, `TOF`, `TP`), math, trigonometry, ramp generators, and PID controllers without requiring target hardware.
- **Hierarchical PLC Force Dashboard:** Inspect live telemetry in real time. Drill down through nested function block instances and complex structures (e.g., `StationFeed1.BreakerClosed`) with one-click variable overrides and status highlights.
- **Headless CLI Test Runner:** Run regression suites, test benches, and simulation assertions directly in command-line environments and GitHub Actions / GitLab CI pipelines.
- **Code Generation & Portability:** Transpile verified control routines to ANSI C or export compliant PLCopen XML for direct import into vendor-specific engineering suites (Siemens TIA Portal, Rockwell Studio 5000, CODESYS).

---

## System Requirements

- **Java Runtime Environment:** OpenJDK or Oracle JDK 17 or 21+.
- **Eclipse IDE Base:** Eclipse IDE 2024-03 (4.31) or newer.
  - *Recommended packages:* Eclipse IDE for Java Developers, Eclipse Modeling Tools, or Eclipse Classic.
- **Operating Systems:** Windows 10/11 (x64), Linux (x86_64 / AArch64), or macOS (Apple Silicon / Intel).

---

## Installation into Eclipse

You can install the CircuitWorkbench Structured Text toolchain into an existing Eclipse installation via the p2 Update Site:

### Method 1: Eclipse Update Site (Recommended)

1. Launch your Eclipse IDE.
2. In the top menu, go to **Help** $\rightarrow$ **Install New Software...**
3. Click the **Add...** button in the upper-right corner.
4. Enter the repository details:
   - **Name:** `CircuitWorkbench ST Update Site`
   - **Location:** `https://raw.githubusercontent.com/circuitworkbench/st-eclipse-ide/main/repository` *(or specify a local archive if using an offline zip)*
5. Click **Add**.
6. Select the **CircuitWorkbench Structured Text Feature** checkbox:
   - `Structured Text IDE (Core Edition)`
   - `Structured Text IDE (Pro Edition)`
7. Click **Next**, review the installation details, accept the license terms, and click **Finish**.
8. Restart Eclipse when prompted.

### Method 2: Verifying the Installation

1. Switch to the **Structured Text Perspective** by navigating to:
   - **Window** $\rightarrow$ **Perspective** $\rightarrow$ **Open Perspective** $\rightarrow$ **Other...** $\rightarrow$ **Structured Text**.
2. Right-click in the **Project Explorer** $\rightarrow$ **New** $\rightarrow$ **Structured Text Project**.
3. Create a `.st` file (e.g., `StationServiceATS.st`) to verify syntax highlighting and code minings.

---

## Using the PLC Force Dashboard

The interactive **PLC Force Dashboard** enables engineers to monitor live simulation values and lock tags during execution:

1. Launch a simulation or debug session by right-clicking your `.st` file or project and choosing **Run As** $\rightarrow$ **PLC Simulation** (or **Debug As** $\rightarrow$ **PLC Simulation**).
2. Open the view via **Window** $\rightarrow$ **Show View** $\rightarrow$ **Other...** $\rightarrow$ **PLC** $\rightarrow$ **PLC Force Dashboard**.
3. **Structured Types & Complex Tags:**
   - Structs (e.g., `StationFeed1`) display expandable chevron toggles (`>`).
   - Expanding the root object exposes nested members such as `Healthy`, `Voltage_RMS`, and `BreakerClosed`.
4. **Applying Forces:**
   - Select any variable or nested leaf tag in the viewer.
   - Click **Force Variable...** in the view toolbar.
   - Enter your target value (`TRUE`, `FALSE`, or numeric literals) to lock the value across execution cycles. Forced tags are highlighted in red with status `FORCED`.
   - Click **Unforce Variable** or **Unforce All** to restore standard programmatic control.

---

## Automated CLI & CI/CD Test Runner

For headless builds and CI/CD pipelines, execute tests using the standalone native runner without launching the Eclipse GUI:

### Windows Command Prompt / PowerShell

```cmd
:: Using an environment variable for the license key
set CIRCUITWORKBENCH_LICENSE_KEY=YOUR_LICENSE_KEY_HERE
st-test-runnerc.exe --source-dir .\substation-logic --report-dir .\build\test-results

:: Passing parameters directly
st-test-runnerc.exe ^
  --source-dir .\substation-logic ^
  --report-dir .\build\test-results ^
  --max-cycles 5000 ^
  --cycle-delta 20 ^
  --license-key YOUR_LICENSE_KEY_HERE

---

## Licensing & StructuredText Pro

StructuredText Pro feature is released under the **Business Source License 1.1 (BSL 1.1)**. 

- **Evaluation & Non-Production:** Free of charge for personal educational study, evaluation, local unit testing, and open non-commercial experiments.
- **Production & Commercial Pipelines:** Commercial engineering work, industrial control verification, transpiler outputs used in production, and CI/CD runner pipelines require an active **CircuitWorkbench Pro** subscription.

### How to Obtain a Pro License Key

1. **Purchase a Subscription:**
   Visit the [CircuitWorkbench Lemon Squeezy Storefront](https://circuitworkbench.lemonsqueezy.com/checkout/buy/a01cb0a1-e3bf-453c-843c-ccd63e819069) to purchase a license.
2. **Retrieve Your Key:**
   Upon checkout completion, your order confirmation email will contain your unique UUID license key (formatted as `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`).

### Activating Your License

Choose one of three activation methods:

#### 1. Inside the Eclipse IDE (Graphical)
- Open **Window** $\rightarrow$ **Preferences** (or **Eclipse** $\rightarrow$ **Settings...** on macOS).
- Navigate to **StructuredText** $\rightarrow$ **Licensing**.
- Paste your license key into the **License Key** field and click **Activate/Refresh**.
- The IDE will contact the activation service and cache an offline entitlement token valid for 3 days between connectivity syncs.
- Click **Apply and Close**

#### 2. Via Environment Variable (Ideal for Docker & CI/CD Runners)
Set the environment variable prior to starting Eclipse or running the headless CLI:
```bash
export CIRCUITWORKBENCH_LICENSE_KEY="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

#### 3. Via Eclipse / Java VM Arguments
Add the flag to your `eclipse.ini` file or pass it to the command-line JVM launch:
```text
-Dcircuitworkbench.license.key=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

---

## Contributing & Bug Reports

- **Issue Tracker:** Submit bug reports, unexpected transpilation behaviors, or feature requests via [GitHub Issues](https://github.com/circuitworkbench/st-eclipse-ide/issues).
- **Discussions:** Ask technical questions regarding IEC 61131-3 syntax support, AST configurations, or simulation timing on [GitHub Discussions](https://github.com/circuitworkbench/st-eclipse-ide/discussions).

---

## License

This software is distributed under the terms of the **Business Source License 1.1**. See [LICENSE.md](LICENSE.md) for full terms and conditions.

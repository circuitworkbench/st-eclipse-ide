# CircuitWorkbench IEC 61131-3 Structured Text IDE for Eclipse (`st-eclipse-ide`)

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
- **Eclipse IDE Base (for IDE extension):** Eclipse IDE 2024-03 (4.31) or newer.
  - *Recommended packages:* Eclipse IDE for Java Developers, Eclipse Modeling Tools, or Eclipse Classic.
- **Operating Systems:** Windows 10/11 (x64), Linux (x86_64), or macOS (x86_64 / Apple Silicon via Rosetta or native JVM).

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

For headless verification suites and automated CI/CD pipelines, execute tests using the standalone native runner without launching the Eclipse GUI.

### Downloading the CLI Executable Packages

Standalone pre-built packages for Windows, Linux, and macOS are published under GitHub Releases:  
🔗 **[CircuitWorkbench ST CLI v1.0.0 Release](https://github.com/circuitworkbench/st-eclipse-ide/releases/tag/cli-v1.0.0)**

Download the archive matching your target operating system:

| Platform | Architecture | Release Package Archive | Executable Binary |
| :--- | :--- | :--- | :--- |
| **Windows** | x86_64 | `com.circuitworkbench.st.cli.product-win32.win32.x86_64.zip` | `st-test-runnerc.exe` |
| **Linux** | x86_64 | `com.circuitworkbench.st.cli.product-linux.gtk.x86_64.tar.gz` | `st-test-runnerc` |
| **macOS** | x86_64 | `com.circuitworkbench.st.cli.product-macosx.cocoa.x86_64.tar.gz` | `st-test-runnerc` |

> **Note for Windows:** The archive provides both `st-test-runner.exe` (GUI subsystem) and `st-test-runnerc.exe` (Console subsystem). Always execute **`st-test-runnerc.exe`** in Command Prompt, PowerShell, or CI scripts to synchronously capture terminal output, test progress logs, and process exit codes.

---

### Installation & Quick Start

#### Windows (Command Prompt / PowerShell)

1. Download and extract `com.circuitworkbench.st.cli.product-win32.win32.x86_64.zip`.
2. Open Command Prompt or PowerShell and navigate to the extracted directory.
3. Run the test runner:

```cmd
:: Set license key via environment variable (recommended for CI/CD)
set CIRCUITWORKBENCH_LICENSE_KEY=YOUR_LICENSE_KEY_HERE
st-test-runnerc.exe --source-dir .\substation-logic --report-dir .\build\test-results

:: Or pass parameters directly via CLI arguments
st-test-runnerc.exe ^
  --source-dir .\substation-logic ^
  --report-dir .\build\test-results ^
  --max-cycles 5000 ^
  --cycle-delta 20 ^
  --license-key YOUR_LICENSE_KEY_HERE
```

#### Linux & macOS (Bash / Terminal)

1. Download and extract the archive:
   ```bash
   # Linux
   tar -xzf com.circuitworkbench.st.cli.product-linux.gtk.x86_64.tar.gz
   
   # macOS
   tar -xzf com.circuitworkbench.st.cli.product-macosx.cocoa.x86_64.tar.gz
   ```
2. Navigate to the extracted directory and ensure execution permissions are granted:
   ```bash
   chmod +x st-test-runnerc
   ```
3. Run the test runner:
   ```bash
   # Set license key via environment variable
   export CIRCUITWORKBENCH_LICENSE_KEY="YOUR_LICENSE_KEY_HERE"
   ./st-test-runnerc --source-dir ./substation-logic --report-dir ./build/test-results

   # Or pass parameters directly via CLI arguments
   ./st-test-runnerc \
     --source-dir ./substation-logic \
     --report-dir ./build/test-results \
     --max-cycles 5000 \
     --cycle-delta 20 \
     --license-key "YOUR_LICENSE_KEY_HERE"
   ```

---

### Command-Line Arguments

| Argument | Description | Default |
| :--- | :--- | :--- |
| `--source-dir <path>` | Directory path containing `.st` source and automated test suite files | `.` |
| `--report-dir <path>` | Destination folder where the JUnit XML report (`TEST-plc-results.xml`) is written | `./build/test-results` |
| `--max-cycles <int>` | Maximum scan cycles per test before terminating | `100` |
| `--cycle-delta <ms>` | Simulated scan cycle interval in milliseconds (advances runtime timers like `TON`) | `50` |
| `--license-key <key>` | Pro license key (can also be supplied via `CIRCUITWORKBENCH_LICENSE_KEY`) | None |

### Exit Codes & CI Integration

* **`0`**: All discovered test POUs passed successfully.
* **`1`**: Test assertion failure, syntax/linking validation error, invalid argument, or missing Pro license entitlement.
* **Test Reports**: Automatically writes standard JUnit XML results to `<report-dir>/TEST-plc-results.xml` for native ingestion into GitHub Actions test reporters, GitLab CI reports, or Jenkins.

---

## Licensing & Editions

CircuitWorkbench Structured Text is distributed under proprietary binary licensing across two editions:

- **Core Edition (Freeware):** Free of charge for development, syntax validation, AST analysis, and local non-simulation editing.
- **Pro Edition (Commercial Subscription):** Commercial engineering work, industrial control verification, virtual PLC cycle simulation, interactive Force Dashboard overrides, transpiler outputs, and headless CI/CD runner pipelines require an active **CircuitWorkbench Pro** subscription.

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
- Click **Apply and Close**.

#### 2. Via Environment Variable (Ideal for Docker & CI/CD Runners)
Set the environment variable prior to starting Eclipse or running the headless CLI:
```bash
export CIRCUITWORKBENCH_LICENSE_KEY="YOUR_LICENSE_KEY_HERE"
```

#### 3. Via Command-Line Argument / JVM System Properties
Pass the license key directly to the CLI test launcher:
```bash
./st-test-runnerc --license-key "YOUR_LICENSE_KEY_HERE"
```
Or append it to your `eclipse.ini` / Java VM launch flags:
```text
-Dcircuitworkbench.license.key=YOUR_LICENSE_KEY_HERE
```

---

## Contributing & Bug Reports

- **Issue Tracker:** Submit bug reports, unexpected transpilation behaviors, or feature requests via [GitHub Issues](https://github.com/circuitworkbench/st-eclipse-ide/issues).
- **Discussions:** Ask technical questions regarding IEC 61131-3 syntax support, AST configurations, or simulation timing on [GitHub Discussions](https://github.com/circuitworkbench/st-eclipse-ide/discussions).

---

## License

This software is distributed under the terms of the **CircuitWorkbench End User License Agreement**. See `about.html` in each feature distribution for edition-specific terms and conditions.

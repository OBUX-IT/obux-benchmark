# OBUX Benchmark & Bootstrapper

**Version 0.2.0** • *Last updated: May 2025*

A complete suite for measuring and reporting system and user‐interaction performance, plus a lightweight updater/launcher to keep you on the latest build.

---

## 📖 Table of Contents

1. [What’s Inside This Release](#whats-inside-this-release)
2. [Key Features](#key-features)
3. [Prerequisites](#prerequisites)
4. [Installation & Usage](#installation--usage)

   * [GUI Mode](#gui-mode)
   * [Silent Mode](#silent-mode)
   * [Command-Line Arguments](#command-line-arguments)
   * [Manual Installation (Optional)](#manual-installation-optional)
5. [How It Works](#how-it-works)
6. [Troubleshooting](#troubleshooting)
7. [Support & Contributing](#support--contributing)

---

## 🗂 What’s Inside This Release

* **OBUXBootstrapper.zip**
  Contains the updater/launcher (`OBUXBootstrapper.exe`), which:

  * Checks GitHub for the latest build
  * Downloads & installs into `C:\Program Files\OBUX`
  * Shows a progress UI (or runs fully headless)
  * Launches the benchmark automatically

* **OBUXBenchmark.zip**
  The benchmark package. After extraction to `C:\Program Files\OBUX`, its folder structure looks like:

  ```
  C:\Program Files\OBUX\
  ├─ Wrapper\OBUX Benchmark.exe
  ├─ Insight\Control\LoadGen.Cloud.Client.Control.exe
  ├─ Insight\Insight\LoadGen.Cloud.Client.Insight.exe
  └─ Launcher\DNKLocalClient.exe
  ```

---

## ✨ Key Features

* **Automatic Updates**
  Always run the latest benchmark without manual downloads.
* **Two‐Mode Operation**

  * **GUI**: friendly progress window with status messages.
  * **Silent**: headless console mode for automation.
* **Progress Reporting**
  Both console and GUI get real‐time download/extract percentages.
* **Compatibility Comparison**
  Side‐by‐side comparison of scoring formulas v0.1 vs. v0.2.
* **Customizable Benchmark Runs**
  Pass email, benchmark name, data‐sharing consent, and insight‐interval via CLI.

---

## 🛠 Prerequisites

* **Operating System**: Windows 10 or later
* **Full .NET Framework 4.8**

  * If missing, **OBUX Benchmark.exe** will detect and auto-install it.
* **Visual C++ Redistributable (2015–2019 x64)**

  * Also auto-installed by **OBUX Benchmark.exe** if not present.
* **Internet Access** to GitHub (for checking/updating builds)

> The installer silently installs any missing dependencies, so simply launching the bootstrapper handles everything.

---

## 🚀 Installation & Usage

### GUI Mode

1. **Download** `OBUXBootstrapper.zip` and extract it anywhere.
2. **Run** `OBUXBootstrapper.exe` (double-click).
3. The window shows “Spinning up…” then:

   * **Version check** against GitHub commits
   * **Download** with a progress bar
   * **Extraction** into `C:\Program Files\OBUX`
   * **Launch** of `OBUX Benchmark.exe`

### Silent Mode

For unattended automation, open a **Command Prompt** and run:

```bat
OBUXBootstrapper.exe /silent
```

You’ll see console updates like:

```
Spinning up bootstrapper…
Downloading… 23%
Downloading… 100%
Download complete.
Starting extraction…
Extracting… 100%
Extraction complete.
Launching OBUX Benchmark…
```

*No GUI window appears*.

### Command-Line Arguments

You can also pass benchmark parameters in silent mode:

```bat
OBUXBootstrapper.exe /silent ^
  /email:you@domain.com ^
  /benchmark:MyTestRun ^
  /sharedata:true ^
  /insightinterval:5
```

* `/email:<address>` – included in final results
* `/benchmark:<name>` – identifier for this run
* `/sharedata:true|false` – opt in/out of anonymous telemetry
* `/insightinterval:<sec>` – logging interval for Insight tool

In **GUI mode**, these flags prefill the form but still display the window.

### Manual Installation (Optional)

1. Extract **OBUXBenchmark.zip** into `C:\Program Files\OBUX`.
2. Run `C:\Program Files\OBUX\Wrapper\OBUX Benchmark.exe`.
3. Fill in email, benchmark name, toggle data sharing, and start.

---

## ⚙️ How It Works

1. **Version Discovery**

   * Calls GitHub’s API to find the latest commit message beginning with `build N`.
   * Compares `N` to the local assembly’s revision number.
2. **Download**

   * Streams the ZIP file (`OBUXBenchmark.zip`) with chunked reads.
   * Reports percentage via `IProgress<int>` (GUI progress bar) and console.
3. **Extraction**

   * Deletes old installation folder if present.
   * Uses `System.IO.Compression.ZipFile` on a background thread.
   * Reports extraction progress similarly.
4. **Launch**

   * Starts `OBUX Benchmark.exe` with the configured parameters.

Inside the benchmark:

* **System Scores** (CPU, IO, total) via `GrandTotalCalculator`.
* **User Scores** (A-category, B-category, total) via `UserScoreCalculator`.
* **Comparison Table** between v0.1 and v0.2 scoring formulas in the results UI.

---

## 🛠 Troubleshooting

* **403 Forbidden** fetching GitHub commits

  * Ensure the bootstrapper sends a `User-Agent` header (it does by default).
* **“Zip file in use”** on extract

  * Close any running `OBUX Benchmark.exe` before updating.
* **No console output in silent mode**

  * Silent mode always writes to `Console.WriteLine`, even when GUI is hidden.
* **Prerequisites not found**

  * The benchmark exe auto-installs .NET 4.8 and VC++ runtimes on first run.

---

## 🤝 Support & Contributing

* **Report issues** or suggest features on [GitHub Issues](https://github.com/OBUX-IT/obux-benchmark/issues).

Thank you for using **OBUX Benchmark**!

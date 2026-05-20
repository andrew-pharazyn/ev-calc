# Interactive EV Cost Simulator - Technical Reference

This document serves as a comprehensive reference guide for the **Interactive EV Cost Simulator** application, detailing its mathematical model, software architecture, user interface structure, and historical modifications.

---

## 📂 Project Structure

* **Main Application Entrypoint**: `c:/Antigravity_Projects/ev_calc/evanalyzer.html`
* **Artifacts Directory**: `C:/Users/aphar/.gemini/antigravity/brain/2054ae40-5c83-4f7a-b196-516100519528/`
  * [implementation_plan.md](file:///C:/Users/aphar/.gemini/antigravity/brain/2054ae40-5c83-4f7a-b196-516100519528/implementation_plan.md)
  * [task.md](file:///C:/Users/aphar/.gemini/antigravity/brain/2054ae40-5c83-4f7a-b196-516100519528/task.md)
  * [walkthrough.md](file:///C:/Users/aphar/.gemini/antigravity/brain/2054ae40-5c83-4f7a-b196-516100519528/walkthrough.md)

---

## 🧮 Mathematical Model & Formulas

The application simulates financial and environmental projections over a **10-year timeline** (Years 0 to 10). Year 0 represents the starting baseline/transaction values. Years 1–10 simulate recurring annual costs adjusted for compound inflation.

### 1. Annual Operational Costs

All baseline operational costs are compounded annually using the user-specified **Annual Price Inflation Rate ($I$)**:
$$\text{Inflated Cost}_y = \text{Baseline Cost} \times (1 + I)^y$$

* **Gasoline Operating Cost (Annual)**:
  $$\text{Fuel Cost}_y = \left( \frac{\text{Annual Distance}}{\text{ICE MPG}} \times \text{Gas Price} \times (1 + I)^y \right)$$
  $$\text{Gas Ops Cost}_y = \text{Fuel Cost}_y + (\text{ICE Maintenance} + \text{ICE Insurance}) \times (1 + I)^y$$

* **Electric Vehicle Operating Cost (Annual)**:
  EV efficiency is subject to battery degradation ($D$), which reduces efficiency year-over-year:
  $$\text{Degradation Factor}_y = (1 - D)^{y-1}$$
  $$\text{Electricity Cost}_y = \left( \frac{\text{Annual Distance}}{\text{EV Efficiency} \times \text{Degradation Factor}_y} \times \text{Elec Rate} \times (1 + I)^y \right)$$
  $$\text{EV Ops Cost}_y = \text{Electricity Cost}_y + (\text{EV Maintenance} + \text{EV Insurance}) \times (1 + I)^y$$

---

### 2. Vehicle Financing & Amortization

When financing is enabled, monthly amortization calculations are executed to separate principal repayments from interest losses.

#### Monthly Annuity Formula:
For a loan with Principal $P$, Annual Interest Rate $r$ (monthly rate $R = r / 12 / 100$), and Term $N$ (months):
$$\text{Monthly Payment} = P \times \frac{R(1+R)^N}{(1+R)^N - 1}$$

#### Amortization Loop:
For each month $m$ (from 1 to $N$):
$$\text{Monthly Interest}_m = \text{Remaining Balance}_{m-1} \times R$$
$$\text{Monthly Principal}_m = \text{Monthly Payment} - \text{Monthly Interest}_m$$
$$\text{Remaining Balance}_m = \text{Remaining Balance}_{m-1} - \text{Monthly Principal}_m$$

#### Symmetrical Modes:
* **Cash Flow Mode (Depreciation OFF)**:
  * **Day 1 (Year 0)**: EV starting cost is reduced to the down payment plus transaction fees:
    $$\text{EV Initial Cost} = \text{Down Payment} + \text{Charger Cost} - \text{Tax Credits}$$
  * **Years 1–10**: The full monthly loan payment (Principal + Interest) is added to the year's cash outflow.
* **Net Wealth Mode (Depreciation ON)**:
  * **Day 1 (Year 0)**: Assets and liabilities are evaluated. Debt is balanced against cash, starting Day 1 at the net transaction cost:
    $$\text{EV Initial Cost} = \text{Charger Cost} - \text{Tax Credits}$$
    $$\text{Net Value Difference} = (\text{ICE Value} - \text{ICE Outstanding Debt}) - (\text{EV Value} - \text{EV Outstanding Debt})$$
  * **Years 1–10**: Only the **Interest Paid** is added to the year's cumulative cost timeline (representing pure wealth loss), while the vehicle's depreciating asset value is tracked.

---

### 3. Vehicle Depreciation (Wealth Mode)

Vehicle asset values depreciate exponentially based on the model year ($Y_{\text{model}}$), baseline rate ($d$), and simulation year ($y$):
$$\text{Age}_y = (\text{Current Year} - Y_{\text{model}}) + y$$
$$\text{Depreciated Value}_y = \text{Sticker Price} \times (1 - d)^{\text{Age}_y}$$

---

## 🎨 UI/UX Features & Design System

The application is styled using a modern, custom dark-mode theme utilizing **Tailwind CSS** and **Vanilla CSS**:

* **Aesthetic Palette**:
  * Primary Highlights: `emerald-500` (success/EV accent/toggles), `indigo-500` (charts/primary actions), and `amber-500` (warning/ICE accent).
  * Background: Slate/zinc gradients (`bg-[#030712]`) with dynamic glassmorphism panels.
* **ICE Financing Disable & Tooltip**:
  * If the user sets the **ICE Outstanding Loan** to `$0` (or leaves it blank), the ICE Loan interest rate and remaining term inputs are programmatically disabled (`disabled = true`).
  * A semi-transparent overlay (`overlay-iceLoan`) with backdrop blur covers the inputs. Hovering or clicking on this overlay reveals a floating tooltip: **"⚠️ No outstanding loan on this vehicle"**.
* **Visual Graph Dashboard**:
  * Features a custom SVG rendering responsive gas vs. EV cumulative cost curves.
  * Real-time crossover marker highlighting the exact month/year the EV pays for itself.

---

## 🛠️ Summary of Historical Work & Bug Fixes

1. **Financing Calculations & Amortization Integration**: Added the monthly loan amortization math and split operational logic between Cash Flow (out-of-pocket) and Net Wealth (asset depreciation + interest).
2. **Financing Control Group Refinement**:
   * Synchronized checkbox accent colors to match the depreciation toggle (`emerald-500`).
   * Implemented the outstanding loan overlay & hover/click tooltip warning when ICE outstanding debt is zero.
3. **HTML Structural Alignment Bug Fix**:
   * Fixed a missing closing `</div>` tag at the end of the Depreciation explanation section.
   * This bug was forcing the browser to nest the right-hand dashboard panel inside the left-hand column, breaking the desktop layout into a single-column stacked view. Correct alignment has been fully restored.

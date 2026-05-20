# Interactive EV Cost Simulator 🚗🔋

An advanced, client-side interactive financial calculator designed to provide an analytical comparison between operating a conventional Gasoline (ICE) vehicle and switching to an Electric Vehicle (EV). 

This tool goes beyond surface-level fuel savings, integrating complex long-term economic factors such as compounding price inflation, multi-stage vehicle asset depreciation, dynamic financing frameworks, and battery capacity decay over a 10-year horizon.

---

## 📊 Key Analytical Features

The simulator operates across a 10-year longitudinal timeline model, executing synchronous recalculations across three core operational parameters:

### 1. Advanced Cost Modeling & Configuration
* **Dynamic Baseline Controls:** Real-time range inputs for annual mileage, per-gallon fuel costs, and per-kWh utility rates.
* **Compound Price Inflation Engine:** Applies compounding annual percentage rate (APR) adjustments across fuel, utility, insurance, and maintenance parameters over a 10-year timeline.
* **Battery Capacity Decay Simulator:** Dynamically models annual battery health degradation using a declining balance multiplier ($mi/kWh \times (1 - \text{rate})^y$), reflecting the rising cost per mile of grid charging over time.

### 2. Double-Mode Asset & Wealth Accounting
Users can toggle between two distinct financial valuation paradigms:
* **Cash Flow Mode (Default):** Evaluates raw wallet liquidity, capturing the immediate capital impact of purchase costs, down payments, and total monthly loan obligations (principal + interest).
* **Net Wealth (Depreciation) Mode:** Treats vehicles as depreciating balance sheet assets rather than immediate sunk expenses. Day 1 capital starts at transaction overhead friction (e.g., charger installation fees minus tax incentives). Subsequent years accumulate actual vehicle valuation losses based on age-weighted declining curves adjusted by mileage factors.

### 3. Integrated Financing & Loan Amortization
* **Dual-Vehicle Amortization Schedules:** Simulates independent financing terms for an existing ICE vehicle loan and a newly structured EV loan.
* **Real-time Amortization Engine:** Executes continuous monthly compound math loop tracking interest vs. principal splits, reporting calculated monthly payments and cumulative interest destruction lines.
* **Smart State Overlays:** Automatically grays out and disables inputs using contextual logic if a vehicle has no outstanding debt.

### 4. Interactive Data Dashboard & Charts
* **Bezier Curve Time-Series Charting:** Custom-engineered vector graphic (SVG) canvas mapping the cumulative financial trajectories of both vehicles.
* **Crossover Intersection Interpolation:** Analytically calculates and overlays exact timeline coordinates ($x, y$) pinpointing the precise payback threshold.
* **High-Fidelity Track Tooltips:** Features a low-latency hover tracking pipeline that samples cursor pixel coordinates and interpolates year-by-year cost differentials.
* **Carbon Offset Equivalents:** Translates raw carbon reductions into digestible ecological metrics, computing the number of mature trees required to replicate the impact and total fuel avoided.

---

## 🛠️ Technical Stack Architecture

The interface is built strictly as an atomic, zero-dependency client-side platform to maximize performance, visual fluidness, and ease of distribution:

* **Markup Structure:** Clean semantic HTML5 structured linearly to align seamlessly with paged media and responsive viewports.
* **Styling Engine:** Streamlined utility classes powered by **Tailwind CSS v4** paired with native CSS custom elements for optimized component rendering.
* **Typography & Aesthetics:** Styled with a premium dark-mode dashboard color palette leveraging the `Outfit` font for bold analytical geometric headings and `Inter` for micro-legibility.
* **Logic Engine:** Pure, performant **Vanilla JS**. Zero external runtime frameworks (No React, Vue, or jQuery), guaranteeing instant load times, zero build overhead, and lightning-fast calculations.

---

## 💻 Mathematical Logic Implementation

The core computational logic runs inside a synchronous execution loop triggered by any user interaction. Key algebraic and financial loops include:

### 1. Loan Amortization
For a given interest rate $r$, loan balance $P$, and term in months $n$, the monthly payment $M$ is derived via:

$$M = P \frac{i(1+i)^n}{(1+i)^n - 1}$$

where $i = \frac{r}{1200}$. A 120-month sub-loop distributes payments across chronological timeline years, cleanly dividing wealth-reducing interest expense from balance sheet principal liquidation.

### 2. Dynamic Age-Weighted Depreciation
Depreciation rates are scaled dynamically by an age-sensitive step function combined with a continuous mileage factor:

$$\text{Depreciation Rate}_y = \text{Base Rate} \times f(\text{Age}_y) \times \left(0.5 + \frac{\text{Annual Mileage}}{24000}\right)$$

* **Age Multipliers:** Year 1 = $1.5\times$, Years 2–3 = $1.2\times$, Years 4–6 = $0.8\times$, Years 7+ = $0.5\times$.

---

## 🚀 Getting Started & Deployment

Because the calculator is completely self-contained within a single document, deployment requires no local server environment or build steps:

1. **Clone / Download:** Save `switching-to-ev-calculator.html` to your local storage.
2. **Run Locally:** Double-click the file to open it directly in modern web browsers (Chrome, Safari, Firefox, Edge).
3. **Deploy to GitHub Pages:**
   * Push the file to any GitHub repository.
   * Navigate to **Settings > Pages**.
   * Set the build source to your branch root and click save to launch an instant, globally accessible URL.

---

## 👤 Author

* **Creator:** Andrew Pharazyn

---

*Disclaimer: This tool provides analytical simulations based on general economic theories and mathematical formulas. Actual operational costs may vary based on specific driving habits, regional utility tariffs, vehicle health variables, and macroeconomic factors.*

*AI Usage: This tool was mostly built as a test of using the new Gemini 3.5 Flash model in Antigravity 2.0 announced at Google I/O 2026 to take a spreadsheet I had made and turn it into an interactive website, which also lead to me drastically fleshing out new features.*















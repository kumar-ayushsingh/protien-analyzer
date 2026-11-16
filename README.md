# 🧬 Protein Sequence Analyzer 3D (PSA 3D)

A modern, high-performance, client-side bioinformatics web application for analyzing protein sequences, simulating 3D polypeptide chain formation, and exploring experimental and AI-predicted 3D biomolecular structures.

🚀 **Live Demo:** [https://kumar-ayushsingh.github.io/protien-analyzer/](https://kumar-ayushsingh.github.io/protien-analyzer/)

---

## 🌟 Key Highlights

- **Zero Backend & Offline-Ready:** Entirely client-side execution in standard HTML5, CSS3, and Vanilla JavaScript. Runs directly in any web browser or via `file://`.
- **High-Performance $O(n)$ Processing:** Instantaneous calculations and responsive rendering even on massive chains (tested up to **35,000+ residues**, e.g., Titin-scale benchmark).
- **Interactive 3D Visualizations:**
  - **Dynamic 3D Landing Page:** Ambient floating $\alpha$-helices and molecular particles built with Three.js.
  - **3D Polypeptide Formation Simulation:** Translation-style residue elongation model illustrating chain assembly before detailed analysis.
  - **Real 3D Structure Viewer:** Interactive 3D molecular viewer (3Dmol.js) supporting RCSB PDB, AlphaFold DB, and ESMFold.
- **Scientific Workstation UI/UX:** Dark-mode-first aesthetic with a collapsible left sidebar, bento overview dashboard, synchronized color palettes, and light/dark theme toggles.

---

## 🔬 Core Capabilities & Features

### 1. Robust Sequence Validation & Input
- **Flexible Input Methods:** Accepts raw pasted sequence text, standard FASTA/multi-FASTA format, or drag-and-drop file uploads (`.fasta`, `.fa`, `.txt`).
- **Cleaning & Validation:** Automatically strips FASTA headers (`>...`), whitespace, digits, and line breaks while uppercasing residues.
- **Error & Warning Safeguards:**
  - Validates strictly against the 20 standard amino acids (`ACDEFGHIKLMNPQRSTVWY`).
  - Detects and categorizes non-standard/ambiguous characters (`B`, `Z`, `X`, `U`, `O`, `J`, `*`, `-`) with inline count badges and excludes them from property calculations without crashing.
  - Gracefully handles empty, too-short (< 2 residues), or completely invalid sequences with clear error banners.
  - Multi-FASTA selector lets users inspect and switch between records seamlessly.

### 2. Physicochemical Properties & Analysis
- **Molecular Weight ($M_w$):** Sum of residue average masses plus terminal water ($18.01524\text{ Da}$).
- **Isoelectric Point ($pI$):** Calculated via the Henderson-Hasselbalch equation solved by bisection over $[0, 14]$ using the EMBOSS pKa set.
- **Net Charge:** Live calculation of net electrical charge at user-selectable pH (default $7.0$, adjustable $0 - 14$).
- **GRAVY (Grand Average of Hydropathy):** Mean hydropathy score across all residues using the Kyte-Doolittle scale.
- **Aliphatic Index:** Relative volume occupied by aliphatic side chains (Ikai, 1980):
  $$\text{Aliphatic Index} = X(A) + 2.9 \cdot X(V) + 3.9 \cdot [X(I) + X(L)]$$
- **Instability Index Consideration:** Omitted rather than approximated, maintaining scientific rigor.

### 3. Switchable Amino-Acid Grouping Models
Toggle between three grouping models with synchronized colors across charts, legends, tables, and 3D scenes:
1. **Side-chain property:**
   - Hydrophobic (`A`, `V`, `L`, `I`, `M`, `F`, `W`, `P`)
   - Polar uncharged (`S`, `T`, `N`, `Q`, `C`, `Y`, `G`)
   - Positive / Basic (`K`, `R`, `H`)
   - Negative / Acidic (`D`, `E`)
2. **Size:**
   - Small (`G`, `A`, `S`, `C`, `T`, `P`, `D`, `N`, `V`)
   - Medium (`E`, `Q`, `H`, `M`, `I`, `L`, `K`, `R`)
   - Large (`F`, `W`, `Y`)
3. **Structure / Function:**
   - Aromatic (`F`, `W`, `Y`), Sulfur-containing (`C`, `M`), Aliphatic (`A`, `V`, `L`, `I`), Acidic (`D`, `E`), Basic (`K`, `R`, `H`), Other (`G`, `N`, `P`, `Q`, `S`, `T`).

### 4. Sliding-Window Hydrophobicity Profile
- Running $O(n)$ Kyte-Doolittle hydropathy profile with user-adjustable window size ($5 - 21$, default $9$).
- Peak-preserving min/max downsampling ($\le 2,000$ points) to prevent browser stutter on ultra-long sequences while preserving sharp hydropathy transitions.

### 5. 3D Polypeptide Formation Simulation
- Animated Three.js viewport showing nascent chain elongation residue-by-residue.
- Residues rendered as spheres colored according to the selected grouping model, connected by peptide bonds.
- **Controls:** Play, Pause, Replay, Restart, Scrub slider, Speed multiplier ($0.25\times - 8\times$), and 3D Orbit/Zoom/Pan.
- For sequences $> 1,000$ residues, utilizes `THREE.InstancedMesh` and accelerated fast-forwarding to maintain 60 FPS.
- Labeled prominently: *"Illustrative model, not a predicted structure."*

### 6. Real 3D Structure Viewer (3Dmol.js)
- **Experimental Structures (RCSB PDB):** Fetch atomic coordinate PDB files directly by 4-character PDB ID (e.g., `1UBQ`, `1EMA`).
- **AlphaFold DB Predictions:** Query AlphaFold Database API via UniProt accession (e.g., `P0CG48`, `P42212`) and dynamically load the predicted structure.
- **AI Sequence Folding (ESMFold):** Submit sequences directly to the Meta ESMFold API for on-the-fly prediction ($\le 400\text{ residues}$).
- **Rendering Modes:** Cartoon, Stick, and Van der Waals Surface styles.
- **Controls:** Color-mapped to active amino-acid grouping model, continuous rotation (Spin), and camera reset.
- **Resilience:** 30-second network timeout and isolated error banners that never crash the main application.

### 7. Export & Reproducibility
- **CSV Report:** Export sequence header, length, valid residues, excluded characters, MW, pI, net charge, GRAVY, aliphatic index, grouping breakdown, and composition to `.csv`.
- **Chart Images:** One-click high-resolution PNG download for all charts with solid background fills.

---

## 🧪 Built-in Example Benchmark Proteins

| Protein | Length | Reference ID | Description |
| :--- | :---: | :---: | :--- |
| **Human Insulin A Chain** | 21 aa | UniProt `P01308` | Short regulatory hormone peptide chain |
| **Human Insulin B Chain** | 30 aa | UniProt `P01308` | Complementary insulin polypeptide chain |
| **Ubiquitin** | 76 aa | PDB `1UBQ` / UniProt `P0CG48` | Highly conserved eukaryotic regulatory protein |
| **Green Fluorescent Protein (GFP)** | 238 aa | PDB `1EMA` / UniProt `P42212` | Widely studied fluorescent bioluminescent marker |
| **Synthetic Long Chain Benchmark** | 35,000 aa | Stress Test | Deterministic Titin-scale chain to verify $O(n)$ speed |

---

## 🛠️ Technology Stack

- **Core:** Semantic HTML5, Modular CSS3 (Custom design system with dark/light themes), Vanilla JavaScript (ES6+).
- **Graphics & 3D:**
  - [Three.js](https://threejs.org/) (r147) & OrbitControls — 3D background animation and chain elongation simulation.
  - [3Dmol.js](https://3dmol.csb.pitt.edu/) (v2.4.2) — Hardware-accelerated molecular graphics for PDB/AlphaFold/ESMFold.
- **Data Visualization:** [Chart.js](https://www.chartjs.org/) (v4.4.7) — Composition bar charts, group breakdown donuts, and hydrophobicity profiles.
- **External Bio APIs:**
  - [RCSB Protein Data Bank](https://www.rcsb.org/)
  - [AlphaFold Protein Structure Database (EMBL-EBI)](https://alphafold.ebi.ac.uk/)
  - [ESM Metagenomic Atlas (ESMFold)](https://esmatlas.com/)

---

## 📁 Repository Structure

```text
protien-analyzer/
├── index.html          # Main application page (Landing page + Workspace dashboard)
├── styles.css          # Full styling: dark/light theme, scientific bento layout, sidebar
├── js/
│   ├── constants.js    # Molecular weights, pKa values, KD hydropathy, grouping models, examples
│   ├── analyzer.js     # O(n) parser, validation, statistics, bisection pI, sliding window, CSV export
│   ├── charts.js       # Chart.js renderers (composition bar, group donut, hydropathy curve)
│   ├── landing.js      # Three.js floating alpha-helix background animation for landing page
│   ├── simulation.js   # Three.js residue-by-residue polypeptide formation simulation
│   ├── structure.js    # 3Dmol.js structure fetcher (RCSB, AlphaFold DB, ESMFold) & viewer
│   └── app.js          # Main application controller, UI event handling, state management
└── README.md           # Project documentation and deployment guide
```

---

## 💻 Getting Started Locally

No Node.js build steps, compilers, or server installations are required!

1. **Clone the repository:**
   ```bash
   git clone https://github.com/kumar-ayushsingh/protien-analyzer.git
   cd protien-analyzer
   ```

2. **Open the application:**
   - Simply double-click `index.html` to open it directly in Google Chrome, Microsoft Edge, Firefox, or Safari.
   - Or serve with any static web server:
     ```bash
     # Using Python 3
     python -m http.server 8000
     ```
     Then navigate to `http://localhost:8000`.

---

## 📜 References & Methodology

- **Kyte & Doolittle (1982):** A simple method for displaying the hydropathic character of a protein. *J. Mol. Biol.*, 157(1), 105-132.
- **EMBOSS pKa Values:** Rice, P., Longden, I., & Bleasby, A. (2000). EMBOSS: The European Molecular Biology Open Software Suite. *Trends in Genetics*, 16(6), 276-277.
- **Ikai, A. (1980):** Thermostability and aliphatic index of globular proteins. *J. Biochem.*, 88(6), 1895-1898.
- **ExPASy ProtParam:** Gasteiger, E., et al. (2005). Protein Identification and Analysis Tools on the ExPASy Server. *The Proteomics Protocols Handbook*, 571-607.

---

## 👤 Author

Developed by **[kumar-ayushsingh](https://github.com/kumar-ayushsingh)**.  
Project Repository: [https://github.com/kumar-ayushsingh/protien-analyzer](https://github.com/kumar-ayushsingh/protien-analyzer)

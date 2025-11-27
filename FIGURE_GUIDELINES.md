# Figure Guidelines for Thesis

## How Figures Work in This Thesis

The thesis is already configured to automatically generate a **List of Figures** that appears in the front matter (after Table of Contents). Any figure you add with a `\caption{}` will automatically be included in this list.

## LaTeX Figure Syntax

### Basic Figure Template

```latex
\begin{figure}[htbp]
    \centering
    \includegraphics[width=0.8\textwidth]{Images/filename.png}
    \caption{Your descriptive caption here. This text will appear in the List of Figures.}
    \label{fig:short_reference_name}
\end{figure}
```

### Explanation of Components

- `[htbp]`: Placement specifiers (h=here, t=top, b=bottom, p=page of floats)
- `\centering`: Centers the figure
- `width=0.8\textwidth`: Makes figure 80% of text width (adjust as needed: 0.5, 0.6, 0.9, 1.0)
- `Images/filename.png`: Path to your image file (relative to main.tex)
- `\caption{}`: Description that appears below figure AND in List of Figures
- `\label{fig:...}`: Reference label for citing the figure in text

### Referencing Figures in Text

```latex
As shown in Figure~\ref{fig:pipeline_overview}, the system consists of three stages.
```

The `~` creates a non-breaking space so "Figure" and the number stay together.

## Figure Naming Conventions

### File Names (in Images/ directory)
- `ch1_blood_smear_example.png`
- `ch1_rotation_invariance_demo.png`
- `ch1_pipeline_architecture.png`
- `ch2_maskrcnn_architecture.png`
- `ch3_reynet_architecture.png`
- `ch4_results_comparison.png`

### Label Names (in LaTeX)
- `fig:ch1_blood_smear`
- `fig:ch1_rotation_demo`
- `fig:ch1_pipeline`
- `fig:maskrcnn_arch`
- `fig:reynet_arch`
- `fig:results_comparison`

## Image Format Recommendations

- **Preferred formats**: PNG (for diagrams, screenshots), PDF (for vector graphics)
- **Resolution**: At least 300 DPI for publication quality
- **Size**: Images will be scaled to fit page width, but start with high resolution
- **Color**: Use color sparingly; ensure figures are readable in grayscale for printing

## Multiple Subfigures

For showing multiple related images:

```latex
\begin{figure}[htbp]
    \centering
    \begin{subfigure}[b]{0.45\textwidth}
        \centering
        \includegraphics[width=\textwidth]{Images/image1.png}
        \caption{First subfigure}
        \label{fig:sub1}
    \end{subfigure}
    \hfill
    \begin{subfigure}[b]{0.45\textwidth}
        \centering
        \includegraphics[width=\textwidth]{Images/image2.png}
        \caption{Second subfigure}
        \label{fig:sub2}
    \end{subfigure}
    \caption{Overall caption for both subfigures}
    \label{fig:combined}
\end{figure}
```

Note: This requires `\usepackage{subcaption}` or `\usepackage{subfig}` in main.tex

## Current Figure Placeholders in Chapter 1

### Figure 1.1: Representative Blood Smear Microscopy Image
- **Location**: Section 1.1 (Motivation)
- **Purpose**: Show the complexity of raw blood smear images
- **What to create**: A typical microscopy image showing dozens of cells scattered across the frame
- **Suggested content**: Label RBCs, WBCs, show infected vs healthy cells

### Figure 1.2: Rotation Invariance Challenge
- **Location**: Section 1.2 (Problem Statement - Challenge 3)
- **Purpose**: Illustrate why blood cells need rotation-invariant classification
- **What to create**: Same cell shown at 0°, 45°, 90°, 135°, 180° rotations
- **Suggested content**: 5 identical cells at different angles with caption explaining they should all be classified the same

### Figure 1.3: Pipeline Architecture Overview
- **Location**: Section 1.3 (Proposed Solution)
- **Purpose**: Visual summary of the three-stage pipeline
- **What to create**: Block diagram showing: Raw Image → Mask R-CNN → Single Cells → ReyNet/ResNet18 → Disease Report
- **Suggested content**: Include example images at each stage with arrows showing data flow

### Figure 1.4: Segmentation-Based Dataset Generation
- **Location**: Section 1.3.1 (Stage 1: Segmentation) OR Section 1.4 (Contribution 3)
- **Purpose**: Illustrate how segmentation creates synthetic single-cell dataset
- **What to create**: Before/After showing field-of-view image → extracted individual cell crops
- **Suggested content**: One blood smear with bounding boxes → grid of extracted single cells

## Tips for Creating Figures

### For Diagrams and Architecture Figures:
- Use tools like: draw.io, PowerPoint, TikZ (LaTeX), or Inkscape
- Keep text large enough to read when scaled down
- Use consistent colors and styles across all figures
- Export as PNG (300 DPI) or PDF (vector)

### For Result Plots:
- Use Python (matplotlib, seaborn) or MATLAB
- Save with: `plt.savefig('filename.png', dpi=300, bbox_inches='tight')`
- Include clear axis labels, legends, and grid lines
- Use color-blind friendly palettes

### For Annotated Images:
- Use image editing tools: GIMP, Photoshop, or Python (OpenCV, PIL)
- Add arrows, boxes, labels to highlight important features
- Ensure annotations are visible and not cluttered

## Checklist Before Adding a Figure

- [ ] Image file is in `Images/` directory
- [ ] Filename follows naming convention
- [ ] Figure has descriptive `\caption{}`
- [ ] Figure has unique `\label{fig:...}`
- [ ] Figure is referenced in text at least once
- [ ] Image resolution is at least 300 DPI
- [ ] Figure adds value (doesn't just repeat text)

## Common Issues and Fixes

**Figure appears on wrong page:**
- Try different placement specifiers: `[h]`, `[t]`, `[htbp]`, `[H]` (requires `\usepackage{float}`)

**Figure too large/small:**
- Adjust width: `width=0.5\textwidth`, `width=0.9\textwidth`, etc.
- Or use height: `height=6cm`

**Image not found error:**
- Check path is relative to main.tex: `Images/filename.png`
- Ensure image extension is included: `.png`, `.jpg`, `.pdf`
- Check file actually exists in Images/ directory

**List of Figures not updating:**
- Compile LaTeX twice (first pass collects labels, second pass uses them)
- Delete auxiliary files and recompile

## Example of Good Figure Caption

**Bad:** "Pipeline architecture."

**Good:** "Overview of the proposed three-stage pipeline for blood disease detection. Stage 1 uses Mask R-CNN to segment individual cells from raw blood smears. Stage 2 classifies cells using ReyNet (for RBCs) and ResNet18 (for WBCs). Stage 3 aggregates predictions to generate diagnostic reports with infection counts and spatial localization."

Good captions:
- Are self-contained (reader can understand without reading full text)
- Describe WHAT is shown and WHY it matters
- Point out key features to notice
- Are specific rather than generic

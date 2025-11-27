# Figures to Create for Chapter 1 - Introduction

This document lists all figures added to Chapter 1 with detailed specifications for creating them.

---

## Figure 1.1: Representative Blood Smear Microscopy Image

**File name:** `ch1_blood_smear_example.png`
**Location in chapter:** Section 1.1 (Motivation) - End of section
**Dimensions:** Width should be 70% of text width (will scale automatically)
**Purpose:** Show readers what a typical blood smear looks like and why it's complex

### What to Include:
- Real microscopy image of a blood smear (or representative sample)
- Should show 30-50 cells visible in the frame
- Mix of red blood cells (RBCs) and white blood cells (WBCs)
- Optional: Light annotations pointing out:
  - Red blood cells (circle 2-3 examples)
  - White blood cell (circle 1 example)
  - Infected cell if available (circle with "Infected RBC" label)

### How to Create:
1. Source a blood smear image from your dataset or a public dataset (ensure licensing allows use)
2. If annotating, use image editing software (PowerPoint, GIMP, Photoshop, or Python's matplotlib)
3. Add subtle arrows/boxes to highlight key features
4. Export as PNG at 300 DPI minimum
5. Save to `Images/ch1_blood_smear_example.png`

### Tools:
- Python: matplotlib, PIL, OpenCV
- PowerPoint: Insert image → Add shapes/text → Save as PNG (high resolution)
- GIMP/Photoshop: Use annotation tools

---

## Figure 1.2: Rotation Invariance Challenge

**File name:** `ch1_rotation_invariance_demo.png`
**Location in chapter:** Section 1.2 (Problem Statement) - After Challenge 3
**Dimensions:** Width should be 85% of text width
**Purpose:** Visually demonstrate that the same cell at different angles should get the same diagnosis

### What to Include:
- A single blood cell image shown at 5 different rotation angles
- Angles: 0°, 45°, 90°, 135°, 180°
- All 5 images should be the SAME cell, just rotated
- Arrange in a horizontal row or grid (2-3 per row)
- Label each image with its rotation angle
- Optional: Add text below saying "Same cell → Same diagnosis"

### How to Create:

**Python Method (Recommended):**
```python
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt

# Load a single cell image
cell_img = Image.open('single_cell.png')

# Create figure with 5 subplots
fig, axes = plt.subplots(1, 5, figsize=(15, 3))

angles = [0, 45, 90, 135, 180]
for i, angle in enumerate(angles):
    rotated = cell_img.rotate(angle, expand=True)
    axes[i].imshow(rotated)
    axes[i].set_title(f'{angle}°', fontsize=14)
    axes[i].axis('off')

plt.tight_layout()
plt.savefig('Images/ch1_rotation_invariance_demo.png', dpi=300, bbox_inches='tight')
```

**Alternative: PowerPoint Method:**
1. Insert the same cell image 5 times
2. Rotate each copy: Right-click → Rotate → Custom (enter angles)
3. Arrange horizontally
4. Add text boxes below each with angle labels
5. Export as PNG (File → Save As → PNG, max resolution)

---

## Figure 1.3: Pipeline Architecture Overview

**File name:** `ch1_pipeline_architecture.png`
**Location in chapter:** Section 1.3 (Proposed Solution) - Right after introductory paragraph
**Dimensions:** Width should be 100% of text width (full width)
**Purpose:** Give high-level visual overview of the entire three-stage system

### What to Include:

**Layout:** Horizontal flow diagram (left to right)

**Stage 1 - Segmentation:**
- Box labeled "Mask R-CNN"
- Input: Small blood smear image thumbnail
- Output: Same image with bounding boxes around cells
- Arrow pointing right

**Stage 2 - Classification:**
- Two parallel branches:
  - Top branch: "ReyNet (RBC)" box
  - Bottom branch: "ResNet18 (WBC)" box
- Input: Individual cell crops
- Output: Class labels (Healthy/Infected for RBC, Subtypes for WBC)
- Arrows pointing right

**Stage 3 - Aggregation:**
- Box labeled "Disease Detection & Counting"
- Output: Diagnostic report (can show example text or annotated image)

**Visual Elements:**
- Use different colors for each stage (e.g., blue, green, orange)
- Include small example images at each stage
- Add arrows showing data flow
- Label key outputs

### How to Create:

**PowerPoint/Draw.io Method (Easiest):**
1. Create horizontal layout with 3 main sections
2. Use rectangles for process boxes
3. Use arrows for connections
4. Insert small example images
5. Add text labels
6. Export as high-resolution PNG

**Python/Matplotlib Method:**
1. Create canvas with 3 columns
2. Use `plt.arrow()` for connectors
3. Use `plt.imshow()` for example images
4. Add text with `plt.text()`
5. Save at 300 DPI

**Draw.io Instructions:**
1. Go to app.diagrams.net
2. Use flowchart shapes
3. Add images: File → Import → Select images
4. Export: File → Export as → PNG (300 DPI)

---

## Figure 1.4: Segmentation-Driven Dataset Generation

**File name:** `ch1_dataset_generation.png`
**Location in chapter:** Section 1.3.1 (Stage 1: Segmentation) - End of subsection
**Dimensions:** Width should be 90% of text width
**Purpose:** Illustrate how segmentation automatically creates single-cell datasets

### What to Include:

**Layout:** Two-panel figure (left and right)

**Left Panel - "Input":**
- Blood smear image with bounding boxes around detected cells
- Show 10-20 cells with colorful bounding boxes
- Label: "Detected Cells in Field of View"

**Right Panel - "Output":**
- Grid of extracted individual cell crops (e.g., 4×6 grid = 24 cells)
- Each cell should be a small square crop
- Label: "Extracted Single-Cell Dataset"

**Visual Connection:**
- Arrow from left to right labeled "Automatic Extraction"
- Optional: Color-code 2-3 specific cells in left panel and show where they appear in right grid

### How to Create:

**Python Method:**
```python
import matplotlib.pyplot as plt
import matplotlib.patches as patches
from PIL import Image
import numpy as np

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(14, 6))

# Left panel: Blood smear with bounding boxes
blood_smear = Image.open('blood_smear.png')
ax1.imshow(blood_smear)
# Add bounding boxes (example coordinates)
for bbox in bounding_boxes:  # [(x, y, w, h), ...]
    rect = patches.Rectangle((bbox[0], bbox[1]), bbox[2], bbox[3],
                              linewidth=2, edgecolor='lime', facecolor='none')
    ax1.add_patch(rect)
ax1.set_title('Detected Cells in Field of View', fontsize=14)
ax1.axis('off')

# Right panel: Grid of extracted cells
cell_crops = [Image.open(f'cell_{i}.png') for i in range(24)]
rows, cols = 4, 6
combined = np.zeros((rows * cell_size, cols * cell_size, 3))
for i, cell in enumerate(cell_crops):
    r, c = i // cols, i % cols
    combined[r*cell_size:(r+1)*cell_size, c*cell_size:(c+1)*cell_size] = cell
ax2.imshow(combined.astype(np.uint8))
ax2.set_title('Extracted Single-Cell Dataset', fontsize=14)
ax2.axis('off')

plt.tight_layout()
plt.savefig('Images/ch1_dataset_generation.png', dpi=300, bbox_inches='tight')
```

**Manual Method:**
1. Take blood smear image
2. Use Mask R-CNN results or manually draw bounding boxes (PowerPoint rectangles)
3. Separately, crop individual cells and arrange in grid
4. Combine both in PowerPoint/GIMP as side-by-side panels
5. Export as PNG

---

## Quick Reference Summary

| Figure | File Name | Location | Key Content |
|--------|-----------|----------|-------------|
| 1.1 | `ch1_blood_smear_example.png` | Sec 1.1 | Annotated microscopy image showing cell complexity |
| 1.2 | `ch1_rotation_invariance_demo.png` | Sec 1.2 | Same cell at 5 rotation angles |
| 1.3 | `ch1_pipeline_architecture.png` | Sec 1.3 | Block diagram of 3-stage pipeline |
| 1.4 | `ch1_dataset_generation.png` | Sec 1.3.1 | Before/after of dataset extraction |

---

## Image Sources

### If You Have Your Own Data:
- Use actual images from your experiments
- Shows authenticity and matches your results
- Ensure you have permission/rights to use

### If You Need Sample Data:
- **Malaria Dataset:** [NIH Malaria Dataset](https://lhncbc.nlm.nih.gov/LHC-downloads/downloads.html#malaria-datasets)
- **General Blood Cells:** Search "blood cell microscopy dataset" on Kaggle
- **Create Synthetic:** Use image editing to create illustrative examples

### Important:
- Credit sources if using external images
- Ensure license permits use in thesis
- For Figure 1.2 (rotation), you MUST use your own or properly licensed image and rotate it yourself

---

## Testing Your Figures

Before finalizing, check:
1. **Resolution:** Zoom to 200% - text/details should still be clear
2. **Size:** File size 100KB-5MB is typical (not too small, not too large)
3. **Readability:** Can you read all labels without squinting?
4. **Colors:** If printed in grayscale, is everything still distinguishable?
5. **Consistency:** Do figures use similar fonts, colors, and styles?

---

## Compilation Check

After adding images to `Images/` folder:
1. Compile LaTeX document (run twice for proper figure numbering)
2. Check that figures appear in List of Figures
3. Verify figure references work (e.g., "Figure~\ref{fig:ch1_pipeline}")
4. Ensure figures appear on appropriate pages near their mentions

---

## Next Steps

1. Create/gather the 4 images listed above
2. Save them to `Images/` directory with exact filenames
3. Recompile LaTeX document
4. Review placement and adjust figure positions if needed (change `[htbp]` parameters)
5. Proceed to add figures for subsequent chapters following same pattern

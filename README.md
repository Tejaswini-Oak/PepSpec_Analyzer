# PepSpec Analyzer: Peptide MS/MS Spectrum Peak Annotator

PepSpec Analyzer is a command-line Python application engineered to automate the decoding, parsing, and structural annotation of tandem mass spectrometry (MS/MS) data. By computing theoretical peptide fragment ions and executing tolerance-based peak matching against raw data, the script isolates and graphically flags matched and unmatched fragment peaks.

## 📊 Pipeline Architecture & Workflow
- **Data Input Syntax:** Parses high-dimensional `mzXML` file matrices extracted via a standard command-line interface.
- **XML Stream Processing:** Leverages a memory-efficient `xml.etree.ElementTree.iterparse` stream loop to pinpoint explicit mass spectra scan numbers, bypassing the overhead of full DOM tree parsing.
- **Binary Array Transformation:** Extracts base64-encoded strings from the `<peaks>` element fields, decoding them directly into 32-bit floating-point numeric vectors with built-in big-endian byte-swapping array alignments.
- **Fragmentation Modeling:** Evaluates input primary peptide structures to calculate individual expected residue mass weights, outputting discrete $b$-ion ($N$-terminal fragment + 1 Da) and $y$-ion ($C$-terminal fragment + 19 Da) matrices.
- **Tolerance Matching & Noise Filtering:** Applies a baseline filtering rule ($Intensity \ge 5\%$ of $I_{max}$) to eliminate chemical background noise, matching candidate masses to calculated metrics using an error window of $\le 0.05$ m/z.

## 📁 Repository Contents
- `pepspec_analyzer.py`: Main executable file featuring automated data extraction blocks, ion modeling, peak processing logic, and visualization layers.

## 📈 Key Engineering Milestones & Metrics
- **Performance Optimizations:** Transitioning matrix conversion checks into low-level native array decoding structures reduced script footprint and eliminated execution runtime overhead.
- **Improved Annotation Quality:** Implementing a strict $5\%$ relative intensity threshold alongside custom 0.05 m/z mass drift window rules restricted false-positive annotations while yielding high peak matching precision.
- **Automated Visualization:** Embeds complete color-coded graphical plotting engines using `matplotlib`, dynamically sorting signal pools:
  - **Black lines:** Low-intensity or background unassigned signal noise pool.
  - **Red lines:** Matched $b$-ion series structural peaks.
  - **Blue lines:** Matched $y$-ion series structural peaks.

## 💻 CLI Usage & Syntax Execution

To run the peak mapping workflow, execute the script via the command line by supplying the target mzXML file path, desired scan index, and amino acid query:

```bash
python pepspec_analyzer.py <path_to_data.mzXML> <scan_number> <peptide_sequence>

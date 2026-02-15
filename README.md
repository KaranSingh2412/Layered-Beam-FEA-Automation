# Layered-Beam-FEA-Automation
Automated Abaqus Python framework for multi-layer beam analysis with CDP, interlayer bonding, and parametric studies.


# Unified Beam Analyzer

**One-file automated multi-layer beam analysis for Abaqus CAE**

---

## 🚀 Quick Start

```bash
abaqus cae script=unified_beam_analyzer.py
```

A GUI appears → Fill parameters → Click "RUN ANALYSIS" → Done!

---

## ✨ Features

✅ **GUI Input** - No manual scripting  
✅ **Multi-Layer Beams** - Up to 20 layers  
✅ **Interlayer Bonding** - Tie / Cohesive / Contact  
✅ **CDP Materials** - Concrete damage plasticity  
✅ **Auto Meshing** - Smart structured/free fallback  
✅ **Result Extraction** - Stress, deflection, frequencies  
✅ **CSV Export** - Professional results table  
✅ **Analytical Check** - Beam theory validation  

---

## 📋 Requirements

- Abaqus 6.14+ installed and in PATH
- That's it!

---

## 🎯 Example Usage

### Simple Cantilever Beam

**GUI Inputs**:
```
Beam: 1000 × 50 × 100 mm
Layers: 1
Material: Steel, 200000, 0.3, 7850
BC: Left=Fixed, Right=Free
Load: 10000 N, Y direction
```

**Outputs**:
- `BeamAnalysis.odb` - Full results
- `BeamAnalysis_results.csv` - Summary table

**Runtime**: ~5 minutes

---

### 3-Layer Sandwich Beam

**GUI Inputs**:
```
Beam: 1000 × 50 × 100 mm
Layers: 3
Thicknesses: 10, 80, 10
Materials:
  Steel-Top, 200000, 0.3, 7850
  Foam-Core, 50, 0.4, 100
  Steel-Bottom, 200000, 0.3, 7850
Bonding: Tie
BC: Left=Fixed, Right=Free
Load: 10000 N, Y direction
```

**Results**:
```csv
Max von Mises Stress,245.32,MPa
Max Deflection,2.3456,mm
```

**Runtime**: ~10 minutes

---

## 📊 GUI Parameter Guide

| Section | Key Inputs |
|---------|------------|
| **1. Analysis** | Linear Static / Nonlinear CDP |
| **2. Geometry** | Length, Width, Height (mm) |
| **3. Layers** | Number, Thicknesses (comma-separated) |
| **4. Materials** | Format: `Name, E(MPa), nu, density(kg/m³)` |
| **5. Bonding** | Tie (perfect) / Cohesive / Contact |
| **6. Mesh** | C3D8R/C3D20R, Seed size (mm) |
| **7. BC** | Left: Fixed/Pinned, Right: Free/Pinned |
| **8. Load** | Point/Distributed, Magnitude (N), Direction |
| **9. Modal** | ☑ Enable, Number of modes |
| **10. Output** | Job name, ☑ Extract, ☑ CSV, ☑ Validate |

---

## 🔧 Material Input Format

One line per layer:
```
MaterialName, E(MPa), nu, density(kg/m³)
```

**Examples**:
```
Steel, 200000, 0.3, 7850
Aluminum, 70000, 0.33, 2700
Concrete, 30000, 0.2, 2400
```

**CDP Toggle**: Check box to enable concrete damage plasticity for "concrete" named layers.

---

## ⚡ Mesh Recommendations

| Purpose | Seed Size | Elements/Length | Time |
|---------|-----------|-----------------|------|
| Quick check | 15 mm | 10 | 5 min |
| Standard | 10 mm | 20 | 10 min |
| Accurate | 5 mm | 40 | 30 min |

---

## 🛠️ Troubleshooting

| Problem | Solution |
|---------|----------|
| GUI doesn't open | Run: `abaqus cae script=...` not `python ...` |
| Material mismatch | # of materials must equal # of layers |
| Thickness error | Sum of thicknesses must equal total height |
| Mesh fails | Increase seed size to 15mm |
| Job fails | Check `BeamAnalysis.msg` file |

---

## 📁 Output Files

```
BeamAnalysis.odb          # Main results (open in Abaqus Viewer)
BeamAnalysis_results.csv  # Quick summary
BeamAnalysis_modal.odb    # Modal results (if enabled)
```

**View Results**:
```bash
abaqus viewer odb=BeamAnalysis.odb
```

---

## 📐 Analytical Validation

For **cantilever beams**, the script compares FEA to theory:

```
δ = P×L³/(3×E×I)
σ = P×L×c/I
```

**Output Example**:
```
DEFLECTION:
  Analytical: 2.3100 mm
  FEA:        2.3456 mm
  Error:      1.54% ✓

STRESS:
  Analytical: 240.00 MPa
  FEA:        245.32 MPa
  Error:      2.22% ✓
```

**✓ = Validated** (error < 10%)

---

## 🎯 Common Configurations

### Cantilever
```
Left BC: Fixed
Right BC: Free
```

### Simply Supported
```
Left BC: Pinned
Right BC: Pinned
```

### Load Types
- **Point Load**: Concentrated force at free end
- **Distributed**: Uniform pressure on top face

---

## 🔬 Advanced Features

### Interlayer Bonding

| Type | Use Case |
|------|----------|
| **Tie** | Perfect bond (welded, bonded) |
| **Cohesive** | Adhesive with damage/delamination |
| **Contact** | Frictional (μ=0.6), can slide/separate |

### Concrete Damage Plasticity

Auto-configured when:
- ☑ "Use CDP" is checked
- Material name contains "concrete"

Parameters:
- Compression hardening
- Tension softening  
- Damage evolution

---

## 💡 Tips

**Best Practices**:
- Start with coarse mesh → validate → refine
- Use C3D8R for standard analyses
- Enable analytical validation for cantilevers
- Check elements per layer ≥ 3

**Mesh Quality**:
```
Elements per layer = Total elements / Number of layers
Recommended: ≥ 3 elements per layer
```

---

## 📊 CSV Output Format

```csv
Result Type,Value,Unit
Max von Mises Stress,245.32,MPa
Max Deflection,2.3456,mm

Mode,Frequency,Hz
1,15.23,
2,95.45,
```

---

## 🔄 Workflow Summary

```
1. Run script → GUI opens
2. Fill 10 parameter sections
3. Click "RUN ANALYSIS"
4. Script executes:
   - Build geometry
   - Create materials (+ CDP)
   - Apply bonding
   - Generate mesh
   - Apply BC & loads
   - Run analysis
   - Extract results
   - Export CSV
   - Validate analytically
5. View BeamAnalysis.odb or .csv
```

**Total Time**: 5-30 minutes (depends on mesh)

---

## 📄 License

MIT License - Free for academic and commercial use

---

## 📞 Support

**Quick Checks**:
1. Is Abaqus in PATH? → `abaqus information=system`
2. GUI not showing? → Use `abaqus cae script=...`
3. Job failed? → Check `BeamAnalysis.msg`
4. High error? → Refine mesh (reduce seed size)

**Key Requirements**:
- Layers = Materials (count must match)
- Thicknesses sum = Total height
- At least 3 elements per layer

---

## 🎓 Citation

```bibtex
@software{unified_beam_analyzer,
  title={Unified Beam Analyzer},
  version={1.0},
  year={2026}
}
```

---

**Version**: 1.0.0  
**Compatible**: Abaqus 6.14+  
**Last Updated**: February 2026

---

**Ready to start?** → `abaqus cae script=unified_beam_analyzer.py` 🚀

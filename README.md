# CFD Simulation of Hydrogen Propellant Flow in Nuclear Thermal Propulsion Reactor Cores

**Thomas A. P. Gattiker**  
for ETH Zürich / PSI LRT, with special gratitude going to **Dr Ezequiel O. Fogliatto** for supporting the development of the CFD simulation.

CAD reconstruction & CFD simulation of a NERVA/NRX-A6 nuclear thermal propulsion reactor fuel cluster.

**OpenFOAM v13 · CFD · Conjugate Heat Transfer · Supercritical Hydrogen · Nuclear Thermal Propulsion · NERVA · NRX-A6 · FreeCAD · SALOME/Gmsh · ParaView**

This repository contains the principal technical artifacts from the thesis, including reconstructed CAD geometry, OpenFOAM CFD/CHT case files, alternative computational meshes, the thesis PDF, and errata.

- Thesis PDF — methodology, assumptions, results, and limitations.
- ERRATA — corrections to the Thesis PDF since the official submission.
- Presentation PDF — Original slides from defence presentation.
- `CAD.zip` — reconstructed NERVA/NRX-A-series fuel-cluster CAD.
- `CoolantChannel.zip` — OpenFOAM v13 multi-region CFD/CHT case for a single channel.
- `*_inch_unvs.zip` — alternative `.unv` mesh sets with different axial resolutions.

---

## Thesis — Licence: CC BY 4.0

The included thesis documents the archival reconstruction, numerical formulation, material properties, boundary conditions, power-distribution reconstruction, experimental comparison, and model limitations.

`ERRATA for Revision 3.txt` records identified corrections separately so that the historical thesis document itself remains unchanged.

The work should be interpreted as a **research reconstruction and thesis-scale numerical study**. Historical documentation is incomplete in places, and some geometry and model inputs therefore required reconstruction or inference.

For detailed methodology and limitations, refer to the thesis.

### Citation

If using material from this repository in academic or technical work, please cite the thesis and identify the repository as the accompanying CAD/CFD reconstruction archive.

### Presentation

The accompanying thesis defence presentation provides a condensed overview of the project's motivation, historical NRX-A6 reactor reconstruction, OpenFOAM v13 modelling workflow, and preliminary validation results. Particular attention is given to the provenance and limitations of historical material-property data, axial and radial power-distribution modelling, computational mesh constraints, and discrepancies between numerical predictions and experimental measurements. The presentation is preserved as an archival record of the original defence and may be read as a brief introduction to this project.

---

## CAD Files — Licence: CERN-OHL-P-2.0

`CAD.zip` contains reconstructed geometry of a late NERVA/NRX-A-series fuel cluster.

The models were created in **FreeCAD** from historical technical drawings, reports, photographs, dimensional information, and other archival documentation. They are **research reconstructions**, not original Westinghouse or Los Alamos CAD files.

Historical source material includes declassified Westinghouse documentation available through the University of North Texas Digital Library:

https://digital.library.unt.edu

### CAD File Formats

- `.FCStd` — native FreeCAD models.
- `.step` — neutral exports of the individual reconstructed components.
- `Full Cluster.FCStd` — complete reconstructed fuel-cluster assembly, splittable through the centerline.
- `Sixth Part Cluster.FCStd` — reduced 1/6-sector geometry developed for possible reduced-domain thermal/CFD analysis.

The reconstructed components include the fuel and unfueled elements, support block, tie rod and associated hardware, pyrolytic-graphite sleeve, support washer, corrosion-protection and insulating cups, and compressed pyrofoil/graphite stack ring.

Repeated parts in the full cluster are instantiated from the corresponding unique geometry rather than supplied as duplicate STEP files.

Where complete dimensional information was unavailable, geometry was reconstructed from additional archival evidence and, in some cases, photographic metrology. Not all support-block fillets were implemented because of geometric artifacts at the edges.

See the thesis and appendices for the reconstruction basis, assumptions, and limitations.

---

## OpenFOAM CFD Case — Licence: MIT

`CoolantChannel.zip` contains the **OpenFOAM v13** case used for the thesis simulations.

The model couples compressible hydrogen flow with heat conduction in the surrounding solid regions using a multi-region conjugate heat-transfer formulation.

Large mesh inputs are supplied separately rather than embedded in the case archive.

### Mesh Sets

Three alternative mesh sets are provided. Their names indicate the number of axial subdivisions per inch of physical length. For example, 20th_inch corresponds to 20 axial intervals per inch, 
i.e. a spacing of 0.05 in (1.27 mm). The .unv coordinates are stored in millimetres and are converted to metres during preprocessing by Allpre.

```text
16th_inch_unvs.zip
20th_inch_unvs.zip
24th_inch_unvs.zip
```

Each contains:

```text
Coolant.unv
FuelPipe.unv
SupportBlockPipe.unv
```

These represent alternative axial discretizations developed during the thesis.

### Preparing the Case

Extract `CoolantChannel.zip`, then extract **one** mesh set and copy its three `.unv` files into the root of the case directory.

The resulting structure should look like:

```text
CoolantChannel/
├── Coolant.unv
├── FuelPipe.unv
├── SupportBlockPipe.unv
├── 0/
├── constant/
├── system/
├── Allclean
├── Allpre
├── AllResume
├── Allrun
├── CRS
└── PartRetrieve
```

The filenames `Coolant.unv`, `FuelPipe.unv`, and `SupportBlockPipe.unv` are used directly by the preprocessing scripts and should therefore be retained.

Only one mesh set should be installed in the case directory at a time.

---

## Workflow Scripts

| Script | Purpose |
|---|---|
| `Allpre` | Imports the three `.unv` meshes with `ideasUnvToFoam`, applies region-specific patch definitions, runs `checkMesh`, stores mesh logs, and scales the imported geometry from mm to m. |
| `Allrun` | Decomposes the multi-region case, runs `foamMultiRun` on 16 MPI ranks, records solver output, reconstructs the latest result, and creates ParaView files. |
| `AllResume` | Resumes an already decomposed parallel simulation using 16 MPI ranks without repeating the initial decomposition. |
| `CRS` | Reconstructs all regions, creates ParaView files, and opens the coolant-region case for visualization. |
| `PartRetrieve` | Reconstructs a predefined selection of intermediate simulation times from the decomposed processor data. |
| `Allclean` | Removes generated logs, processor directories, post-processing output, dynamic code, visualization files, and generated time directories while preserving the base case. |

The scripts are preserved substantially as used during the thesis and may require adaptation to another machine, MPI configuration, or OpenFOAM installation.

### Typical Use

With **OpenFOAM v13** loaded and one mesh set placed in the case directory:

```bash
./Allpre
./Allrun
```

To continue an existing decomposed run:

```bash
./AllResume
```

For reconstruction/visualization:

```bash
./CRS
```

To return the case toward its pre-run state:

```bash
./Allclean
```

`Allclean` deletes generated simulation data. Back up any results that are still required before using it.

---



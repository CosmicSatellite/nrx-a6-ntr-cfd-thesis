# CFD Simulation of Hydrogen Propellant Flow in Nuclear Thermal Propulsion Reactor Cores

**Thomas A. P. Gattiker**  
ETH Zürich / Paul Scherrer Institute

CFD and CAD reconstruction of a NERVA/NRX-A6 nuclear thermal propulsion reactor fuel cluster.

**OpenFOAM v13 · CFD · Conjugate Heat Transfer · Supercritical Hydrogen · Nuclear Thermal Propulsion · NERVA · NRX-A6 · FreeCAD · SALOME/Gmsh · ParaView**

Reconstruction and simulation of historical NERVA/NRX-A6 reactor-core geometry from archival documentation, including CAD reconstruction, meshing, conjugate heat-transfer CFD and comparison with historical experimental data.

## CAD Files
This directory contains the reconstructed CAD geometry of a late NERVA/NRX-A-series fuel cluster used in the thesis.
The models were reconstructed in FreeCAD from historical technical drawings, reports, photographs, dimensional information, and other archival documentation. They are RESEARCH RECONSTRUCTIONS, and were created using declassified documentation by Westinghouse made public in the online library of the University of North Texas: https://digital.library.unt.edu
### File Formats
- .step — neutral CAD exports of the individual reconstructed components for use in other CAD software.
- Full Cluster.FCStd — native FreeCAD model containing the reconstructed complete fuel-cluster assembly - splitable through the centerline.
- Sixth Part Cluster.FCStd — reduced 1/6-sector cluster geometry developed for potential reduced-domain thermal/CFD analysis.
### Components
The STEP files contain the principal unique cluster components, including:
- fuel element(s)
- unfueled central element
- support block
- tie rod
- tie-rod annulus
- pyrolytic-graphite sleeve
- tie-rod cone
- support washer
- corrosion-protection cup
- insulating cup
- compressed pyrofoil & graphite stack ring

Repeated components in the full cluster are instantiated from the corresponding unique geometry rather than provided as duplicate STEP files.

### Notes on Fidelity

The geometry was reconstructed to represent the historical reactor hardware as faithfully as practical from the available documentation. Where complete dimensional information was unavailable, dimensions or geometry were reconstructed from additional archival evidence and, in some cases, photographic metrology. Not all fillets were implemented on the support block due to artifacts arising at the edges.

See the thesis and its appendices for the source basis, reconstruction methodology, assumptions, and limitations.

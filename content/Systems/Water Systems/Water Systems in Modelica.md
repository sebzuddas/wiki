---
aliases:
  - Modelica water systems
  - Water systems Modelica
---
When using tanks, it is important to define the number of ports. This can be done in the following way

```
Modelica.Fluid.Vessels.BaseClasses.VesselPortsData(diameter = 0.2, height = 0, zeta_out = 0, zeta_in = 1).
``` 

This sets out how high the port is, and how large it is. But it is not yet clear what zeta does. 

It is also important to define the medium with which the whole tank system operates - ie is it compressible. 
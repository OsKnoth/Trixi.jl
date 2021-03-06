# Overview of how to reproduce results in Euler-gravity paper

## Sec. 4.1.1, Table 2, EOC tests compressible Euler
**polydeg = 3**:
```julia
Trixi.convtest("examples/paper-self-gravitating-gas-dynamics/parameters_eoc_test_euler.toml", 4)
```

**polydeg = 4**:
```julia
Trixi.convtest("examples/paper-self-gravitating-gas-dynamics/parameters_eoc_test_euler.toml", 4, polydeg=4)
```

## Sec. 4.1.2, Table 3, EOC tests hyperbolic diffusion
**polydeg = 3**:
```julia
Trixi.convtest("examples/paper-self-gravitating-gas-dynamics/parameters_eoc_test_hyperbolic_diffusion.toml", 4)
```

**polydeg = 4**:
```julia
Trixi.convtest("examples/paper-self-gravitating-gas-dynamics/parameters_eoc_test_hyperbolic_diffusion.toml", 4, polydeg=4)
```

## Sec. 4.1.3, Table 4, EOC tests coupled Euler-gravity
**polydeg = 3**:
```julia
Trixi.convtest("examples/paper-self-gravitating-gas-dynamics/parameters_eoc_test_coupled_euler_gravity.toml", 4)
```

**polydeg = 4**:
```julia
Trixi.convtest("examples/paper-self-gravitating-gas-dynamics/parameters_eoc_test_coupled_euler_gravity.toml", 4, polydeg=4)
```

## Sec. 4.1.3, Table 5, EOC tests coupled Euler-gravity (update gravity once per step)
```julia
Trixi.convtest("examples/paper-self-gravitating-gas-dynamics/parameters_eoc_test_coupled_euler_gravity.toml", 4,
               update_gravity_once_per_stage=false)
```

## Sec. 4.2.1, Figures 3 + 5a, Jeans energies with Euler/CK45 and gravity/CK45
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_jeans_instability.toml")
```

## Sec. 4.2.1, Figure 4, Jeans energies with Euler/CK45 and gravity/CK45 (update gravity once per step)
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_jeans_instability.toml",
          update_gravity_once_per_stage=false)
```

## Sec. 4.2.1, Figure 5b, Jeans energies with Euler/CK45 and gravity/RK3S*
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_jeans_instability.toml",
          time_integration_scheme_gravity="timestep_gravity_erk52_3Sstar!", cfl_gravity=1.2)
```

## Sec. 4.2.1, Creating Jeans energies figures 3 and 4
One must also shrink the analysis interval in the above command, e.g.,
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_jeans_instability.toml",
          analysis_interval=1)
```
to generate necessary data for the plots to look nice. Then run the python
script with the analysis file from the run as input
```
./jeans_all_in_one.py analysis.dat
```
to generate the figure.

## Sec. 4.2.2, Figure 6, T=0.5, AMR meshes for Sedov + gravity
**T = 0.0 and T = 0.5:**
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_sedov_self_gravity.toml", t_end=0.5)
```

**T = 1.0:**
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_sedov_self_gravity.toml")
```

## Sec. 4.2.2, Figure 7a, T=0.5, Sedov + gravity with Euler/CK45 and gravity/RK3S*
**AMR mesh:**
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_sedov_self_gravity.toml", t_end=0.5)
```

**Uniform mesh:**
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_sedov_self_gravity.toml",
          amr_interval=0, initial_refinement_level=8, t_end=0.5)
```

## Sec. 4.2.2, Figure 7b, T=1.0, Sedov + gravity with Euler/CK45 and gravity/RK3S*
**AMR mesh:**
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_sedov_self_gravity.toml")
```

**Uniform mesh:**
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_sedov_self_gravity.toml",
          amr_interval=0, initial_refinement_level=8)
```

## Sec. 4.2.2, Table 6, Sedov + gravity, performance uniform vs. AMR
**AMR mesh:**
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_sedov_self_gravity.toml")
```

**Uniform mesh:**
```julia
Trixi.run("examples/paper-self-gravitating-gas-dynamics/parameters_sedov_self_gravity.toml",
          amr_interval=0, initial_refinement_level=8)
```

### To postprocess the solution files use
```bash
/postprocessing/trixi2vtk --nvisnodes 16 --format vti
```
Then use Paraview.

# OpenFold3 Galaxy wrapper (first iteration)

Galaxy tool wrapper for [OpenFold3](https://github.com/aqlaboratory/openfold-3), an
open-source (Apache 2.0) reproduction of AlphaFold3, for biomolecular structure
prediction. Inference runs in the official OpenFold3 Docker image, optionally
using the ColabFold MSA server. 

This is a work in progress (WIP). 

## Administrator / deployment notes

### Model/param cache

Model parameters must be pre-staged into a shared directory and exposed to the
job via the `OPENFOLD_CACHE` environment variable (defaults to `/data/openfold3`). 

### Runner configuration

The runner configuration is resolved at run time:

- If a `runner.yml` is present in `$OPENFOLD_CACHE`, it is used as-is.
- Otherwise the wrapper writes a default runner config (`predict` + `low_mem`
  presets) into the job directory.

The default's optional acceleration kernels can be toggled via environment
variables (both default to `false`):

| Variable            | Effect                                    |
| ------------------- | ----------------------------------------- |
| `OF3_USE_CUEQ`      | `use_cueq_triangle_kernels`               |
| `OF3_USE_DEEPSPEED` | `use_deepspeed_evo_attention`             |

To pin a custom runner configuration, place a `runner.yml` in `$OPENFOLD_CACHE`,
for example:

```yaml
model_update:
  presets:
    - "predict"
    - "low_mem"
  custom:
    settings:
      memory:
        eval:
          use_cueq_triangle_kernels: false
          use_deepspeed_evo_attention: false
```

# Converting Jupyter Notebooks to Quarto

Quarto can render `.ipynb` files directly, or you can convert them to `.qmd` for better version control and editing.

## Direct Rendering (No Conversion Required)

Quarto renders `.ipynb` files as-is:

```bash
quarto render notebook.ipynb
quarto render notebook.ipynb --to pdf
```

Cell outputs stored in the notebook are used unless `execute: enabled: true` forces re-execution.

## Converting .ipynb to .qmd

Use the Quarto CLI to convert:

```bash
quarto convert notebook.ipynb
# produces notebook.qmd
```

This extracts cell source into `{python}` code blocks and converts markdown cells to prose. Cell outputs are discarded — Quarto will re-execute the code.

## Key Differences: .ipynb vs .qmd

| Feature | .ipynb | .qmd |
| ------- | ------ | ---- |
| Execution | Jupyter kernel | Quarto + Jupyter kernel |
| Cell options | Cell metadata JSON | `#|` hashpipe comments |
| Version control | JSON (noisy diffs) | Plain text (clean diffs) |
| Edit experience | Jupyter UI | Any text editor |
| Inline outputs | Stored in file | Re-computed on render |

## Cell Option Migration

Jupyter cell metadata becomes hashpipe options in .qmd:

**Before (.ipynb cell metadata):**

```json
{
  "tags": ["remove-input"],
  "jupyter": {
    "source_hidden": true
  }
}
```

**After (.qmd):**

```python
#| echo: false
```

### Common Metadata Mappings

| Jupyter / nbconvert tag | Quarto hashpipe option |
| ----------------------- | ---------------------- |
| `remove-input` / `hide-input` | `#| echo: false` |
| `remove-output` / `hide-output` | `#| output: false` |
| `remove-cell` | `#| include: false` |
| `raises-exception` | `#| error: true` |

## YAML Front Matter

Add YAML front matter at the top of the .qmd (not needed in .ipynb, which uses notebook metadata):

```yaml
---
title: "My Analysis"
author: "Jane Doe"
date: today
format: html
execute:
  echo: true
  warning: false
jupyter: python3
---
```

For .ipynb, equivalent settings go in the notebook's metadata:

```json
{
  "kernelspec": {"name": "python3", ...},
  "quarto": {
    "title": "My Analysis",
    "execute": {"echo": true}
  }
}
```

## Specifying the Jupyter Kernel

In .qmd, set the kernel in YAML:

```yaml
jupyter: python3          # default Python 3 kernel
jupyter: ir               # R kernel (IRkernel)
jupyter: myenv            # a named venv/conda env kernel
```

Or use the `engine` key:

```yaml
engine: jupyter
jupyter: python3
```

## Controlling Re-execution

By default Quarto re-executes all cells. To use stored outputs from the notebook:

```yaml
execute:
  enabled: false  # use stored outputs, don't re-run
```

Or per-format:

```yaml
format:
  html:
    execute:
      enabled: false
```

## freeze for Collaborative Projects

In `_quarto.yml`, freeze Jupyter outputs so they are only re-run when source changes:

```yaml
execute:
  freeze: auto
```

## Workflow Recommendation

For new Python-focused Quarto documents, prefer `.qmd` over `.ipynb`:

- Better Git diffs (no binary output blobs)
- Hashpipe options are cleaner than JSON cell metadata
- Works with any editor, not just Jupyter UI

Use `.ipynb` when: the notebook is primarily interactive exploration, shared with non-Quarto users, or heavily uses Jupyter widgets.

## Resources

- [Quarto Jupyter Reference](https://quarto.org/docs/computations/jupyter.html)
- [Using Jupyter in Quarto](https://quarto.org/docs/tools/jupyter-lab.html)
- [Freeze / Caching](https://quarto.org/docs/projects/code-execution.html)

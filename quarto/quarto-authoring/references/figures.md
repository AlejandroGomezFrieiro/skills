# Figures

Quarto provides comprehensive support for figures including sizing, layout, subfigures, and accessibility features.

## Basic Figure Syntax

### Markdown Images

````markdown
![Caption text](image.png)
````

### With Attributes

````markdown
![Caption text](image.png){width=50%}
````

### Cross-Referenceable Figure

````markdown
![Caption text](image.png){#fig-example}

See @fig-example.
````

## Figure Attributes

Common attributes for images:

| Attribute   | Description  | Example                  |
| ----------- | ------------ | ------------------------ |
| `width`     | Image width  | `width=50%`, `width=4in` |
| `height`    | Image height | `height=3in`             |
| `fig-align` | Alignment    | `fig-align="center"`     |
| `fig-alt`   | Alt text     | `fig-alt="Description"`  |

### Sizing

Multiple units supported:

````markdown
![](image.png){width=50%}
![](image.png){width=400px}
![](image.png){width=4in}
![](image.png){width=10cm}
````

### Alignment

````markdown
![Left aligned](image.png){fig-align="left"}
![Centered](image.png){fig-align="center"}
![Right aligned](image.png){fig-align="right"}
````

### Alt Text for Accessibility

````markdown
![Caption](image.png){fig-alt="Detailed description for screen readers"}
````

Alt text differs from caption - it describes the image content for accessibility.

## Computational Figures

Figures generated from code use hashpipe options:

### R Examples

````markdown
```{r}
#| label: fig-scatter
#| fig-cap: "Scatter plot showing the relationship."
#| fig-alt: "Scatter plot with positive trend."
#| fig-width: 8
#| fig-height: 6
#| fig-align: center

plot(x, y)
```
````

````markdown
```{r}
#| label: fig-ggplot
#| fig-cap: "ggplot2 scatter with trend line."
#| fig-width: 8
#| fig-height: 6

library(ggplot2)
ggplot(data, aes(x, y)) +
  geom_point() +
  geom_smooth(method = "lm")
```
````

### Python Examples

Matplotlib:

````markdown
```{python}
#| label: fig-histogram
#| fig-cap: "Distribution of values."
#| fig-alt: "Histogram with 30 bins showing right-skewed distribution."
#| fig-width: 8
#| fig-height: 6

import matplotlib.pyplot as plt
plt.hist(data, bins=30)
plt.xlabel("Value")
plt.ylabel("Count")
plt.show()
```
````

Seaborn:

````markdown
```{python}
#| label: fig-scatter
#| fig-cap: "Scatter plot with regression line."
#| fig-alt: "Scatter plot showing positive correlation between x and y."
#| fig-width: 8
#| fig-height: 6

import seaborn as sns
sns.regplot(x=df["x"], y=df["y"], data=df)
```
````

Plotnine (ggplot2 for Python):

````markdown
```{python}
#| label: fig-plotnine
#| fig-cap: "ggplot-style scatter plot."
#| fig-width: 8
#| fig-height: 6

from plotnine import ggplot, aes, geom_point, geom_smooth
(ggplot(df, aes("x", "y")) + geom_point() + geom_smooth(method="lm"))
```
````

Plotly (interactive HTML figures):

````markdown
```{python}
#| label: fig-interactive
#| fig-cap: "Interactive scatter plot."

import plotly.express as px
fig = px.scatter(df, x="x", y="y", color="group")
fig.show()
```
````

### Figure Options

| Option             | Description      | Example                         |
| ------------------ | ---------------- | ------------------------------- |
| `fig-cap`          | Caption          | `"Figure caption."`             |
| `fig-subcap`       | Subcaptions      | `["A", "B"]`                    |
| `fig-alt`          | Alt text         | `"Description."`                |
| `fig-width`        | Width in inches  | `8`                             |
| `fig-height`       | Height in inches | `6`                             |
| `fig-align`        | Alignment        | `"center"`                      |
| `fig-cap-location` | Caption position | `"bottom"`, `"top"`, `"margin"` |
| `fig-format`       | Output format    | `"png"`, `"svg"`, `"pdf"`       |
| `fig-dpi`          | Resolution       | `300`                           |

## Subfigures

Group multiple images with a shared caption:

````markdown
::: {#fig-comparison layout-ncol=2}

![First image](image1.png){#fig-first}

![Second image](image2.png){#fig-second}

Comparison of two approaches.
:::

See @fig-comparison, particularly @fig-first.
````

### From Code

R:

````markdown
```{r}
#| label: fig-panels
#| fig-cap: "Panel figure."
#| fig-subcap:
#|   - "Distribution of X"
#|   - "Distribution of Y"
#| layout-ncol: 2

hist(x)
hist(y)
```
````

Python (each `plt.show()` call produces one subfigure):

````markdown
```{python}
#| label: fig-panels
#| fig-cap: "Panel figure."
#| fig-subcap:
#|   - "Distribution of X"
#|   - "Distribution of Y"
#| layout-ncol: 2

import matplotlib.pyplot as plt
plt.hist(x)
plt.show()
plt.hist(y)
plt.show()
```
````

## Figure Layouts

For arranging multiple figures, use layout divs. See [layout.md](layout.md) for full layout options.

### Basic Layout

````markdown
::: {layout-ncol=2}
![](image1.png)

![](image2.png)
:::
````

### Layout Attributes

| Attribute        | Description           | Example             |
| ---------------- | --------------------- | ------------------- |
| `layout-ncol`    | Number of columns     | `layout-ncol=2`     |
| `layout-nrow`    | Number of rows        | `layout-nrow=2`     |
| `layout`         | Custom layout array   | `layout="[[1,1]]"`  |
| `layout-valign`  | Vertical alignment    | `layout-valign=top` |

## Figure Panels

For images without individual captions:

````markdown
::: {#fig-panel layout-ncol=2}
![](plot1.png)

![](plot2.png)

Multiple plots in a panel.
:::
````

## Caption Location

### Document Level

```yaml
fig-cap-location: top
```

### Per Figure

R:

````markdown
```{r}
#| label: fig-example
#| fig-cap: "Caption on top."
#| fig-cap-location: top

plot(1:10)
```
````

Python:

````markdown
```{python}
#| label: fig-example
#| fig-cap: "Caption on top."
#| fig-cap-location: top

import matplotlib.pyplot as plt
plt.plot(range(10))
plt.show()
```
````

Options: `top`, `bottom`, `margin`.

## Lightbox

Enable click-to-zoom for images (HTML only):

````markdown
![](image.png){.lightbox}
````

Or for a group:

````markdown
::: {.lightbox}
![](image1.png)

![](image2.png)
:::
````

### Lightbox Options

````markdown
![](image.png){.lightbox group="gallery" description="Detailed view"}
````

### Enable Globally

```yaml
lightbox: true
```

Or with options:

```yaml
lightbox:
  match: auto
  effect: zoom
  loop: true
```

## Linked Images

Make images clickable:

````markdown
[![Caption](thumbnail.png)](fullsize.png)
````

Or link to URL:

````markdown
[![Logo](logo.png)](https://example.com)
````

## Figure Divs

For complex figure content:

````markdown
::: {#fig-custom}

<iframe src="interactive.html"></iframe>

Custom interactive figure.
:::
````

## Document Defaults

Set figure defaults in YAML:

```yaml
format:
  html:
    fig-width: 8
    fig-height: 6
    fig-format: svg
    fig-dpi: 300
    fig-align: center
```

## Resources

- [Quarto Figures](https://quarto.org/docs/authoring/figures.html)
- [Figure Layout](https://quarto.org/docs/authoring/figures.html#figure-panels)
- [Lightbox](https://quarto.org/docs/output-formats/html-lightbox-figures.html)


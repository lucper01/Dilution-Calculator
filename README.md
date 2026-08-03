# Concentration and Dilution Calculator

A responsive, bilingual, single-file concentration and serial dilution calculator designed for publication with GitHub Pages.

The complete website—including its visual design, calculation engine, translations, accessibility controls, help dialogs, export logic, and inline SVG icons—is contained in **one `index.html` file**. No JavaScript library, CSS framework, image directory, build process, web server, database, or third-party API is required.

## Key features

### Concentration input formats

The calculator accepts four concentration notations:

1. **Percentage v/v**
   - Example: `5` means `5% v/v`.

2. **Coefficient × 10^exponent**
   - Example: coefficient `5`, exponent `-2` means `5 × 10⁻²`.
   - The coefficient is optional.
   - A blank coefficient with exponent `-1` means `10⁻¹`.
   - Accepted exponent forms include `-1`, `10^-1`, `10-1`, `10**-1`, and `10⁻¹`.

3. **Power of ten only**
   - Example: `x = 2` in `10^-x` produces `10⁻²`.

4. **Fraction**
   - Example: `1 / 20` produces `5% v/v`.

Changing the concentration format resets only the concentration fields to the equivalent of `10⁻¹`. Product type, diluent, volume, unit, and display precision are preserved.

### Detailed serial dilution protocol

The calculator automatically determines whether the requested concentration should be prepared directly or through a serial dilution.

For every tube, including on small mobile screens, it displays:

- the source solution;
- the volume of diluent to add;
- the source volume to measure;
- the resulting total volume;
- the dilution factor for the step;
- the resulting concentration;
- labeling instructions;
- transfer and mixing instructions;
- whether the tube becomes the source for the next step;
- explicit identification of the final tube.

The protocol is never replaced by a shortened mobile summary.

### Product and diluent guidance

The interface provides an indicative compatibility message for common combinations of:

- aqueous or water-soluble products;
- oil-based or lipophilic products;
- alcohol-based products;
- complex flavors or mixtures;
- propylene glycol;
- mineral oil;
- distilled water.

These messages are general guidance only. Experimental checks of miscibility, stability, safety, and device compatibility remain necessary.

### Bilingual interface

The customization panel includes a French/English switch.

The selected language applies to:

- all interface controls;
- field labels and help text;
- accessibility settings;
- calculation errors;
- compatibility messages;
- summary cards;
- every dilution instruction;
- copied protocols;
- shared protocols;
- CSV headers and descriptive values.

The language setting is stored locally in the browser and restored on the next visit.

### Theme controls

The interface includes explicit **Light** and **Dark** themes.

The selected appearance is stored locally in the browser. Dark mode applies to the forms, result cards, mobile toolbar, help dialogs, and customization panel—not only to the page background.

### Accessibility controls

The personalization panel reproduces the relevant accessibility controls from the academic portfolio:

- **Larger text**
  - increases base text size;
  - increases line spacing;
  - enlarges controls.

- **High contrast**
  - reinforces borders and focus indicators;
  - removes decorative shadows;
  - supports both light and dark themes;
  - prevents pale headings from disappearing.

- **Dyslexia-friendly reading**
  - replaces serif titles with a simpler sans-serif font;
  - increases letter and word spacing;
  - increases line height;
  - disables unnecessary hover movement.

- **Reduce motion**
  - minimizes refresh animations, transitions, and movement effects.

The panel also contains **Select all** and **Clear all** actions. Accessibility preferences are stored in the browser.

### Local and private operation

All calculations are performed in the browser. The page:

- does not send entered concentrations or results to a server;
- does not use analytics;
- does not load external scripts;
- does not use cookies;
- does not require an account;
- does not require an internet connection after the HTML file has been opened locally, except when the page itself is hosted on GitHub Pages and must first be loaded.

Browser storage is used only to remember language, theme, and accessibility preferences.

## Repository contents

Only two files are recommended:

```text
index.html
README.md
```

`README.md` documents the project but is not required by the website. The published calculator depends only on `index.html`.

No `icons` directory, `styles.css`, `app.js`, manifest, service worker, or asset directory is required.

## Publishing with GitHub Pages

### 1. Upload the files

Place `index.html` and this `README.md` at the root of the repository.

The repository should look like:

```text
Calculateur-Dilution-Android/
├── index.html
└── README.md
```

### 2. Enable GitHub Pages

In the repository:

1. Open **Settings**.
2. Open **Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**.
4. Select the `main` branch.
5. Select `/(root)`.
6. Save.

The expected URL is:

```text
https://lucper01.github.io/Calculateur-Dilution-Android/
```

GitHub may need a few minutes to publish the first version.

### 3. Updating the site

Replace the existing `index.html` with the new version and commit the change. GitHub Pages will redeploy the website automatically.

Because all website logic is inside this file, there are no asset paths or versioned dependencies to update.

## Using the calculator locally

The file can also be used without GitHub Pages:

1. Download `index.html`.
2. Open it with Chrome, Edge, Firefox, or Safari.
3. Perform the calculation normally.

CSV export, copy, and most share functions depend on browser capabilities and permissions. The Web Share API is mainly available on mobile browsers. When native sharing is unavailable, the calculator copies the protocol instead.

## Mobile behavior

On screens up to 860 pixels wide:

- the parameter and results columns become a single vertical flow;
- the parameter panel is no longer sticky;
- a persistent four-action toolbar provides Reset, Copy, Share, and Export;
- the personalization panel becomes a bottom sheet above the mobile toolbar;
- every dilution card remains expanded;
- volumes and instructions remain visible;
- two-column groups collapse when required;
- safe-area insets are respected on devices with display cutouts or gesture navigation.

On very narrow screens, metric and volume cards progressively switch to a single column.

## Desktop behavior

On desktop:

- parameters remain in a sticky, independently scrollable left column;
- results use the wider right column;
- large screens receive increased spacing;
- the full protocol remains visible while parameters are adjusted;
- recalculation is debounced to avoid abrupt changes during typing.

## Calculation logic

The target concentration is represented as a ratio relative to the stock solution:

```text
target ratio = target concentration / stock concentration
```

A percentage is converted using:

```text
ratio = percentage / 100
```

Scientific notation is converted using:

```text
ratio = coefficient × 10^exponent
```

A fraction is converted using:

```text
ratio = numerator / denominator
```

The serial dilution planner uses successive tenfold steps where appropriate, followed by a residual step when the target is not an exact power of ten.

For a target of `5 × 10⁻²` and a final volume of `10 mL`, the protocol is:

```text
Tube 1: 1 mL stock solution + 9 mL diluent → 10⁻¹
Tube 2: 5 mL from Tube 1 + 5 mL diluent → 5 × 10⁻²
```

The displayed precision affects presentation and CSV output only. Internal calculations use JavaScript floating-point precision.

## CSV export

The CSV export contains:

- target concentration;
- percentage v/v;
- normalized scientific coefficient;
- scientific exponent;
- overall dilution factor;
- selected volume and unit;
- product type;
- diluent;
- indicative compatibility;
- one row per dilution step;
- source and diluent volumes;
- cumulative concentration and exponent.

The file includes a UTF-8 byte-order mark for improved compatibility with spreadsheet software using French or English regional settings.

## Browser compatibility

Recommended current browsers:

- Google Chrome;
- Microsoft Edge;
- Mozilla Firefox;
- Safari;
- Chromium-based Android browsers.

The calculator uses standard HTML, CSS, and JavaScript APIs. No browser extension is required.

The native Share button depends on the Web Share API. If it is unavailable, the protocol is copied to the clipboard.

## Accessibility and keyboard navigation

The page includes:

- a skip link to the main content;
- visible focus indicators;
- semantic headings;
- explicit button states;
- accessible labels for icon buttons;
- keyboard-closing behavior for dialogs;
- Escape-key support;
- live regions for calculation refresh and notifications;
- reduced-motion support through both the operating-system preference and the website control.

## Important laboratory notice

The calculator is a preparation aid. Before using a generated protocol:

- verify all concentrations independently;
- confirm the stock concentration;
- use suitable calibrated volumetric equipment;
- account for dead volume and transfer losses;
- label every tube before transfer;
- verify product/diluent miscibility;
- assess chemical and biological safety;
- confirm compatibility with the experimental delivery system;
- follow the laboratory’s approved procedures.

## Customization and maintenance

The main editable sections inside `index.html` are:

- CSS variables near the beginning of the `<style>` block;
- the `I18N` translation object in the final script;
- compatibility guidance in `compatibilityCopy`;
- the calculation engine in the first script;
- the interface structure in the `<body>`.

When editing translations, keep the same translation keys in both `fr` and `en`.

When changing the product or diluent options, keep stable `value` attributes or update the compatibility mappings accordingly.

## Troubleshooting

### The page is blank

Confirm that the file is named exactly:

```text
index.html
```

It must not be named `index.html.txt`.

### GitHub Pages shows the README instead of the calculator

Check that `index.html` is at the root of the selected publishing branch and that Pages is configured for `/(root)`.

### The old version is still displayed

Reload the page without cache, use a private browsing tab, or wait for the GitHub Pages deployment to finish.

### Sharing does not open a mobile share sheet

The browser may not support the Web Share API. The calculator will fall back to copying the protocol.

### Clipboard copy is blocked

Clipboard access may require HTTPS or a direct user action. GitHub Pages uses HTTPS and is therefore recommended.

### Decimal commas are rejected

The calculator accepts both commas and periods in numeric fields. The interface converts commas internally before calculation.

### Dark mode or accessibility settings persist

Use the customization panel and select the preferred theme or **Clear all** accessibility options. Preferences are intentionally stored in the browser.

## License and reuse

No license has been added automatically. Add a `LICENSE` file if the project should be reusable by others.

If the repository is public, anyone can view and copy the source code. The calculator itself does not contain private experimental data.

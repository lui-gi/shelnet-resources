# Design Philosophy & Guidelines
Reference this markdown file when making contributions to Shelnet. Contributors are required to read this file -> PRs should include 'read05' to ensure that this file has been read.

## 1. Core Aesthetic: "Cyber-Brutalism"
Shelnet defines itself through a **Brutalist** aesthetic. The look is defined by high contrast, raw structural elements, and a "terminal-first" mentality. While we utilize modern UI techniques like glassmorphism, the soul of the site is utilitarian and stark.

### Key Visual Pillars
* **True Black Backgrounds:** We use `#000000` as our canvas. Avoid dark grays for the main body background; the distinct lack of light is a feature, not a bug.
* **Visible Structure:** Borders are often visible (`border-white/10`), acting as the skeleton of the layout.
* **Subtle Textures:** We use very low opacity grid backgrounds (`opacity-[0.03]`) to add depth without clutter.

---

## 2. Color System
The site relies on a strict, semantic color system to differentiate content streams (Certifications).

### Primary Themes
* **Green (`text-green-400` / `bg-green-500`):** Reserved for **CompTIA A+** content.
* **Blue (`text-blue-400` / `bg-blue-500`):** Reserved for **CompTIA Security+** content.

### Extending the Palette
Contributors may introduce new colors (e.g., Purple, Orange, Red) **only** under the following conditions:
1.  **New Certifications:** If adding a new exam path (e.g., Pentest+), a new distinct theme color should be established in `src/config/themeColors.js`.
2.  **Visualizations:** Complex diagrams or PBQs may use other colors for clarity.
3.  **Restricted Areas:** The **Home Page** must strictly adhere to the Blue/Green/Monochrome palette. Do not introduce random accent colors to the landing experience.

---

## 3. Typography
Our typography blends system reliability with brutalist styling.

### Font Family
* **Flexibility:** You may use standard **Helvetica/Arial** stacks or the default **Tailwind Sans** stack. Both are acceptable.
* **Consistency:** Do not mix font families arbitrarily within a single component.

### Header Styling
Major headers often employ a specific "Brutal" style:
* **Uppercase**
* **Tight Tracking** (`tracking-tight`)
* **High Contrast** (White text on Black)
* Example:
    ```jsx
    <h2 className="text-4xl font-bold uppercase tracking-tight">
      Title Here
    </h2>
    ```

---

## 4. Motion & Interactivity
Motion is a tool for **emphasis**, not decoration.

* **Purpose:** Use animation to draw attention to critical concepts or high-value elements (e.g., the Terminal intro).
* **Restraint:** Static content is acceptable and often preferred for documentation. Do not animate every button or card.
* **Technique:** When animating, prefer CSS transitions or lightweight React state animations (like the typing effect in `TerminalComponent`).

---

## 5. Iconography
We do not enforce a single icon library (e.g., Lucide, Heroicons, FontAwesome), but we enforce **documentation**.

* **No Preference:** Use the library that best fits the specific diagram or UI need.
* **Tracking:** If you introduce a new icon library, you **must** document it in the project `README` or a specific `ICONS.md` file.
* **Style Match:** Regardless of the library, ensure icons are stroked/colored to match the current theme (e.g., if inside a Blue theme component, the icon should be `text-blue-400` or `border-blue-500/50`).

---

## 6. UI Patterns to Maintain
When building new components, reference `src/config/themeColors.js` to ensure consistency.

* **Glassmorphism Containers:**
    * Background: `bg-white/[0.05]` or colored variants like `bg-green-500/10`.
    * Borders: `border-white/10`.
* **Muted Text:** Use opacity for secondary text (e.g., `text-white/40` or `text-green-400/80`) rather than gray hex codes.
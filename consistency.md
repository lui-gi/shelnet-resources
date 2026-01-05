# Design Philosophy & Guidelines
Reference this markdown file when making contributions to Shelnet. Contributors are required to read this file -> PRs should include 'read05' to ensure that this file has been read.

## 1. Core Aesthetic: "Brutalism"

Shelnet defines itself through a Cyber-Brutalism aesthetic. Our look is defined by high contrast, raw structural elements, and a "terminal-first" mentality. While we utilize modern UI techniques like glassmorphism, the soul of the site is utilitarian and stark.

Key Visual Pillars

True Black Backgrounds: We use #000000 as our canvas [cite: src/index.css]. Avoid dark grays for the main body background; the distinct lack of light is a feature, not a bug.

Visible Structure: Borders are often visible (White #ffffff at 10% opacity), acting as the skeleton of the layout.

Subtle Textures: We use very low opacity grid backgrounds (White #ffffff at 3% opacity) to add depth without clutter [cite: src/components/shared/GridBackground.jsx].

Implementation Example

A typical container blending brutalism (borders, black bg) with glassmorphism:

```
// Found in ExamsSection.jsx
<div className="border border-white/10 bg-white/[0.02] p-8">
  <div className="text-white/40 mb-2 font-mono">
    root@shelnet:~# exam-runner --type a-plus
  </div>
</div>
```

## 2. Color System

The site relies on a strict, semantic color system to differentiate content streams (Certifications). We utilize the standard Tailwind CSS palette.

Primary Themes [cite: src/config/themeColors.js]

Green (CompTIA A+):

Base: #22c55e (Tailwind green-500) - Used for borders/backgrounds.

Highlight: #4ade80 (Tailwind green-400) - Used for text/glows.

Blue (CompTIA Security+):

Base: #3b82f6 (Tailwind blue-500) - Used for borders/backgrounds.

Highlight: #60a5fa (Tailwind blue-400) - Used for text/glows.

Extending the Palette

Contributors may introduce new colors (e.g., Purple, Orange, Red) only under the following conditions:

New Certifications: If adding a new exam path (e.g., Pentest+).

Visualizations: Complex diagrams or PBQs may use other colors for clarity.

Restricted Areas: The Home Page must strictly adhere to the Blue/Green/Monochrome palette.

Implementation Example

From src/config/themeColors.js, showing how we separate themes:
```
export const themeColors = {
  green: {
    bgActive: 'bg-green-500/10', // #22c55e @ 10%
    borderActive: 'border-green-500', // #22c55e
    text: 'text-green-400', // #4ade80
  },
  blue: {
    bgActive: 'bg-blue-500/10', // #3b82f6 @ 10%
    borderActive: 'border-blue-500', // #3b82f6
    text: 'text-blue-400', // #60a5fa
  }
};
```

## 3. Typography

Our typography blends system reliability with brutalist styling.

Font Family

Flexibility: You may use standard Helvetica/Arial stacks or the default Tailwind Sans stack.

Code/Terminal: Use font-mono heavily for "system" text, file paths, or decorative terminal elements [cite: src/components/animations/TerminalComponent.jsx].

Header Styling [cite: src/components/shared/BrutalHeader.jsx]

Major headers often employ a specific "Brutal" style:

Uppercase

Tight Tracking (tracking-tight)

High Contrast (White #ffffff on Black #000000)

Implementation Example

The BrutalHeader component:
```
// src/components/shared/BrutalHeader.jsx
<h2 className="text-4xl md:text-6xl font-bold text-white mb-2 tracking-tight uppercase" 
    style={{ fontFamily: 'Helvetica Neue, sans-serif' }}>
  {title}
</h2>
<div className="text-xs text-white/40 uppercase tracking-widest font-mono">
  {subtitle}
</div>
```

## 4. Motion & Interactivity

Motion is a tool for emphasis, not decoration.

Purpose: Use animation to draw attention to critical concepts or high-value elements (e.g., the Terminal intro).

Restraint: Static content is acceptable and often preferred for documentation.

Technique: Prefer CSS transitions or lightweight React state animations.

Implementation Example

The typing effect in TerminalComponent mimics a real shell [cite: src/components/animations/TerminalComponent.jsx]:
```
// Simulating typing delay
await delay(50 + Math.random() * 40);
setTypedCmd(current.cmd.slice(0, i));
```

## 5. Iconography

We enforce documentation and consistency in stroke weight.

Library: Lucide React is the standard library used throughout the site [cite: package.json].

Style: Ensure icons are stroked/colored to match the current theme (e.g., if inside a Blue theme component, the icon should be #60a5fa).

Implementation Example

Using Lucide with dynamic theme colors in PageHeader.jsx [cite: src/components/shared/PageHeader.jsx]:
```
import { ChevronLeft } from 'lucide-react';

<Link to="/" className="text-white/40 hover:text-green-400">
  <ChevronLeft size={16} />
  ../BACK_TO_HOME
</Link>
```

## 6. Common Components & Design Elements

Reference this list when building new UI to maintain consistency.

Libraries

Icons: lucide-react [cite: package.json]

Styling: tailwindcss (v4)

Routing: react-router-dom

Standard Color Palette (Hex Codes)

Element

Green Theme (A+)

Blue Theme (Security+)

Primary Text

#4ade80 (green-400)

#60a5fa (blue-400)

Muted Text

#4ade80 (80% opacity)

#60a5fa (80% opacity)

Border Active

#22c55e (green-500)

#3b82f6 (blue-500)

Background Active

#22c55e (10% opacity)

#3b82f6 (10% opacity)

Icon Background

#22c55e (20% opacity)

#3b82f6 (20% opacity)

Utility Patterns (Hex Reference)

Glass Container: #ffffff at 5% opacity (hover) or 2% opacity (static).

Subtle Border: #ffffff at 10% opacity (Standard for almost all cards).

Grid Background: #ffffff at 3% opacity (<GridBackground /> component).

Muted Text: #ffffff at 40-50% opacity.

Terminal Text: font-mono text-sm usually paired with #22c55e or #3b82f6.
# SHELNET DESIGN SYSTEM BLUEPRINT

**Philosophy:** Brutalist / Functional
**Core Principle:** High contrast, raw structure, monospaced details, and neon accents on a void-black background.

---

## 1. BASE CONFIGURATION (Tailwind CSS)

To recreate the look in a standalone HTML file, inject this configuration into the `<script>` tag of your Tailwind CDN.

### Color Palette & Fonts
* **Background:** `#000000` (Pure Black)
* **Surface:** `rgba(255, 255, 255, 0.05)` (Glass/Ghost)
* **Border:** `rgba(255, 255, 255, 0.1)` (Structural)
* **Accents:**
    * **Green (A+):** `#4ade80` (Text/Glow), `#22c55e` (Borders)
    * **Blue (Sec+):** `#60a5fa` (Text/Glow), `#3b82f6` (Borders)

### Tailwind Config Script
```html
<script src="[https://cdn.tailwindcss.com](https://cdn.tailwindcss.com)"></script>
<script>
    tailwind.config = {
        theme: {
            extend: {
                colors: {
                    // Theme: Green
                    green: { 400: '#4ade80', 500: '#22c55e' },
                    // Theme: Blue 
                    blue: { 400: '#60a5fa', 500: '#3b82f6' },
                    // Theme: Red
                    red: { 400: '#f87171', 500: '#ef4444' }
                },
                fontFamily: {
                    // Headings: Brutalist, Tight, Impactful
                    sans: ['Helvetica Neue', 'Helvetica', 'Arial', 'sans-serif'],
                    // Data/Subtitles: Technical, Precise
                    mono: ['Courier New', 'Courier', 'monospace'],
                },
                animation: {
                    'blink': 'blink 1s step-end infinite',
                },
                keyframes: {
                    blink: {
                        '0%, 100%': { opacity: '1' },
                        '50%': { opacity: '0' },
                    }
                }
            }
        }
    }
</script>

<header class="mb-12 border-b border-white/10 pb-6">
    <h1 class="text-4xl md:text-6xl font-bold text-white mb-2 tracking-tight uppercase font-sans">
        PAGE TITLE
    </h1>
    <div class="text-xs text-white/40 uppercase tracking-widest font-mono">
        SUBTITLE_OR_DIRECTORY_PATH
    </div>
</header>
<div class="relative group">
    <div class="absolute -inset-0.5 bg-gradient-to-r from-green-500 to-green-900 rounded-lg blur opacity-20 group-hover:opacity-40 transition duration-1000"></div>
    
    <div class="relative bg-black border border-white/10 rounded-lg p-6 font-mono text-white">
        <div class="flex items-center gap-2 mb-4 border-b border-white/5 pb-2">
            <div class="w-3 h-3 rounded-full bg-red-500/20 border border-red-500/50"></div>
            <div class="w-3 h-3 rounded-full bg-yellow-500/20 border border-yellow-500/50"></div>
            <div class="w-3 h-3 rounded-full bg-green-500/20 border border-green-500/50"></div>
            <span class="ml-auto text-xs text-white/20">terminal</span>
        </div>
        
        <p class="text-white/80">System ready...</p>
    </div>
</div>
<button class="px-4 py-2 font-mono text-sm border rounded transition-all duration-200 
               bg-white/[0.05] border-white/10 text-white/80 
               hover:border-green-500 hover:text-green-400 hover:shadow-[0_0_10px_rgba(34,197,94,0.2)]">
    [ INITIATE ]
</button>
<span class="px-2 py-1 text-[10px] font-mono uppercase tracking-wider rounded border
             bg-green-500/10 border-green-500/50 text-green-400">
    ONLINE
</span>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shelnet Module</title>
    
    <script src="[https://cdn.tailwindcss.com](https://cdn.tailwindcss.com)"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        green: { 400: '#4ade80', 500: '#22c55e' },
                        blue: { 400: '#60a5fa', 500: '#3b82f6' }
                    },
                    fontFamily: {
                        sans: ['Helvetica Neue', 'Helvetica', 'Arial', 'sans-serif'],
                        mono: ['Courier New', 'Courier', 'monospace'],
                    }
                }
            }
        }
    </script>
    
    <style>
        /* GLOBAL OVERRIDES */
        body { background-color: #000; color: #fff; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .cursor-blink { animation: blink 1s step-end infinite; }
        @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }
    </style>
</head>
<body class="bg-black text-white min-h-screen font-sans selection:bg-green-500/30 flex flex-col p-6 md:p-12 max-w-7xl mx-auto">

    <header class="mb-12 border-b border-white/10 pb-6">
        <h1 class="text-4xl md:text-6xl font-bold text-white mb-2 tracking-tight uppercase">
            Module Name
        </h1>
        <div class="text-xs text-white/40 uppercase tracking-widest font-mono">
            ./src/modules/new-file.html
        </div>
    </header>

    <main class="grid grid-cols-1 md:grid-cols-2 gap-8">
        
        <div class="space-y-6">
            <p class="text-xl text-white/60 leading-relaxed">
                Primary descriptive content goes here. Keep it minimal and clean.
            </p>
            
            <button class="px-4 py-3 w-full md:w-auto font-mono text-sm border rounded transition-all
                           bg-white/[0.05] border-white/10 text-white
                           hover:border-green-500 hover:text-green-400">
                ACTION_BUTTON
            </button>
        </div>

        <div class="bg-black border border-white/10 rounded-lg p-6 font-mono text-sm h-64 overflow-y-auto">
            <div class="text-green-400 mb-2">root@shelnet:~# <span class="text-white">./execute</span></div>
            <div class="text-white/50">Running diagnostic...</div>
            <div class="text-white/50">Loading assets... [OK]</div>
            <div class="text-green-400 mt-2">Done.<span class="inline-block w-2 h-4 bg-green-400 ml-1 cursor-blink"></span></div>
        </div>

    </main>

</body>
</html>
```

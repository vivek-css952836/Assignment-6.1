This web application includes:

Interactive Cards Workspace: Dynamic sliders to adjust width, height, padding, border, margin, and box-sizing mode (content-box vs border-box) for three card components in real-time.

DevTools-style Visual Box Model Diagram: A live computed Box Model inspector highlighting Content (Blue), Padding (Green), Border (Yellow/Amber), and Margin (Orange) with live pixel dimensions and width/height mathematical formulas.

Annotated Reference & Screenshot/Export Tool: Clear visual explanation of all 4 box model layers, devtools inspection instructions, and a quick export tool for complete HTML/CSS code and printable assignment submission views.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Assignment 6.1 — Box Model Inspection</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        boxMargin: '#f97316',   /* Orange */
                        boxBorder: '#eab308',   /* Yellow */
                        boxPadding: '#22c55e',  /* Green */
                        boxContent: '#3b82f6',  /* Blue */
                    }
                }
            }
        }
    </script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        /* Custom styles for box model highlights */
        .highlight-margin { background-color: rgba(249, 115, 22, 0.25); border: 2px dashed #f97316; }
        .highlight-border { background-color: rgba(234, 179, 8, 0.3); border: 2px solid #eab308; }
        .highlight-padding { background-color: rgba(34, 197, 94, 0.25); border: 2px dashed #22c55e; }
        .highlight-content { background-color: rgba(59, 130, 246, 0.3); border: 1px solid #3b82f6; }

        /* Smooth slider transitions */
        input[type=range] {
            accent-color: #6366f1;
        }

        /* Custom print formatting for assignment submission */
        @media print {
            .no-print { display: none !important; }
            body { background: white !important; color: black !important; }
            .print-full { width: 100% !important; max-width: 100% !important; }
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen font-sans antialiased pb-12">

    <!-- Top Header -->
    <header class="border-b border-slate-800 bg-slate-950/80 backdrop-blur sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-4 sm:px-6 lg:px-8 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
            <div>
                <span class="text-xs font-semibold uppercase tracking-wider text-indigo-400 bg-indigo-950/80 px-2.5 py-1 rounded-full border border-indigo-800/50">CSS Layout Interactive Tool</span>
                <h1 class="text-2xl font-bold text-white mt-1">Assignment 6.1 — Box Model Inspection</h1>
            </div>
            <div class="flex items-center gap-2">
                <button onclick="scrollToSection('annotation-section')" class="px-3 py-1.5 text-xs font-medium bg-slate-800 hover:bg-slate-700 text-slate-200 rounded-lg transition flex items-center gap-1.5">
                    <i data-lucide="book-open" class="w-4 h-4 text-emerald-400"></i> Box Model Theory
                </button>
                <button onclick="exportCodeModal()" class="px-3 py-1.5 text-xs font-medium bg-indigo-600 hover:bg-indigo-500 text-white rounded-lg transition flex items-center gap-1.5 shadow-lg shadow-indigo-600/30">
                    <i data-lucide="code" class="w-4 h-4"></i> Get Submission Code
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 space-y-10">

        <!-- Instructions Banner -->
        <div class="bg-slate-800/60 border border-slate-700/60 rounded-xl p-5 shadow-lg relative overflow-hidden">
            <div class="absolute -right-6 -bottom-6 w-32 h-32 bg-indigo-500/10 rounded-full blur-2xl"></div>
            <h2 class="text-lg font-semibold text-white flex items-center gap-2">
                <i data-lucide="info" class="w-5 h-5 text-indigo-400"></i> Objectives & Inspection Instructions
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mt-3 text-sm text-slate-300">
                <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-800">
                    <span class="font-semibold text-indigo-300 block mb-1">1. Modify Box Dimensions</span>
                    Use controls on each card to change width, height, padding, border, and margins in real-time.
                </div>
                <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-800">
                    <span class="font-semibold text-amber-300 block mb-1">2. Compare Sizing Modes</span>
                    Observe key differences between <code class="bg-slate-800 px-1 py-0.5 rounded text-amber-400">content-box</code> and <code class="bg-slate-800 px-1 py-0.5 rounded text-emerald-400">border-box</code> sizing behaviors.
                </div>
                <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-800">
                    <span class="font-semibold text-emerald-300 block mb-1">3. Live DevTools Inspection</span>
                    Select any card to view its live Computed Box Model diagram (Content, Padding, Border, Margin) & exact total calculations.
                </div>
            </div>
        </div>

        <!-- MAIN WORKSPACE: CARDS & LIVE INSPECTOR -->
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-start">
            
            <!-- LEFT COLUMN: 3 CARDS PREVIEW & CONTROLS (7 cols) -->
            <div class="lg:col-span-7 space-y-8">
                <div class="flex justify-between items-center">
                    <h2 class="text-xl font-bold text-white flex items-center gap-2">
                        <i data-lucide="layout-grid" class="w-5 h-5 text-indigo-400"></i> Card Components
                    </h2>
                    <span class="text-xs text-slate-400">Click a card header to inspect in DevTools diagram</span>
                </div>

                <!-- CARD 1: CONTENT-BOX -->
                <div id="card-wrapper-1" onclick="selectCard(1)" class="card-container bg-slate-800/80 border-2 transition-all rounded-xl p-4 cursor-pointer hover:border-indigo-500/50 border-indigo-500 shadow-lg shadow-indigo-950/20">
                    <div class="flex justify-between items-center mb-3">
                        <div class="flex items-center gap-2">
                            <span class="w-3 h-3 rounded-full bg-indigo-500"></span>
                            <h3 class="font-bold text-white">Card 1: Standard Content Box</h3>
                        </div>
                        <span class="text-xs font-mono bg-indigo-900/50 text-indigo-300 border border-indigo-700/50 px-2 py-0.5 rounded">box-sizing: content-box</span>
                    </div>

                    <!-- Visual Card Element Output -->
                    <div class="overflow-x-auto py-2 bg-slate-950/50 rounded-lg p-3 border border-slate-800">
                        <div id="card-1-element" class="transition-all rounded bg-slate-800 text-slate-100 shadow-md">
                            <h4 class="text-lg font-bold text-indigo-300 mb-1" id="card-1-heading">Card 1 Heading</h4>
                            <p class="text-xs text-slate-300 leading-relaxed" id="card-1-p">This card demonstrates <code class="text-amber-300">content-box</code> where padding and borders add onto the specified width.</p>
                        </div>
                    </div>

                    <!-- Controls Accordion -->
                    <div class="mt-4 pt-3 border-t border-slate-700/60 grid grid-cols-2 sm:grid-cols-4 gap-3 text-xs">
                        <div>
                            <label class="text-slate-400 block mb-1">Width (<span id="c1-w-val">220</span>px)</label>
                            <input type="range" min="150" max="320" value="220" oninput="updateCard(1, 'w', this.value)" class="w-full">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">Padding (<span id="c1-p-val">16</span>px)</label>
                            <input type="range" min="0" max="40" value="16" oninput="updateCard(1, 'p', this.value)" class="w-full">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">Border (<span id="c1-b-val">3</span>px)</label>
                            <input type="range" min="0" max="15" value="3" oninput="updateCard(1, 'b', this.value)" class="w-full">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">Margin (<span id="c1-m-val">12</span>px)</label>
                            <input type="range" min="0" max="30" value="12" oninput="updateCard(1, 'm', this.value)" class="w-full">
                        </div>
                    </div>
                </div>

                <!-- CARD 2: BORDER-BOX -->
                <div id="card-wrapper-2" onclick="selectCard(2)" class="card-container bg-slate-800/80 border-2 border-slate-700/80 transition-all rounded-xl p-4 cursor-pointer hover:border-indigo-500/50 shadow-lg">
                    <div class="flex justify-between items-center mb-3">
                        <div class="flex items-center gap-2">
                            <span class="w-3 h-3 rounded-full bg-emerald-500"></span>
                            <h3 class="font-bold text-white">Card 2: Custom Border Box</h3>
                        </div>
                        <span class="text-xs font-mono bg-emerald-900/50 text-emerald-300 border border-emerald-700/50 px-2 py-0.5 rounded">box-sizing: border-box</span>
                    </div>

                    <!-- Visual Card Element Output -->
                    <div class="overflow-x-auto py-2 bg-slate-950/50 rounded-lg p-3 border border-slate-800">
                        <div id="card-2-element" class="transition-all rounded bg-slate-800 text-slate-100 shadow-md">
                            <h4 class="text-lg font-bold text-emerald-300 mb-1" id="card-2-heading">Card 2 Heading</h4>
                            <p class="text-xs text-slate-300 leading-relaxed" id="card-2-p">This card uses <code class="text-emerald-300">border-box</code>. Width includes padding and borders, preventing layout overflow.</p>
                        </div>
                    </div>

                    <!-- Controls Accordion -->
                    <div class="mt-4 pt-3 border-t border-slate-700/60 grid grid-cols-2 sm:grid-cols-4 gap-3 text-xs">
                        <div>
                            <label class="text-slate-400 block mb-1">Width (<span id="c2-w-val">260</span>px)</label>
                            <input type="range" min="150" max="320" value="260" oninput="updateCard(2, 'w', this.value)" class="w-full">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">Padding (<span id="c2-p-val">20</span>px)</label>
                            <input type="range" min="0" max="40" value="20" oninput="updateCard(2, 'p', this.value)" class="w-full">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">Border (<span id="c2-b-val">4</span>px)</label>
                            <input type="range" min="0" max="15" value="4" oninput="updateCard(2, 'b', this.value)" class="w-full">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">Margin (<span id="c2-m-val">16</span>px)</label>
                            <input type="range" min="0" max="30" value="16" oninput="updateCard(2, 'm', this.value)" class="w-full">
                        </div>
                    </div>
                </div>

                <!-- CARD 3: EXPANDED MARGIN/PADDING (TOGGLEABLE) -->
                <div id="card-wrapper-3" onclick="selectCard(3)" class="card-container bg-slate-800/80 border-2 border-slate-700/80 transition-all rounded-xl p-4 cursor-pointer hover:border-indigo-500/50 shadow-lg">
                    <div class="flex justify-between items-center mb-3">
                        <div class="flex items-center gap-2">
                            <span class="w-3 h-3 rounded-full bg-amber-500"></span>
                            <h3 class="font-bold text-white">Card 3: Dynamic Sizing Lab</h3>
                        </div>
                        <div class="flex items-center gap-2">
                            <label class="text-xs text-slate-400">Sizing:</label>
                            <select id="card-3-box-sizing-select" onchange="toggleCard3Sizing(this.value)" onclick="event.stopPropagation()" class="text-xs font-mono bg-slate-900 border border-slate-700 text-amber-300 rounded px-2 py-1 focus:outline-none">
                                <option value="content-box">content-box</option>
                                <option value="border-box" selected>border-box</option>
                            </select>
                        </div>
                    </div>

                    <!-- Visual Card Element Output -->
                    <div class="overflow-x-auto py-2 bg-slate-950/50 rounded-lg p-3 border border-slate-800">
                        <div id="card-3-element" class="transition-all rounded bg-slate-800 text-slate-100 shadow-md">
                            <h4 class="text-lg font-bold text-amber-300 mb-1" id="card-3-heading">Card 3 Heading</h4>
                            <p class="text-xs text-slate-300 leading-relaxed" id="card-3-p">Toggle sizing modes and drag sliders to directly observe layout reflow in real time.</p>
                        </div>
                    </div>

                    <!-- Controls Accordion -->
                    <div class="mt-4 pt-3 border-t border-slate-700/60 grid grid-cols-2 sm:grid-cols-4 gap-3 text-xs">
                        <div>
                            <label class="text-slate-400 block mb-1">Width (<span id="c3-w-val">240</span>px)</label>
                            <input type="range" min="150" max="320" value="240" oninput="updateCard(3, 'w', this.value)" class="w-full">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">Padding (<span id="c3-p-val">28</span>px)</label>
                            <input type="range" min="0" max="40" value="28" oninput="updateCard(3, 'p', this.value)" class="w-full">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">Border (<span id="c3-b-val">6</span>px)</label>
                            <input type="range" min="0" max="15" value="6" oninput="updateCard(3, 'b', this.value)" class="w-full">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">Margin (<span id="c3-m-val">24</span>px)</label>
                            <input type="range" min="0" max="30" value="24" oninput="updateCard(3, 'm', this.value)" class="w-full">
                        </div>
                    </div>
                </div>

            </div>

            <!-- RIGHT COLUMN: LIVE DEVTOOLS BOX MODEL DIAGRAM (5 cols) -->
            <div class="lg:col-span-5 sticky top-24 space-y-6">
                <div class="bg-slate-800 border border-slate-700 rounded-xl p-5 shadow-xl">
                    
                    <div class="flex justify-between items-center mb-4 pb-3 border-b border-slate-700">
                        <div>
                            <span class="text-xs uppercase font-semibold text-indigo-400 tracking-wider">DevTools Inspector</span>
                            <h3 class="text-lg font-bold text-white flex items-center gap-2">
                                <i data-lucide="crosshair" class="w-5 h-5 text-indigo-400"></i> Box Model Diagram
                            </h3>
                        </div>
                        <span id="active-card-badge" class="px-2.5 py-1 text-xs font-semibold rounded bg-indigo-900/60 text-indigo-300 border border-indigo-700">Inspecting: Card 1</span>
                    </div>

                    <!-- DEVTOOLS NESTED BOX DIAGRAM -->
                    <div class="w-full flex justify-center py-2">
                        <!-- MARGIN BOX -->
                        <div class="highlight-margin rounded-lg p-4 w-full text-center transition-all relative">
                            <span class="absolute top-1 left-2 text-[10px] font-bold tracking-wider text-orange-400 uppercase">Margin</span>
                            <span id="diagram-margin-top" class="text-xs font-mono text-orange-300 block mb-1">12</span>

                            <!-- BORDER BOX -->
                            <div class="highlight-border rounded p-4 text-center transition-all relative">
                                <span class="absolute top-1 left-2 text-[10px] font-bold tracking-wider text-yellow-400 uppercase">Border</span>
                                <span id="diagram-border-top" class="text-xs font-mono text-yellow-300 block mb-1">3</span>

                                <!-- PADDING BOX -->
                                <div class="highlight-padding rounded p-4 text-center transition-all relative">
                                    <span class="absolute top-1 left-2 text-[10px] font-bold tracking-wider text-emerald-400 uppercase">Padding</span>
                                    <span id="diagram-padding-top" class="text-xs font-mono text-emerald-300 block mb-1">16</span>

                                    <!-- CONTENT BOX -->
                                    <div class="highlight-content rounded p-3 text-center transition-all flex flex-col justify-center items-center min-h-[70px]">
                                        <span class="text-[10px] font-bold tracking-wider text-blue-400 uppercase block mb-1">Content</span>
                                        <span id="diagram-content-dims" class="text-sm font-bold font-mono text-blue-200">220 × 98</span>
                                    </div>

                                    <span id="diagram-padding-bottom" class="text-xs font-mono text-emerald-300 block mt-1">16</span>
                                </div>

                                <span id="diagram-border-bottom" class="text-xs font-mono text-yellow-300 block mt-1">3</span>
                            </div>

                            <span id="diagram-margin-bottom" class="text-xs font-mono text-orange-300 block mt-1">12</span>
                        </div>
                    </div>

                    <!-- Computed Dimensions Breakdown -->
                    <div class="mt-5 space-y-3 pt-4 border-t border-slate-700">
                        <div class="flex justify-between items-center text-xs">
                            <span class="text-slate-400">Box-Sizing Mode:</span>
                            <span id="calc-box-sizing" class="font-mono font-bold text-amber-400 bg-slate-900 px-2 py-0.5 rounded">content-box</span>
                        </div>
                        <div class="flex justify-between items-center text-xs">
                            <span class="text-slate-400">Specified Width:</span>
                            <span id="calc-spec-w" class="font-mono text-slate-200">220px</span>
                        </div>
                        <div class="flex justify-between items-center text-xs">
                            <span class="text-slate-400">Computed Content Area:</span>
                            <span id="calc-content-w" class="font-mono text-blue-400 font-semibold">220px</span>
                        </div>

                        <!-- Mathematical Formula Visualizer -->
                        <div class="bg-slate-950 p-3 rounded-lg border border-slate-800 text-xs font-mono space-y-1">
                            <div class="text-slate-400 text-[11px] font-sans font-semibold mb-1">Total Rendered Width Formula:</div>
                            <div id="calc-formula" class="text-indigo-300 font-bold break-all leading-relaxed">
                                220 + (16×2) + (3×2) = 258px
                            </div>
                        </div>

                        <!-- Total Element Footprint -->
                        <div class="grid grid-cols-2 gap-2 text-center pt-2">
                            <div class="bg-slate-900 p-2 rounded border border-slate-700/60">
                                <span class="text-[10px] text-slate-400 block uppercase">Border-Box Total</span>
                                <span id="total-border-box-w" class="text-sm font-bold text-emerald-400 font-mono">258px</span>
                            </div>
                            <div class="bg-slate-900 p-2 rounded border border-slate-700/60">
                                <span class="text-[10px] text-slate-400 block uppercase">With Margins Total</span>
                                <span id="total-margin-box-w" class="text-sm font-bold text-orange-400 font-mono">282px</span>
                            </div>
                        </div>
                    </div>

                </div>
            </div>

        </div>

        <!-- SECTION 2: ANNOTATED EXPLANATION & CHEAT SHEET -->
        <section id="annotation-section" class="bg-slate-800/90 border border-slate-700 rounded-xl p-6 shadow-xl space-y-6">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3 border-b border-slate-700 pb-4">
                <div>
                    <h2 class="text-xl font-bold text-white flex items-center gap-2">
                        <i data-lucide="layers" class="w-5 h-5 text-indigo-400"></i> Annotated Box Model Structure
                    </h2>
                    <p class="text-xs text-slate-400 mt-0.5">Core components required for Assignment 6.1 submission</p>
                </div>
                <button onclick="window.print()" class="no-print px-3 py-1.5 text-xs bg-slate-700 hover:bg-slate-600 text-slate-200 rounded-lg flex items-center gap-1.5 transition">
                    <i data-lucide="printer" class="w-4 h-4"></i> Print / Save PDF View
                </button>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                
                <!-- CONTENT CARD -->
                <div class="bg-slate-900 border border-blue-500/40 rounded-xl p-4 flex flex-col justify-between">
                    <div>
                        <div class="flex items-center gap-2 mb-2">
                            <div class="w-3 h-3 rounded-full bg-boxContent"></div>
                            <h3 class="font-bold text-blue-400">1. Content</h3>
                        </div>
                        <p class="text-xs text-slate-300 leading-relaxed">
                            The innermost layer containing text, images, or child HTML elements. Its dimensions are defined by <code class="text-blue-300">width</code> and <code class="text-blue-300">height</code>.
                        </p>
                    </div>
                    <div class="mt-4 pt-2 border-t border-slate-800 text-[11px] font-mono text-slate-400">
                        CSS: <span class="text-blue-300">width: 200px; height: auto;</span>
                    </div>
                </div>

                <!-- PADDING CARD -->
                <div class="bg-slate-900 border border-green-500/40 rounded-xl p-4 flex flex-col justify-between">
                    <div>
                        <div class="flex items-center gap-2 mb-2">
                            <div class="w-3 h-3 rounded-full bg-boxPadding"></div>
                            <h3 class="font-bold text-emerald-400">2. Padding</h3>
                        </div>
                        <p class="text-xs text-slate-300 leading-relaxed">
                            The transparent space surrounding the content inside the border. Inherits the background color/image of the element.
                        </p>
                    </div>
                    <div class="mt-4 pt-2 border-t border-slate-800 text-[11px] font-mono text-slate-400">
                        CSS: <span class="text-emerald-300">padding: 16px;</span>
                    </div>
                </div>

                <!-- BORDER CARD -->
                <div class="bg-slate-900 border border-yellow-500/40 rounded-xl p-4 flex flex-col justify-between">
                    <div>
                        <div class="flex items-center gap-2 mb-2">
                            <div class="w-3 h-3 rounded-full bg-boxBorder"></div>
                            <h3 class="font-bold text-yellow-400">3. Border</h3>
                        </div>
                        <p class="text-xs text-slate-300 leading-relaxed">
                            The perimeter line wrapping around padding and content. Customizable with width, style (solid/dashed), and color.
                        </p>
                    </div>
                    <div class="mt-4 pt-2 border-t border-slate-800 text-[11px] font-mono text-slate-400">
                        CSS: <span class="text-yellow-300">border: 3px solid #eab308;</span>
                    </div>
                </div>

                <!-- MARGIN CARD -->
                <div class="bg-slate-900 border border-orange-500/40 rounded-xl p-4 flex flex-col justify-between">
                    <div>
                        <div class="flex items-center gap-2 mb-2">
                            <div class="w-3 h-3 rounded-full bg-boxMargin"></div>
                            <h3 class="font-bold text-orange-400">4. Margin</h3>
                        </div>
                        <p class="text-xs text-slate-300 leading-relaxed">
                            The completely transparent outer spacing separating this element from adjacent sibling elements and parent walls.
                        </p>
                    </div>
                    <div class="mt-4 pt-2 border-t border-slate-800 text-[11px] font-mono text-slate-400">
                        CSS: <span class="text-orange-300">margin: 12px;</span>
                    </div>
                </div>

            </div>

            <!-- DEVTOOLS INSTRUCTION GUIDE -->
            <div class="bg-slate-950 rounded-xl p-5 border border-slate-800">
                <h3 class="font-bold text-white text-sm flex items-center gap-2 mb-3">
                    <i data-lucide="terminal" class="w-4 h-4 text-indigo-400"></i> Steps to Inspect in Chrome DevTools (Step 5 & 6 of Assignment):
                </h3>
                <ol class="list-decimal list-inside text-xs text-slate-300 space-y-2 leading-relaxed">
                    <li>Right-click on any card on this page and click <strong class="text-indigo-300">Inspect</strong> (or press <code class="bg-slate-800 px-1 py-0.5 rounded">F12</code> / <code class="bg-slate-800 px-1 py-0.5 rounded">Cmd+Option+I</code>).</li>
                    <li>Ensure the <strong class="text-indigo-300">Elements</strong> tab is selected in DevTools.</li>
                    <li>In the right/bottom sidebar, select the <strong class="text-indigo-300">Computed</strong> sub-tab.</li>
                    <li>Scroll down to view the live nested square box model diagram showing margin, border, padding, and content values.</li>
                    <li>Take a screenshot of the DevTools panel for your final assignment submission.</li>
                </ol>
            </div>
        </section>

    </main>

    <!-- CODE EXPORT MODAL -->
    <div id="code-modal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-slate-900 border border-slate-700 rounded-xl max-w-2xl w-full p-6 shadow-2xl space-y-4">
            <div class="flex justify-between items-center border-b border-slate-800 pb-3">
                <h3 class="text-lg font-bold text-white flex items-center gap-2">
                    <i data-lucide="file-code" class="w-5 h-5 text-indigo-400"></i> Assignment 6.1 Solution HTML/CSS Code
                </h3>
                <button onclick="closeCodeModal()" class="text-slate-400 hover:text-white">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            
            <p class="text-xs text-slate-300">
                You can copy this standalone, pure HTML & CSS source code snippet to turn in for your assignment code submission:
            </p>

            <pre class="bg-slate-950 p-4 rounded-lg border border-slate-800 text-xs text-indigo-200 font-mono overflow-x-auto max-h-80 select-all" id="export-code-block"></pre>

            <div class="flex justify-end gap-3 pt-2">
                <button onclick="closeCodeModal()" class="px-4 py-2 text-xs bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-lg">Close</button>
                <button onclick="copySubmissionCode()" id="copy-btn" class="px-4 py-2 text-xs bg-indigo-600 hover:bg-indigo-500 text-white rounded-lg font-medium flex items-center gap-1.5">
                    <i data-lucide="copy" class="w-4 h-4"></i> Copy Code
                </button>
            </div>
        </div>
    </div>

    <!-- JAVASCRIPT LOGIC -->
    <script>
        // State for 3 Cards
        const cardState = {
            1: { width: 220, padding: 16, border: 3, margin: 12, boxSizing: 'content-box', borderColor: '#6366f1' },
            2: { width: 260, padding: 20, border: 4, margin: 16, boxSizing: 'border-box', borderColor: '#10b981' },
            3: { width: 240, padding: 28, border: 6, margin: 24, boxSizing: 'border-box', borderColor: '#f59e0b' }
        };

        let activeCardId = 1;

        // Initialize lucide icons and apply state
        document.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            applyCardStyles(1);
            applyCardStyles(2);
            applyCardStyles(3);
            selectCard(1);
        });

        // Update single card parameters
        function updateCard(id, prop, val) {
            val = parseInt(val);
            if (prop === 'w') cardState[id].width = val;
            if (prop === 'p') cardState[id].padding = val;
            if (prop === 'b') cardState[id].border = val;
            if (prop === 'm') cardState[id].margin = val;

            // Update UI slider labels
            document.getElementById(`c${id}-${prop}-val`).innerText = val;

            applyCardStyles(id);

            // Re-render diagram if currently active card changed
            if (activeCardId === id) {
                renderDiagram();
            }
        }

        // Apply dynamic inline CSS styles to specified card
        function applyCardStyles(id) {
            const card = cardState[id];
            const el = document.getElementById(`card-${id}-element`);

            el.style.width = card.width + 'px';
            el.style.padding = card.padding + 'px';
            el.style.borderWidth = card.border + 'px';
            el.style.borderStyle = 'solid';
            el.style.borderColor = card.borderColor;
            el.style.margin = card.margin + 'px';
            el.style.boxSizing = card.boxSizing;
        }

        // Toggle card 3 sizing mode
        function toggleCard3Sizing(mode) {
            cardState[3].boxSizing = mode;
            applyCardStyles(3);
            if (activeCardId === 3) {
                renderDiagram();
            }
        }

        // Highlight selected card and update DevTools Diagram
        function selectCard(id) {
            activeCardId = id;

            // Update card border highlights
            for (let i = 1; i <= 3; i++) {
                const wrapper = document.getElementById(`card-wrapper-${i}`);
                if (i === id) {
                    wrapper.classList.add('border-indigo-500', 'shadow-indigo-950/40');
                    wrapper.classList.remove('border-slate-700/80');
                } else {
                    wrapper.classList.remove('border-indigo-500', 'shadow-indigo-950/40');
                    wrapper.classList.add('border-slate-700/80');
                }
            }

            document.getElementById('active-card-badge').innerText = `Inspecting: Card ${id}`;
            renderDiagram();
        }

        // Render live DevTools Diagram & Computations
        function renderDiagram() {
            const card = cardState[activeCardId];
            const el = document.getElementById(`card-${activeCardId}-element`);

            // Read computed heights from rendered DOM element
            const computedHeight = el.clientHeight; // includes padding and border if border-box

            // Text dimensions
            document.getElementById('diagram-margin-top').innerText = card.margin;
            document.getElementById('diagram-margin-bottom').innerText = card.margin;
            document.getElementById('diagram-border-top').innerText = card.border;
            document.getElementById('diagram-border-bottom').innerText = card.border;
            document.getElementById('diagram-padding-top').innerText = card.padding;
            document.getElementById('diagram-padding-bottom').innerText = card.padding;

            // Compute exact content dimensions based on sizing mode
            let contentW, contentH;
            let totalBorderBoxW;

            if (card.boxSizing === 'content-box') {
                contentW = card.width;
                totalBorderBoxW = card.width + (card.padding * 2) + (card.border * 2);
                document.getElementById('calc-formula').innerText = 
                    `${card.width} (width) + ${card.padding * 2} (padding) + ${card.border * 2} (border) = ${totalBorderBoxW}px`;
            } else {
                // border-box
                totalBorderBoxW = card.width;
                contentW = Math.max(0, card.width - (card.padding * 2) - (card.border * 2));
                document.getElementById('calc-formula').innerText = 
                    `${card.width} (specified width) - ${card.padding * 2} (padding) - ${card.border * 2} (border) = ${contentW}px content area`;
            }

            // Approximate inner height text
            contentH = Math.max(20, computedHeight - (card.padding * 2) - (card.border * 2));

            document.getElementById('diagram-content-dims').innerText = `${contentW} × ${contentH}`;

            // Update stats panel
            document.getElementById('calc-box-sizing').innerText = card.boxSizing;
            document.getElementById('calc-spec-w').innerText = card.width + 'px';
            document.getElementById('calc-content-w').innerText = contentW + 'px';

            document.getElementById('total-border-box-w').innerText = totalBorderBoxW + 'px';
            document.getElementById('total-margin-box-w').innerText = (totalBorderBoxW + (card.margin * 2)) + 'px';
        }

        // Scroll to smooth anchor
        function scrollToSection(id) {
            document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
        }

        // Code Generation Modal
        function exportCodeModal() {
            const standaloneHTML = `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Assignment 6.1 — Box Model Inspection Solution</title>
    <style>
        /* CSS Reset & General Styles */
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: sans-serif; background: #0f172a; color: #f8fafc; padding: 2rem; }
        h1 { margin-bottom: 1.5rem; text-align: center; }
        .container { display: flex; flex-wrap: wrap; gap: 2rem; justify-content: center; }

        /* Card 1: content-box */
        .card-1 {
            box-sizing: content-box;
            width: ${cardState[1].width}px;
            padding: ${cardState[1].padding}px;
            border: ${cardState[1].border}px solid ${cardState[1].borderColor};
            margin: ${cardState[1].margin}px;
            background-color: #1e293b;
            border-radius: 8px;
        }

        /* Card 2: border-box */
        .card-2 {
            box-sizing: border-box;
            width: ${cardState[2].width}px;
            padding: ${cardState[2].padding}px;
            border: ${cardState[2].border}px solid ${cardState[2].borderColor};
            margin: ${cardState[2].margin}px;
            background-color: #1e293b;
            border-radius: 8px;
        }

        /* Card 3: custom border-box */
        .card-3 {
            box-sizing: ${cardState[3].boxSizing};
            width: ${cardState[3].width}px;
            padding: ${cardState[3].padding}px;
            border: ${cardState[3].border}px solid ${cardState[3].borderColor};
            margin: ${cardState[3].margin}px;
            background-color: #1e293b;
            border-radius: 8px;
        }

        h2 { margin-bottom: 0.5rem; font-size: 1.2rem; }
        p { font-size: 0.875rem; color: #94a3b8; }
    </style>
</head>
<body>
    <h1>Assignment 6.1 — Box Model Inspection</h1>
    <div class="container">
        <!-- Card 1 -->
        <div class="card-1">
            <h2>Card 1 (content-box)</h2>
            <p>This box uses content-box sizing. Total rendered width equals width + padding + border.</p>
        </div>

        <!-- Card 2 -->
        <div class="card-2">
            <h2>Card 2 (border-box)</h2>
            <p>This box uses border-box sizing. Padding and border fit inside the specified width.</p>
        </div>

        <!-- Card 3 -->
        <div class="card-3">
            <h2>Card 3 (${cardState[3].boxSizing})</h2>
            <p>Expanded padding and margin card for inspecting dynamic spatial adjustments.</p>
        </div>
    </div>
</body>
</html>`;

            document.getElementById('export-code-block').textContent = standaloneHTML;
            document.getElementById('code-modal').classList.remove('hidden');
        }

        function closeCodeModal() {
            document.getElementById('code-modal').classList.add('hidden');
        }

        function copySubmissionCode() {
            const codeText = document.getElementById('export-code-block').textContent;
            
            // Clipboard fallback
            const dummy = document.createElement("textarea");
            document.body.appendChild(dummy);
            dummy.value = codeText;
            dummy.select();
            document.execCommand("copy");
            document.body.removeChild(dummy);

            const btn = document.getElementById('copy-btn');
            btn.innerHTML = `<i data-lucide="check" class="w-4 h-4"></i> Copied!`;
            setTimeout(() => {
                btn.innerHTML = `<i data-lucide="copy" class="w-4 h-4"></i> Copy Code`;
                lucide.createIcons();
            }, 2000);
        }
    </script>
</body>
</html>
Features Included:
Interactive Workspace: Features Card 1 (content-box), Card 2 (border-box), and Card 3 (dynamic laboratory toggle) each containing headings, paragraphs, and sliders to adjust content width, padding, border, and margin in real time.

DevTools Live Inspector: Visual nested box model diagram (Content -> Padding -> Border -> Margin) showing live pixel calculations, specified width vs computed width, and mathematical formulas.

Annotated Theory & Export: Detailed breakdown of Content, Padding, Border, and Margin with Chrome DevTools inspection instructions and an instant HTML/CSS code export tool for assignment submission
# AI Agent Instructions: Presentation Framework

Generate standalone HTML presentations with professional PowerPoint-style design. Output is a single HTML file containing all styles, scripts, and content.

---

## Agent Role & Responsibilities

**YOU decide the presentation structure.** This framework provides the building blocks (slide types, styling, animations) - you determine:

- **Number of slides** based on content depth needed
- **Which slide types to use** based on the content being presented
- **Content flow and narrative** that tells a coherent story
- **When to use animations** for emphasis (sparingly is better)
- **Visual choices** (tables vs lists, charts vs bullet points)

### Request Validation

Before generating, ensure you have sufficient information. If the user's request is unclear or missing critical details, **ask for clarification** instead of guessing:

**Required Information:**
- Topic or subject matter
- Purpose/audience (informational, persuasive, training, etc.)
- Approximate scope (quick overview vs deep dive)

**Helpful but Optional:**
- Specific points to cover
- Preferred slide count
- Tone (formal, casual, technical)
- Any required sections or content

**If Missing Critical Info, Respond Like:**
> "I can create this presentation, but I need a bit more context:
> - [Specific question about missing info]
> - [Another question if needed]
> 
> Or if you'd like, I can create a general overview with [X] slides covering the basics."

---

## Document Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[PRESENTATION TITLE]</title>
    <style>/* ALL STYLES HERE */</style>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
</head>
<body>
    <div class="slide-container">
        <!-- SLIDES HERE -->
    </div>
    <!-- UI ELEMENTS -->
    <div class="slide-number"><span id="currentSlide">1</span> / <span id="totalSlides">X</span></div>
    <div class="progress-bar" id="progressBar"></div>
    <div class="laser-pointer" id="laserPointer"></div>
    <div class="controls">
        <button id="laserBtn" class="laser-toggle">🔴 L</button>
        <button id="fullscreenBtn" class="fullscreen-toggle">⛶ F</button>
        <button id="prevBtn">← Previous</button>
        <button id="nextBtn">Next →</button>
    </div>
    <script>/* NAVIGATION SCRIPT HERE */</script>
</body>
</html>
```

---

## Slide Types & Templates

### 1. Title Slide (First Slide)

```html
<div class="slide title-slide active bg-first-slide" data-slide="1">
    <div class="slide-content">
        <h1>[MAIN TITLE]</h1>
        <p class="subtitle">[SUBTITLE]</p>
        <div>
            <span class="badge">[BADGE 1]</span>
            <span class="badge">[BADGE 2]</span>
        </div>
    </div>
</div>
```

**Rules:**
- Always first slide, always `active` class initially
- Use `bg-first-slide` background
- Badges optional, 2-4 recommended
- Title: h1, Subtitle: p.subtitle

---

### 2. Bullet List Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[SLIDE TITLE]</h2>
        <p>[OPTIONAL INTRO TEXT]</p>
        <ul>
            <li>[ITEM 1]</li>
            <li>[ITEM 2]</li>
            <li><strong>[BOLD PART]</strong> - [DESCRIPTION]</li>
        </ul>
    </div>
</div>
```

**Rules:**
- 3-6 bullet points optimal
- Use `<strong>` for key terms
- Optional intro paragraph before list

---

### 3. Numbered List Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[SLIDE TITLE]</h2>
        <h3>[OPTIONAL SECTION HEADER]</h3>
        <ol>
            <li><strong>[STEP NAME]</strong> - [DESCRIPTION]</li>
            <li><strong>[STEP NAME]</strong> - [DESCRIPTION]</li>
        </ol>
    </div>
</div>
```

**Rules:**
- Use for sequential steps, processes, ranked items
- Keep to 3-7 items

---

### 4. Table Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[SLIDE TITLE]</h2>
        <table>
            <thead>
                <tr>
                    <th>[COL 1]</th>
                    <th>[COL 2]</th>
                    <th>[COL 3]</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>[DATA]</td>
                    <td>✅ [POSITIVE]</td>
                    <td>❌ [NEGATIVE]</td>
                </tr>
            </tbody>
        </table>
    </div>
</div>
```

**Rules:**
- 2-5 columns recommended
- 3-6 rows optimal for readability
- Use emoji indicators: ✅ ❌ ⚠️ ⏱️ ⚡

---

### 5. Chart Slide (Bar Chart)

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[SLIDE TITLE]</h2>
        <div class="chart-container">
            <canvas id="[UNIQUE_CHART_ID]"></canvas>
        </div>
        <p><span class="highlight">[KEY INSIGHT]</span></p>
    </div>
</div>
```

**JavaScript (add to chart initialization):**
```javascript
function init[ChartName](canvas) {
    const ctx = canvas.getContext('2d');
    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: ['Label1', 'Label2'],
            datasets: [{
                label: '[DATASET LABEL]',
                data: [VALUE1, VALUE2],
                backgroundColor: ['#dc3545', '#28a745'],
                borderColor: ['#bd2130', '#218838'],
                borderWidth: 2
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: true,
            plugins: { legend: { labels: { font: { size: 18, family: 'Calibri' }}}},
            scales: {
                y: { beginAtZero: true, ticks: { font: { size: 16, family: 'Calibri' }}},
                x: { ticks: { font: { size: 16, family: 'Calibri' }}}
            }
        }
    });
}
```

**Rules:**
- Each chart needs unique canvas ID
- Register in `initChartsOnSlide()` function
- Colors: red (#dc3545) for negative, green (#28a745) for positive, blue (#0066cc) for neutral

---

### 6. Image Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[SLIDE TITLE]</h2>
        <p>[DESCRIPTION]</p>
        <div class="image-demo">
            <img src="[IMAGE_URL]" alt="[ALT TEXT]">
            <div style="flex: 1; min-width: 300px;">
                <h3>[IMAGE CAPTION/TITLE]</h3>
                <ul>
                    <li>[POINT 1]</li>
                    <li>[POINT 2]</li>
                </ul>
            </div>
        </div>
    </div>
</div>
```

**Rules:**
- Always include alt text
- Side content optional (use for image explanation)
- Supported: URLs, local files, base64

---

### 7. Animation Showcase Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2 class="anim-fade-up">[SLIDE TITLE]</h2>
        <p class="anim-fade-up anim-delay-1">[INTRO TEXT]</p>
        <div style="display: flex; flex-wrap: wrap; margin-top: 30px;">
            <div class="animation-demo-box anim-fade-left anim-delay-2">[LABEL]</div>
            <div class="animation-demo-box anim-fade-up anim-delay-3">[LABEL]</div>
            <div class="animation-demo-box anim-scale anim-delay-4">[LABEL]</div>
            <div class="animation-demo-box anim-pulse">[CONTINUOUS]</div>
        </div>
        <p class="anim-fade-up anim-delay-5">
            <span class="shimmer-text" style="font-size: 2.2rem; font-weight: 600;">[EMPHASIZED TEXT]</span>
        </p>
    </div>
</div>
```

**Animation Classes:**
| Class | Effect | Use Case |
|-------|--------|----------|
| `anim-fade-up` | Fade in from below | Default entrance |
| `anim-fade-left` | Fade in from left | Sequential items |
| `anim-fade-right` | Fade in from right | Contrast/comparison |
| `anim-scale` | Scale in from smaller | Important elements |
| `anim-pulse` | Continuous breathing | Persistent attention |
| `shimmer-text` | Gradient sweep | Text emphasis |

**Delay Classes:** `anim-delay-1` through `anim-delay-5` (0.6s increments)

---

### 8. Two-Column Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[SLIDE TITLE]</h2>
        <div class="two-column">
            <div class="column">
                <h3>[LEFT HEADER]</h3>
                <ul>
                    <li>[ITEM]</li>
                </ul>
            </div>
            <div class="column">
                <h3>[RIGHT HEADER]</h3>
                <ul>
                    <li>[ITEM]</li>
                </ul>
            </div>
        </div>
        <p style="margin-top: 30px;">[SUMMARY/CONCLUSION]</p>
    </div>
</div>
```

**Rules:**
- Use for: Before/After, Pros/Cons, Comparison, Parallel concepts
- Keep columns balanced in content length
- Collapses to single column on mobile

---

### 9. Quote/Callout Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[SLIDE TITLE]</h2>
        <div class="quote-block">
            <p>[QUOTE TEXT]</p>
            <div class="attribution">— [AUTHOR]</div>
        </div>
        <div class="callout">
            <p><strong>💡 Pro Tip:</strong> [TIP TEXT]</p>
        </div>
        <div class="callout warning">
            <p><strong>⚠️ Warning:</strong> [WARNING TEXT]</p>
        </div>
        <div class="callout success">
            <p><strong>✅ Success:</strong> [SUCCESS TEXT]</p>
        </div>
    </div>
</div>
```

**Callout Variants:**
- Default (blue): Information, tips
- `.warning` (orange): Cautions, important notes
- `.success` (green): Confirmations, achievements

---

### 10. Code Block Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[SLIDE TITLE]</h2>
        <p>[DESCRIPTION]</p>
        <div class="code-header">[LANGUAGE]</div>
        <div class="code-block"><span class="comment">// Comment</span>
<span class="keyword">function</span> <span class="function">name</span>(param) {
    <span class="keyword">return</span> <span class="string">"value"</span>;
}</div>
    </div>
</div>
```

**Syntax Highlighting Classes:**
| Class | Color | Use For |
|-------|-------|---------|
| `.comment` | Green (#6a9955) | Comments |
| `.keyword` | Blue (#569cd6) | Keywords (function, return, const, etc.) |
| `.string` | Orange (#ce9178) | String literals |
| `.function` | Yellow (#dcdcaa) | Function names |
| `.number` | Light green (#b5cea8) | Numbers |
| `.tag` | Blue (#569cd6) | HTML tags |
| `.attribute` | Light blue (#9cdcfe) | Attributes |

**Rules:**
- NO indentation on first line of code-block content
- Preserve exact whitespace for code formatting
- Include code-header with language name

---

### 11. Icon Grid Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[SLIDE TITLE]</h2>
        <p>[DESCRIPTION]</p>
        <div class="icon-grid">
            <div class="icon-item">
                <div class="icon">
                    <svg viewBox="0 0 24 24"><path d="[SVG PATH]"/></svg>
                </div>
                <div class="icon-content">
                    <h4>[FEATURE TITLE]</h4>
                    <p>[FEATURE DESCRIPTION]</p>
                </div>
            </div>
            <!-- Repeat icon-item for each feature -->
        </div>
    </div>
</div>
```

**Rules:**
- 3-6 icon items recommended
- Use Material Design Icons (MDI) SVG paths
- Grid auto-fits 2-3 columns on desktop, 1 on mobile

**Common SVG Icons:**
- Rocket: `M13.13 22.19L11.5 18.36...` (speed/launch)
- Palette: `M12,22A10,10 0 0,1 2,12...` (design)
- Phone: `M17,19H7V5H17...` (responsive)
- Plugin: `M16,7V3H14V7...` (offline)

---

### 12. CTA (Call-to-Action) Slide

```html
<div class="slide cta-slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[CTA HEADLINE]</h2>
        <p class="cta-subtitle">[SUPPORTING TEXT]</p>
        <div class="cta-buttons">
            <a href="[URL]" class="cta-button primary">[PRIMARY ACTION]</a>
            <a href="[URL]" class="cta-button secondary">[SECONDARY ACTION]</a>
        </div>
        <div class="cta-contact">
            [CONTACT TEXT] <a href="mailto:[EMAIL]">[EMAIL]</a>
        </div>
    </div>
</div>
```

**Rules:**
- Always last slide
- Add `cta-slide` class to slide element
- Primary button: main action, Secondary button: alternative
- Contact info optional

---

## Design System

### Colors
| Purpose | Hex | Usage |
|---------|-----|-------|
| Primary | `#0066cc` | Links, buttons, accents, bullets |
| Primary Hover | `#0052a3` | Button hover states |
| Text Primary | `#1a1a1a` | Headings |
| Text Secondary | `#333` | Body text |
| Text Muted | `#666` | Captions, attributions |
| Background Dark | `#1a1a1a` | Page background |
| Border | `#444` | Slide frame |
| Success | `#28a745` / `#4caf50` | Positive indicators |
| Warning | `#ff9800` | Caution indicators |
| Error | `#dc3545` | Negative indicators |

### Typography
| Element | Size | Weight |
|---------|------|--------|
| h1 (Title) | 5rem | 600 |
| h2 (Slide title) | 4rem | 600 |
| h3 (Section) | 2.5rem | 600 |
| p, li (Body) | 2.7rem | 400 |
| Table | 2rem | 400 |
| Badge | 1.6rem | 600 |
| Code | 1.6rem | 400 |

### Font Stack
```css
font-family: 'Calibri', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
```

---

## Background Images

**Title Slide:** `presentation_first_slide.png`
```css
.bg-first-slide { background-image: url('presentation_first_slide.png'); }
```

**Content Slides:** `presentation_default_slide.png`
```css
.bg-default-slide { background-image: url('presentation_default_slide.png'); }
```

---

## Navigation & Controls

### Keyboard Shortcuts
| Key | Action |
|-----|--------|
| → / Space | Next slide |
| ← | Previous slide |
| F | Toggle fullscreen |
| L | Toggle laser pointer |
| Esc | Exit laser mode |

### Touch Support
- Swipe left → Next slide
- Swipe right → Previous slide
- Threshold: 50px

---

## Required JavaScript

Include this script block before `</body>`:

```javascript
let currentSlide = 1;
const slides = document.querySelectorAll('.slide');
const totalSlides = slides.length;

document.getElementById('totalSlides').textContent = totalSlides;
updateProgressBar();

function updateProgressBar() {
    const progress = (currentSlide / totalSlides) * 100;
    document.getElementById('progressBar').style.width = progress + '%';
}

function updateSlide(direction) {
    const current = slides[currentSlide - 1];
    
    if (direction === 'next' && currentSlide < totalSlides) {
        current.classList.remove('active');
        current.classList.add('prev');
        currentSlide++;
        slides[currentSlide - 1].classList.add('active');
        slides[currentSlide - 1].classList.remove('prev');
    } else if (direction === 'prev' && currentSlide > 1) {
        current.classList.remove('active');
        currentSlide--;
        slides[currentSlide - 1].classList.remove('prev');
        slides[currentSlide - 1].classList.add('active');
    }
    
    document.getElementById('currentSlide').textContent = currentSlide;
    updateProgressBar();
    initChartsOnSlide(slides[currentSlide - 1]);
}

function initChartsOnSlide(slide) {
    const canvases = slide.querySelectorAll('canvas:not([data-initialized])');
    canvases.forEach(canvas => {
        // Add chart initialization by ID here
        canvas.setAttribute('data-initialized', 'true');
    });
}

// Auto-hide controls
let mouseTimer = null;
const controls = document.querySelector('.controls');

function showControls() {
    controls.classList.remove('hidden');
    clearTimeout(mouseTimer);
    mouseTimer = setTimeout(() => controls.classList.add('hidden'), 2000);
}

document.addEventListener('mousemove', showControls);
showControls();

document.getElementById('nextBtn').addEventListener('click', () => updateSlide('next'));
document.getElementById('prevBtn').addEventListener('click', () => updateSlide('prev'));

// Laser pointer
const laserPointer = document.getElementById('laserPointer');
const laserBtn = document.getElementById('laserBtn');
let laserMode = false;

function toggleLaser() {
    laserMode = !laserMode;
    laserPointer.classList.toggle('active', laserMode);
    laserBtn.classList.toggle('active', laserMode);
    document.body.classList.toggle('laser-mode', laserMode);
}

laserBtn.addEventListener('click', toggleLaser);
document.getElementById('fullscreenBtn').addEventListener('click', toggleFullscreen);

document.addEventListener('mousemove', (e) => {
    if (laserMode) {
        laserPointer.style.left = e.clientX + 'px';
        laserPointer.style.top = e.clientY + 'px';
    }
});

// Keyboard navigation
document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowRight' || e.key === ' ') updateSlide('next');
    else if (e.key === 'ArrowLeft') updateSlide('prev');
    else if (e.key === 'f' || e.key === 'F') toggleFullscreen();
    else if (e.key === 'l' || e.key === 'L') toggleLaser();
    else if (e.key === 'Escape' && laserMode) toggleLaser();
});

// Touch/swipe
let touchStartX = 0, touchEndX = 0;
const swipeThreshold = 50;

document.addEventListener('touchstart', (e) => {
    touchStartX = e.changedTouches[0].screenX;
}, { passive: true });

document.addEventListener('touchend', (e) => {
    touchEndX = e.changedTouches[0].screenX;
    const diff = touchStartX - touchEndX;
    if (Math.abs(diff) > swipeThreshold) {
        updateSlide(diff > 0 ? 'next' : 'prev');
    }
}, { passive: true });

// Fullscreen
function toggleFullscreen() {
    if (!document.fullscreenElement) document.documentElement.requestFullscreen();
    else document.exitFullscreen();
}
```

---

## Print/PDF Export

Built-in via `Ctrl+P`. Behavior:
- Title slide: **Keeps background image**
- Content slides: **White background** (for readability)
- All UI elements hidden
- Animations rendered in final state
- Landscape orientation, page-break per slide

---

## Checklist Before Output

1. ☐ First slide has `active` class
2. ☐ All slides have sequential `data-slide="N"` attributes
3. ☐ `totalSlides` matches actual slide count
4. ☐ All charts registered in `initChartsOnSlide()`
5. ☐ Chart.js CDN included if using charts
6. ☐ Background images referenced correctly
7. ☐ Code blocks have proper syntax highlighting
8. ☐ CTA slide is last (if included)
9. ☐ All canvas elements have unique IDs

---

## Content Best Practices

### Slide Count Guidelines
| Presentation Type | Recommended Slides |
|-------------------|-------------------|
| Quick pitch / intro | 3-5 |
| Standard presentation | 6-10 |
| Deep dive / training | 10-15 |
| Workshop / comprehensive | 15-25 |

### Narrative Flow Patterns

**Problem → Solution:**
1. Title → 2. Problem/Challenge → 3. Solution → 4. Features/Benefits → 5. CTA

**Feature Showcase:**
1. Title → 2. Overview → 3-N. Feature slides → N+1. Summary → CTA

**Comparison/Decision:**
1. Title → 2. Context → 3. Option A → 4. Option B → 5. Comparison Table → 6. Recommendation

**Training/Tutorial:**
1. Title → 2. Objectives → 3-N. Steps with examples → N+1. Summary → N+2. Resources

### Content Density Rules
- **One idea per slide** - don't overload
- **Max 6 bullet points** per slide
- **Max 6 table rows** visible without scrolling
- **Headlines should stand alone** - if someone only reads titles, they get the story

### When to Use Each Slide Type
| Content Type | Best Slide Type |
|--------------|-----------------|
| Opening / closing | Title, CTA |
| List of items (unordered) | Bullet List |
| Sequential steps | Numbered List |
| Data comparison | Table |
| Quantitative data | Chart |
| Visual explanation | Image |
| Feature overview | Icon Grid |
| Before/After, Pros/Cons | Two-Column |
| Expert opinion, emphasis | Quote/Callout |
| Technical demo | Code Block |
| Key takeaways | Callout boxes |

---

## Common Pitfalls to Avoid

1. **Missing `active` class** on first slide → blank screen
2. **Wrong data-slide sequence** → navigation breaks
3. **Code block indentation** → first line must start at column 0
4. **Forgetting Chart.js CDN** when using charts
5. **Too much text** per slide → unreadable in presentation mode
6. **Overusing animations** → distracting instead of enhancing
7. **Inconsistent heading levels** → h2 for slide titles, h3 for sections

---

## Quick Reference: Minimal Slide

```html
<div class="slide bg-default-slide" data-slide="N">
    <div class="slide-content">
        <h2>[TITLE]</h2>
        <!-- CONTENT -->
    </div>
</div>
```

Every slide MUST have:
- `class="slide bg-default-slide"` (or `bg-first-slide` for title)
- `data-slide="N"` (sequential number)
- `<div class="slide-content">` wrapper
---

## Complete CSS Stylesheet

Copy this entire stylesheet into the `<style>` tag:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Calibri', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    overflow: hidden;
    background: #1a1a1a;
    padding: 20px;
}

.slide-container {
    width: calc(100vw - 40px);
    height: calc(100vh - 40px);
    position: relative;
    overflow: hidden;
    border-radius: 12px;
    border: 5px solid #444;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
}

.slide {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    padding: 60px 80px;
    opacity: 0;
    transform: translateX(100%);
    transition: all 0.5s ease-in-out;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
}

.slide.active {
    opacity: 1;
    transform: translateX(0);
    z-index: 10;
}

.slide.prev {
    transform: translateX(-100%);
}

.slide-content {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
}

/* Title slide specific */
.slide.title-slide .slide-content {
    justify-content: center;
    align-items: flex-start;
    padding-left: 100px;
}

.slide.title-slide h1 {
    font-size: 5rem;
    color: #1a1a1a;
    margin-bottom: 30px;
    font-weight: 600;
    line-height: 1.2;
}

.slide.title-slide .subtitle {
    font-size: 2.7rem;
    color: #4a4a4a;
    margin-bottom: 40px;
}

/* Content slides */
h2 {
    font-size: 4rem;
    color: #1a1a1a;
    margin-bottom: 40px;
    font-weight: 600;
    border-bottom: 4px solid #0066cc;
    padding-bottom: 15px;
    display: inline-block;
}

h3 {
    font-size: 2.5rem;
    color: #333;
    margin-bottom: 20px;
    margin-top: 30px;
    font-weight: 600;
}

p {
    font-size: 2.7rem;
    line-height: 1.6;
    color: #333;
    margin-bottom: 20px;
}

ul, ol {
    font-size: 2.7rem;
    line-height: 1.8;
    color: #333;
    margin-left: 0;
    list-style-position: outside;
    padding-left: 40px;
}

ul {
    list-style-type: none;
}

ul li {
    margin-bottom: 20px;
    padding-left: 30px;
    position: relative;
}

ul li::before {
    content: "●";
    position: absolute;
    left: 0;
    color: #0066cc;
    font-size: 1.5rem;
}

ol {
    list-style-type: decimal;
}

ol li {
    margin-bottom: 20px;
    padding-left: 15px;
}

strong {
    color: #000;
    font-weight: 600;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin: 30px 0;
    font-size: 2rem;
    background: white;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

th {
    background: #0066cc;
    color: white;
    padding: 20px;
    text-align: left;
    font-weight: 600;
    border: 1px solid #0052a3;
}

td {
    padding: 18px 20px;
    border: 1px solid #ddd;
    color: #333;
}

tr:nth-child(even) {
    background: #f5f5f5;
}

.chart-container {
    width: 100%;
    max-width: 900px;
    margin: 30px 0;
    background: white;
    padding: 30px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.controls {
    position: fixed;
    bottom: 30px;
    right: 30px;
    display: flex;
    gap: 15px;
    z-index: 100;
    opacity: 1;
    transition: opacity 0.5s ease;
}

.controls.hidden {
    opacity: 0;
    pointer-events: none;
}

button {
    background: #0066cc;
    color: white;
    border: none;
    padding: 12px 25px;
    font-size: 1.1rem;
    border-radius: 4px;
    cursor: pointer;
    font-family: 'Calibri', sans-serif;
    font-weight: 600;
    transition: background 0.2s ease;
}

button:hover {
    background: #0052a3;
}

button:active {
    background: #004080;
}

.slide-number {
    position: fixed;
    bottom: 30px;
    left: 30px;
    color: #1a1a1a;
    font-size: 1.1rem;
    background: rgba(255, 255, 255, 0.9);
    padding: 8px 16px;
    border-radius: 4px;
    z-index: 100;
    font-weight: 600;
}

.progress-bar {
    position: fixed;
    bottom: 0;
    left: 0;
    height: 4px;
    background: #0066cc;
    z-index: 100;
    transition: width 0.3s ease;
}

/* Laser pointer mode */
.laser-pointer {
    position: fixed;
    width: 12px;
    height: 12px;
    background: radial-gradient(circle, #ff0000 0%, #ff0000 40%, rgba(255, 0, 0, 0.4) 70%, transparent 100%);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 10px 3px rgba(255, 0, 0, 0.6);
    display: none;
}

.laser-pointer.active {
    display: block;
}

body.laser-mode {
    cursor: none;
}

.laser-toggle {
    background: #666;
    padding: 12px 15px;
    font-size: 0.9rem;
}

.laser-toggle.active {
    background: #993333;
}

.laser-toggle.active:hover {
    background: #802929;
}

.fullscreen-toggle {
    background: #666;
    padding: 12px 15px;
    font-size: 0.9rem;
}

.fullscreen-toggle:hover {
    background: #555;
}

.highlight {
    color: #0066cc;
    font-weight: 600;
}

.badge {
    display: inline-block;
    background: #0066cc;
    color: white;
    padding: 10px 25px;
    border-radius: 4px;
    font-size: 1.6rem;
    margin: 10px 15px 10px 0;
    font-weight: 600;
}

/* Background image classes */
.bg-first-slide {
    background-image: url('presentation_first_slide.png');
}

.bg-default-slide {
    background-image: url('presentation_default_slide.png');
}

.image-demo {
    display: flex;
    gap: 30px;
    margin: 30px 0;
    flex-wrap: wrap;
    align-items: center;
}

.image-demo img {
    box-shadow: 0 4px 12px rgba(0,0,0,0.2);
    border: 3px solid #0066cc;
    border-radius: 8px;
}

/* Animation classes */
@keyframes fadeInUp {
    from { opacity: 0; transform: translateY(30px); }
    to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeInLeft {
    from { opacity: 0; transform: translateX(-30px); }
    to { opacity: 1; transform: translateX(0); }
}

@keyframes fadeInRight {
    from { opacity: 0; transform: translateX(30px); }
    to { opacity: 1; transform: translateX(0); }
}

@keyframes scaleIn {
    from { opacity: 0; transform: scale(0.8); }
    to { opacity: 1; transform: scale(1); }
}

@keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.05); }
}

@keyframes shimmer {
    0% { background-position: -200% center; }
    100% { background-position: 200% center; }
}

.slide.active .anim-fade-up { animation: fadeInUp 3s ease-out forwards; }
.slide.active .anim-fade-left { animation: fadeInLeft 3s ease-out forwards; }
.slide.active .anim-fade-right { animation: fadeInRight 3s ease-out forwards; }
.slide.active .anim-scale { animation: scaleIn 2.4s ease-out forwards; }
.slide.active .anim-pulse { animation: pulse 2.5s ease-in-out infinite; }

.anim-delay-1 { animation-delay: 0.6s !important; opacity: 0; }
.anim-delay-2 { animation-delay: 1.2s !important; opacity: 0; }
.anim-delay-3 { animation-delay: 1.8s !important; opacity: 0; }
.anim-delay-4 { animation-delay: 2.4s !important; opacity: 0; }
.anim-delay-5 { animation-delay: 3.0s !important; opacity: 0; }

.slide.active .anim-delay-1,
.slide.active .anim-delay-2,
.slide.active .anim-delay-3,
.slide.active .anim-delay-4,
.slide.active .anim-delay-5 { opacity: 1; }

.animation-demo-box {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 150px;
    height: 150px;
    background: linear-gradient(135deg, #0066cc, #004499);
    color: white;
    border-radius: 12px;
    font-size: 1.2rem;
    font-weight: 600;
    margin: 15px;
    box-shadow: 0 4px 15px rgba(0, 102, 204, 0.4);
}

.shimmer-text {
    background: linear-gradient(90deg, #0066cc 0%, #00aaff 50%, #0066cc 100%);
    background-size: 200% auto;
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: shimmer 3s linear infinite;
}

/* Two-column layout */
.two-column {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 50px;
    margin-top: 30px;
}

.two-column .column {
    display: flex;
    flex-direction: column;
}

.two-column .column h3 {
    margin-top: 0;
    border-bottom: 3px solid #0066cc;
    padding-bottom: 10px;
}

/* Quote/Callout styling */
.quote-block {
    background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
    border-left: 6px solid #0066cc;
    padding: 40px 50px;
    margin: 30px 0;
    position: relative;
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.quote-block::before {
    content: '"';
    position: absolute;
    top: 10px;
    left: 20px;
    font-size: 5rem;
    color: #0066cc;
    opacity: 0.3;
    font-family: Georgia, serif;
    line-height: 1;
}

.quote-block p {
    font-size: 2.4rem;
    font-style: italic;
    color: #333;
    margin-bottom: 15px;
}

.quote-block .attribution {
    font-size: 1.6rem;
    font-style: normal;
    color: #666;
    text-align: right;
}

.callout {
    background: #e7f3ff;
    border: 2px solid #0066cc;
    border-radius: 8px;
    padding: 30px 40px;
    margin: 30px 0;
}

.callout.warning {
    background: #fff3e0;
    border-color: #ff9800;
}

.callout.success {
    background: #e8f5e9;
    border-color: #4caf50;
}

.callout p { margin-bottom: 0; }

/* Code block styling */
.code-block {
    background: #1e1e1e;
    color: #d4d4d4;
    font-family: 'Consolas', 'Monaco', 'Courier New', monospace;
    font-size: 1.6rem;
    padding: 30px;
    border-radius: 8px;
    overflow-x: auto;
    margin: 30px 0;
    line-height: 1.6;
    box-shadow: 0 4px 12px rgba(0,0,0,0.3);
    white-space: pre;
}

.code-block::-webkit-scrollbar { height: 8px; }
.code-block::-webkit-scrollbar-track { background: #333; border-radius: 4px; }
.code-block::-webkit-scrollbar-thumb { background: #666; border-radius: 4px; }
.code-block::-webkit-scrollbar-thumb:hover { background: #888; }

.code-block .comment { color: #6a9955; }
.code-block .keyword { color: #569cd6; }
.code-block .string { color: #ce9178; }
.code-block .function { color: #dcdcaa; }
.code-block .number { color: #b5cea8; }
.code-block .tag { color: #569cd6; }
.code-block .attribute { color: #9cdcfe; }

/* Code scroll indicator (optional wrapper) */
.code-wrapper { position: relative; }
.code-wrapper::after {
    content: '→';
    position: absolute;
    right: 10px;
    bottom: 40px;
    color: #666;
    font-size: 1.2rem;
    opacity: 0;
    transition: opacity 0.3s;
    pointer-events: none;
}
.code-wrapper.has-overflow::after { opacity: 1; }

.code-header {
    background: #333;
    color: #888;
    padding: 10px 20px;
    font-size: 1.2rem;
    border-radius: 8px 8px 0 0;
    margin-bottom: -8px;
    font-family: 'Consolas', monospace;
}

/* Icon grid */
.icon-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 30px;
    margin: 30px 0;
}

.icon-item {
    display: flex;
    align-items: flex-start;
    gap: 20px;
    padding: 25px;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.icon-item:hover {
    transform: translateY(-3px);
    box-shadow: 0 4px 16px rgba(0,0,0,0.15);
}

.icon-item .icon {
    font-size: 3rem;
    flex-shrink: 0;
    width: 60px;
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #e7f3ff;
    border-radius: 12px;
}

.icon-item .icon svg {
    width: 32px;
    height: 32px;
    fill: #0066cc;
}

.icon-item .icon-content h4 {
    font-size: 1.8rem;
    color: #1a1a1a;
    margin-bottom: 8px;
    font-weight: 600;
}

.icon-item .icon-content p {
    font-size: 1.4rem;
    color: #666;
    margin: 0;
    line-height: 1.4;
}

/* Call-to-Action slide */
.cta-slide .slide-content {
    justify-content: center;
    align-items: center;
    text-align: center;
}

.cta-slide h2 {
    font-size: 4.5rem;
    border-bottom: none;
    margin-bottom: 30px;
}

.cta-slide .cta-subtitle {
    font-size: 2.4rem;
    color: #555;
    margin-bottom: 50px;
    max-width: 800px;
}

.cta-buttons {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
    justify-content: center;
}

.cta-button {
    display: inline-block;
    padding: 20px 50px;
    font-size: 1.8rem;
    font-weight: 600;
    border-radius: 8px;
    text-decoration: none;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.cta-button.primary {
    background: #0066cc;
    color: white;
    box-shadow: 0 4px 15px rgba(0, 102, 204, 0.4);
}

.cta-button.primary:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba(0, 102, 204, 0.5);
}

.cta-button.secondary {
    background: white;
    color: #0066cc;
    border: 3px solid #0066cc;
}

.cta-button.secondary:hover {
    transform: translateY(-3px);
    background: #f0f7ff;
}

.cta-contact {
    margin-top: 50px;
    font-size: 1.6rem;
    color: #666;
}

.cta-contact a {
    color: #0066cc;
    text-decoration: none;
    font-weight: 600;
}

/* Mobile responsive styles */
@media (max-width: 768px) {
    .slide { padding: 30px 40px; }
    .slide.title-slide .slide-content { padding-left: 20px; }
    .slide.title-slide h1 { font-size: 2.5rem; }
    .slide.title-slide .subtitle { font-size: 1.4rem; }
    h2 { font-size: 2rem; margin-bottom: 20px; }
    h3 { font-size: 1.4rem; }
    p, ul, ol { font-size: 1.3rem; line-height: 1.5; }
    ul li, ol li { margin-bottom: 10px; }
    table { font-size: 1rem; }
    th, td { padding: 10px; }
    .badge { font-size: 1rem; padding: 6px 12px; }
    .controls { bottom: 15px; right: 15px; gap: 8px; }
    button { padding: 10px 15px; font-size: 0.9rem; }
    .slide-number { bottom: 15px; left: 15px; font-size: 0.9rem; padding: 6px 12px; }
    .chart-container { padding: 15px; }
    .image-demo img { max-width: 100%; height: auto; }
    .two-column { grid-template-columns: 1fr; gap: 30px; }
    .quote-block { padding: 25px 30px; }
    .quote-block p { font-size: 1.4rem; }
    .quote-block .attribution { font-size: 1.1rem; }
    .code-block { font-size: 1rem; padding: 15px; }
    .icon-grid { grid-template-columns: 1fr; }
    .icon-item .icon-content h4 { font-size: 1.3rem; }
    .icon-item .icon-content p { font-size: 1.1rem; }
    .cta-slide h2 { font-size: 2.5rem; }
    .cta-slide .cta-subtitle { font-size: 1.4rem; }
    .cta-button { padding: 15px 30px; font-size: 1.2rem; }
}

/* Print styles for PDF export */
@media print {
    @page { size: landscape; margin: 0; }
    body { background: white; padding: 0; margin: 0; }
    .slide-container { width: 100%; height: auto; border: none; border-radius: 0; box-shadow: none; overflow: visible; }
    .slide {
        position: relative !important;
        transform: none !important;
        opacity: 1 !important;
        page-break-after: always;
        page-break-inside: avoid;
        height: 100vh;
        width: 100vw;
        display: flex !important;
        break-after: page;
    }
    .slide.bg-default-slide { background-image: none !important; background-color: white !important; }
    .slide.bg-first-slide { background-size: cover !important; background-position: center !important; -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; }
    .slide:last-child { page-break-after: auto; }
    .slide.prev { transform: none !important; }
    .controls, .slide-number, .progress-bar, .laser-pointer { display: none !important; }
    .slide { -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; color-adjust: exact !important; }
    .anim-fade-up, .anim-fade-left, .anim-fade-right, .anim-scale, .anim-delay-1, .anim-delay-2, .anim-delay-3, .anim-delay-4, .anim-delay-5 {
        opacity: 1 !important; transform: none !important; animation: none !important;
    }
    .slide-content { padding: 40px 60px; }
    .chart-container canvas { max-width: 100%; }
    .cta-button { border: 2px solid #0066cc !important; }
}
```
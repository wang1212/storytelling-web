# Scrolling Mode Workflow

Create immersive, scroll-driven narrative web experiences with progressive content revelation.

## When to Use Scrolling Mode

- **Long-form narratives** — Stories that unfold as the reader scrolls
- **Exploratory content** — Readers control their own pace and path
- **Editorial experiences** — Magazine-style articles, journalism, essays
- **Journey-based content** — Travel stories, process explanations, timelines
- **Emotional storytelling** — Building atmosphere and immersion gradually

## Core Philosophy

1. **Zero Dependencies** — Single HTML files with inline CSS/JS. No npm, no build tools.
2. **Scroll-Driven Narrative** — Content reveals itself as users scroll, creating an exploratory journey.
3. **Distinctive Design** — Avoid generic aesthetics. Every story should feel custom-crafted.
4. **Production Quality** — Code should be well-commented, accessible, and performant.

## Narrative Structure

### Analyzing Content Structure

When working with the user's content, analyze and identify:

1. **Opening Hook** — How does the story begin? (dramatic moment, question, scene)
2. **Story Arc** — What is the progression? (setup → conflict → resolution, or other structure)
3. **Key Moments** — What are the pivotal sections that need visual emphasis?
4. **Pacing** — Where should the reader slow down vs. move quickly?
5. **Ending** — How does it conclude? (reflection, call-to-action, open question)

## Generate Story

### HTML Architecture

**Note on Fonts:** Use Google Fonts for typography. Include the preconnect links and font stylesheet in the `<head>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Your+Selected+Fonts&display=swap" rel="stylesheet">
```

Choose distinctive font pairings (e.g., display fonts for headings, readable fonts for body text).

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Story Title]</title>
  <!-- Add Google Fonts here -->
  <style>
    /* ========================================
       CSS CUSTOM PROPERTIES (THEME)
       ======================================== */
    :root {
      /* Colors */
      --bg-primary: #0a0f1c;
      --bg-secondary: #111827;
      --text-primary: #ffffff;
      --text-secondary: #9ca3af;
      --accent: #00ffcc;
      --accent-glow: rgba(0, 255, 204, 0.3);

      /* Typography - update with your chosen Google Fonts */
      --font-display: 'Your-Display-Font', system-ui, sans-serif;
      --font-body: 'Your-Body-Font', system-ui, sans-serif;

      /* Spacing */
      --section-padding: clamp(4rem, 10vh, 8rem);
      --content-max-width: 720px;

      /* Animation */
      --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
      --duration-normal: 0.6s;
    }

    /* ========================================
       RESET & BASE
       ======================================== */
    *, *::before, *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      font-family: var(--font-body);
      background: var(--bg-primary);
      color: var(--text-primary);
      line-height: 1.7;
      overflow-x: hidden;
    }

    /* ========================================
       PROGRESS INDICATOR
       ======================================== */
    .progress-bar {
      position: fixed;
      top: 0;
      left: 0;
      height: 3px;
      background: var(--accent);
      z-index: 1000;
      transition: width 0.1s linear;
    }

    /* ========================================
       SECTIONS
       ======================================== */
    .section {
      min-height: 100vh;
      padding: var(--section-padding) 2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
    }

    .section-content {
      max-width: var(--content-max-width);
      width: 100%;
    }

    /* ========================================
       SCROLL ANIMATIONS
       ======================================== */
    .reveal {
      opacity: 0;
      transform: translateY(30px);
      transition: opacity var(--duration-normal) var(--ease-out-expo),
                  transform var(--duration-normal) var(--ease-out-expo);
    }

    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }

    .reveal-delay-1 { transition-delay: 0.1s; }
    .reveal-delay-2 { transition-delay: 0.2s; }
    .reveal-delay-3 { transition-delay: 0.3s; }

    /* ========================================
       TYPOGRAPHY
       ======================================== */
    h1, h2, h3 {
      font-family: var(--font-display);
      font-weight: 600;
      line-height: 1.2;
      margin-bottom: 1.5rem;
    }

    h1 { font-size: clamp(2.5rem, 5vw, 4rem); }
    h2 { font-size: clamp(1.8rem, 3vw, 2.5rem); }
    h3 { font-size: clamp(1.3rem, 2vw, 1.8rem); }

    p {
      margin-bottom: 1.5rem;
      font-size: clamp(1rem, 1.2vw, 1.125rem);
      color: var(--text-secondary);
    }

    /* ========================================
       SPECIAL ELEMENTS
       ======================================== */
    .hero {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      position: relative;
      overflow: hidden;
    }

    .hero-title {
      font-size: clamp(3rem, 8vw, 6rem);
      margin-bottom: 1rem;
    }

    .hero-subtitle {
      font-size: clamp(1.2rem, 2vw, 1.5rem);
      color: var(--text-secondary);
      max-width: 600px;
    }

    .scroll-indicator {
      position: absolute;
      bottom: 2rem;
      left: 50%;
      transform: translateX(-50%);
      animation: bounce 2s infinite;
    }

    @keyframes bounce {
      0%, 20%, 50%, 80%, 100% { transform: translateX(-50%) translateY(0); }
      40% { transform: translateX(-50%) translateY(-10px); }
      60% { transform: translateX(-50%) translateY(-5px); }
    }

    .pull-quote {
      font-family: var(--font-display);
      font-size: clamp(1.5rem, 3vw, 2rem);
      font-style: italic;
      text-align: center;
      padding: 3rem 2rem;
      margin: 3rem 0;
      border-left: 4px solid var(--accent);
      background: var(--bg-secondary);
    }

    .chapter-marker {
      position: absolute;
      top: 2rem;
      left: 2rem;
      font-family: var(--font-display);
      font-size: 0.875rem;
      text-transform: uppercase;
      letter-spacing: 0.2em;
      color: var(--accent);
    }

    .image-full {
      width: 100vw;
      height: 100vh;
      object-fit: cover;
      position: absolute;
      top: 0;
      left: 0;
      z-index: -1;
    }

    .image-overlay {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: linear-gradient(to bottom, rgba(0,0,0,0.3), rgba(0,0,0,0.7));
      z-index: -1;
    }
  </style>
</head>
<body>
  <!-- Progress Bar -->
  <div class="progress-bar" id="progressBar"></div>

  <!-- Hero Section -->
  <section class="hero" id="hero">
    <div class="reveal">
      <h1 class="hero-title">[Story Title]</h1>
      <p class="hero-subtitle">[Subtitle or opening hook]</p>
    </div>
    <div class="scroll-indicator">
      <svg width="30" height="30" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M12 5v14M19 12l-7 7-7-7"/>
      </svg>
    </div>
  </section>

  <!-- Story Sections -->
  <section class="section" id="section-1">
    <div class="section-content">
      <span class="chapter-marker reveal">01</span>
      <h2 class="reveal reveal-delay-1">[Section Title]</h2>
      <p class="reveal reveal-delay-2">[Content paragraph]</p>
      <p class="reveal reveal-delay-3">[Content paragraph]</p>
    </div>
  </section>

  <!-- Pull Quote Section -->
  <section class="section" id="quote">
    <blockquote class="pull-quote reveal">
      "[Impactful quote or key moment from the story]"
    </blockquote>
  </section>

  <!-- Continue with more sections... -->

  <script>
    // ========================================
    // SCROLL OBSERVER FOR REVEAL ANIMATIONS
    // ========================================
    const observerOptions = {
      root: null,
      rootMargin: '0px',
      threshold: 0.15
    };

    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
        }
      });
    }, observerOptions);

    document.querySelectorAll('.reveal').forEach(el => {
      observer.observe(el);
    });

    // ========================================
    // PROGRESS BAR
    // ========================================
    function updateProgress() {
      const scrollTop = window.scrollY;
      const docHeight = document.documentElement.scrollHeight - window.innerHeight;
      const progress = (scrollTop / docHeight) * 100;
      document.getElementById('progressBar').style.width = progress + '%';
    }

    window.addEventListener('scroll', updateProgress);
    updateProgress();
  </script>
</body>
</html>
```

### Section Types

Use these section types to create narrative variety:

1. **Hero** — Full-screen opening with title and hook
2. **Text-Only** — Clean reading experience
3. **Image-Text** — Side-by-side or overlaid image with text
4. **Full-Bleed Image** — Image fills screen with overlaid text
5. **Pull Quote** — Standalone impactful quote
6. **Transition** — Short section to shift narrative gears

### Animation Patterns

**Standard Reveal:**

```css
.reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}
```

**Staggered Reveal:**

- Add delay classes: `.reveal-delay-1`, `.reveal-delay-2`, `.reveal-delay-3`
- Creates cascading animation effect

**Parallax (optional):**

```javascript
window.addEventListener('scroll', () => {
  const scrolled = window.pageYOffset;
  const parallax = document.querySelector('.parallax');
  parallax.style.transform = `translateY(${scrolled * 0.5}px)`;
});
```

### Design Principles

1. **Typography Hierarchy** — Clear distinction between headings, body, and captions
2. **Breathing Room** — Generous whitespace between sections
3. **Visual Rhythm** — Alternate between text-heavy and visual sections
4. **Color Consistency** — Use CSS custom properties for easy theming
5. **Accessibility** — Ensure sufficient contrast, semantic HTML, keyboard navigation

## Tips for Great Stories

1. **Start with a hook** — The first 3 seconds matter
2. **Control pacing** — Some sections should be quick, others demand pause
3. **Use images intentionally** — Every image should serve the narrative
4. **Create moments** — Build up to key revelations or emotional beats
5. **End memorably** — The conclusion should resonate
6. **Test on mobile** — Many readers will experience the story on phones

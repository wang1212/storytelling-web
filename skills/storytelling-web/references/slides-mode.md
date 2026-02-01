# Slides Mode Workflow

Create stunning, animation-rich HTML presentations with slide-based navigation.

## When to Use Slides Mode

- **Presentations** — Pitch decks, conference talks, tutorials
- **Structured content** — Step-by-step explanations, sequential information
- **Speaker-led experiences** — Content meant to be presented by a speaker
- **Comparative content** — Side-by-side comparisons, before/after

## Core Philosophy

1. **Zero Dependencies** — Single HTML files with inline CSS/JS. No npm, no build tools.
2. **Show, Don't Tell** — People don't know what they want until they see it. Generate visual previews, not abstract choices.
3. **Distinctive Design** — Avoid generic "AI slop" aesthetics. Every presentation should feel custom-crafted.
4. **Production Quality** — Code should be well-commented, accessible, and performant.

## Generate Presentation

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
  <title>[Presentation Title]</title>
  <!-- Add Google Fonts here -->
  <style>
    :root {
      /* Colors */
      --bg-primary: #0a0f1c;
      --bg-secondary: #111827;
      --text-primary: #ffffff;
      --text-secondary: #9ca3af;
      --accent: #00ffcc;

      /* Typography - update with your chosen Google Fonts */
      --font-display: 'Your-Display-Font', system-ui, sans-serif;
      --font-body: 'Your-Body-Font', system-ui, sans-serif;

      /* Slide dimensions */
      --slide-width: 1280px;
      --slide-height: 720px;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: var(--font-body);
      background: #000;
      color: var(--text-primary);
      overflow: hidden;
    }

    /* Slide container */
    .presentation {
      width: 100vw;
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .slide {
      width: var(--slide-width);
      height: var(--slide-height);
      background: var(--bg-primary);
      padding: 4rem;
      position: relative;
      display: none;
    }

    .slide.active {
      display: flex;
      flex-direction: column;
    }

    /* Navigation */
    .nav {
      position: fixed;
      bottom: 2rem;
      right: 2rem;
      display: flex;
      gap: 1rem;
      z-index: 100;
    }

    .nav button {
      background: var(--bg-secondary);
      border: 1px solid var(--accent);
      color: var(--text-primary);
      padding: 0.5rem 1rem;
      cursor: pointer;
      font-family: var(--font-body);
    }

    /* Progress */
    .progress {
      position: fixed;
      bottom: 0;
      left: 0;
      height: 4px;
      background: var(--accent);
      transition: width 0.3s ease;
    }

    /* Typography */
    h1 {
      font-family: var(--font-display);
      font-size: 3.5rem;
      font-weight: 600;
      margin-bottom: 1.5rem;
    }

    h2 {
      font-family: var(--font-display);
      font-size: 2.5rem;
      font-weight: 600;
      margin-bottom: 1rem;
    }

    p {
      font-size: 1.25rem;
      line-height: 1.6;
      color: var(--text-secondary);
    }

    /* Slide types */
    .slide-title {
      justify-content: center;
      align-items: center;
      text-align: center;
    }

    .slide-content {
      justify-content: center;
    }

    .slide-split {
      flex-direction: row;
      align-items: center;
      gap: 4rem;
    }

    .slide-split > div {
      flex: 1;
    }

    /* Animations */
    @keyframes fadeInUp {
      from {
        opacity: 0;
        transform: translateY(30px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .animate-in {
      animation: fadeInUp 0.6s ease-out forwards;
    }

    .delay-1 { animation-delay: 0.1s; }
    .delay-2 { animation-delay: 0.2s; }
    .delay-3 { animation-delay: 0.3s; }
  </style>
</head>
<body>
  <div class="presentation">
    <!-- Slide 1: Title -->
    <div class="slide slide-title active" data-slide="1">
      <h1 class="animate-in">[Title]</h1>
      <p class="animate-in delay-1">[Subtitle]</p>
    </div>

    <!-- Slide 2: Content -->
    <div class="slide slide-content" data-slide="2">
      <h2 class="animate-in">[Section Title]</h2>
      <p class="animate-in delay-1">[Content]</p>
    </div>

    <!-- Add more slides... -->
  </div>

  <!-- Navigation -->
  <div class="nav">
    <button onclick="prevSlide()">← Previous</button>
    <button onclick="nextSlide()">Next →</button>
  </div>

  <!-- Progress bar -->
  <div class="progress" id="progress"></div>

  <script>
    let currentSlide = 1;
    const totalSlides = document.querySelectorAll('.slide').length;

    function showSlide(n) {
      const slides = document.querySelectorAll('.slide');

      if (n > totalSlides) currentSlide = 1;
      if (n < 1) currentSlide = totalSlides;

      slides.forEach(slide => slide.classList.remove('active'));
      slides[currentSlide - 1].classList.add('active');

      // Update progress
      document.getElementById('progress').style.width =
        (currentSlide / totalSlides * 100) + '%';

      // Re-trigger animations
      const animated = slides[currentSlide - 1].querySelectorAll('.animate-in');
      animated.forEach(el => {
        el.style.animation = 'none';
        el.offsetHeight; // Trigger reflow
        el.style.animation = null;
      });
    }

    function nextSlide() {
      currentSlide++;
      showSlide(currentSlide);
    }

    function prevSlide() {
      currentSlide--;
      showSlide(currentSlide);
    }

    // Keyboard navigation
    document.addEventListener('keydown', (e) => {
      if (e.key === 'ArrowRight' || e.key === ' ') nextSlide();
      if (e.key === 'ArrowLeft') prevSlide();
    });

    // Initialize
    showSlide(currentSlide);
  </script>
</body>
</html>
```

### Slide Types

1. **Title Slide** — Big headline, subtitle, presenter name
2. **Content Slide** — Title + bullet points or paragraph
3. **Split Slide** — Two columns (text + image, or text + text)
4. **Full Image** — Image fills slide with overlaid text
5. **Quote Slide** — Standalone quote or key message
6. **Closing Slide** — Thank you, contact info, call-to-action

### Design Principles

- **16:9 aspect ratio** — Standard presentation format
- **Large typography** — Readable from a distance
- **High contrast** — Clear visibility in various lighting
- **Consistent spacing** — Professional, polished look
- **Animation restraint** — Smooth, purposeful transitions

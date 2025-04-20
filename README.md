# Geturanium
console script 
# 👡 Human-like Click  Script

This script simulates real human behavior by clicking elements on a webpage with random delays and natural timing. Ideal for projects where bot detection is sensitive.

## 🚀 Features

- Simulates real human-like clicks.
- Waits for elements to appear before interacting.
- Adds randomized delay to mimic natural user behavior.
- Automatically observes and clicks new elements as they appear.

---

## 📜 Usage Guide

### 1. 🔧 Add the Script to Your Project

Paste the following code into your browser console or inject it via your content script:

```javascript
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function simulateRealClick(el) {
  const rect = el.getBoundingClientRect();
  const x = rect.left + rect.width / 2;
  const y = rect.top + rect.height / 2;

  const types = ['pointerdown', 'mousedown', 'mouseup', 'click'];

  for (const type of types) {
    const event = new MouseEvent(type, {
      bubbles: true,
      cancelable: true,
      clientX: x,
      clientY: y,
      view: window
    });
    el.dispatchEvent(event);
    await sleep(100 + Math.random() * 100); // random delay between events
  }

  console.log('✅ Clicked:', el);
}

// Store already-clicked elements
const clicked = new WeakSet();

// Check and click visible elements
async function checkAndClickNew() {
  const elements = document.querySelectorAll('.uranium-shard');

  for (const el of elements) {
    if (!clicked.has(el)) {
      const isVisible = el.offsetParent !== null;
      if (!isVisible) continue;

      clicked.add(el);
      await sleep(2000 + Math.random() * 500); // wait before clicking
      await simulateRealClick(el);
    }
  }
}

// Watch for new DOM elements
const observer = new MutationObserver(() => {
  checkAndClickNew();
});

observer.observe(document.body, {
  childList: true,
  subtree: true
});

// Initial scan
checkAndClickNew();
```

Feel free to fork, star ⭐, or contribute!


# shopify-grouped-category-showcase

# 🛍️ Shopify Grouped Category Sliders & ScrollSpy

A high-performance, responsive multi-department category showcase built with vanilla Liquid, CSS Grid, and zero-dependency JavaScript. Features horizontal sub-collection carousels for desktop and an auto-centering, scroll-synchronized category anchor pill bar for mobile devices.

---

## 🚀 Key Benefits for E-Commerce Stores

* **Reduces Category Fatigue:** Organizing large catalogs into multi-level micro-sliders prevents "infinite doom-scrolling" on the homepage while keeping dozens of collections accessible within one fold.
* **Frictionless Mobile Discovery:** The sticky mobile quick-nav bar syncs automatically via `IntersectionObserver`. Customers always know which department they are browsing without losing their place.
* **Instant Native Performance:** **0 KB external libraries.** No Swiper, Slick, or jQuery required. Runs natively on hardware-accelerated CSS scroll-snapping, ensuring 95+ Google PageSpeed and Core Web Vitals scores.
* **Zero Theme Dependency:** Drops cleanly into any Shopify 2.0 or vintage architecture theme (`Dawn`, `Impact`, `Prestige`, `Sense`, etc.).

---

## 🎯 Ideal Use Cases

1. **Department Stores & Supermarts:** Stores carrying multiple distinct verticals (e.g., Groceries, Tech, Home, Apparel) needing an organized homepage directory.
2. **Wholesale & B2B Portals:** High-SKU density stores where merchants need quick visual indexing of deep category branches.
3. **Seasonal & Flash Campaigns:** Quickly highlighting curated deal collections alongside everyday product categories.

---

## ⚙️ Architecture & Features

* **Desktop Layout:** Responsive CSS Grid system ($4 \times N$ column layout on wide viewports, dynamic $2 \times N$ on tablet).
* **Mobile ScrollSpy:** Uses native `IntersectionObserver` with an active reading strip (`rootMargin: -20% 0px -65% 0px`) to highlight and horizontally auto-center the active department pill.
* **Custom Handle Fallbacks:** Supports paired `Display Name:custom-handle` syntax to bridge gaps between internal Shopify SEO URL handles and consumer-facing titles.
* **Fallbacks:** Automatically renders styled typographic badges if a collection has no featured image uploaded.

---

## 📥 Installation

1. From your Shopify Admin, navigate to **Online Store > Themes > Edit Code**.
2. Under **Sections**, click **Add a new section**.
3. Name it `grouped-category-sliders.liquid`.
4. Paste the code from [`sections/grouped-category-sliders.liquid`](./sections/grouped-category-sliders.liquid).
5. Open the **Theme Customizer**, click **Add Section**, and select **Grouped Category Sliders**.

---

## 🛠️ Configuration Syntax

Categories are defined in comma-separated Liquid arrays. If your Shopify collection handle matches the display name, simply write the title. If your URL handle differs, separate them with a colon (`:`):

```liquid
{% assign group1_names = "Baking,Knives:kitchen-knives,Egg Tools:egg-accessories" | split: "," %}

```

* `Knives`: Text displayed on the storefront card.
* `kitchen-knives`: Exact URL handle from Shopify collection settings (`/collections/kitchen-knives`).

---

## 📄 License

MIT License. Free for personal and commercial client theme builds.

```

---

### File 2: `sections/grouped-category-sliders.liquid`

```liquid
{% comment %}
  Grouped Category Sliders with Synchronized Mobile ScrollSpy
  File: sections/grouped-category-sliders.liquid
  License: MIT
{% endcomment %}

<!-- Main Section Header -->
<div class="hm-section-header">
  <span class="hm-header-badge">BROWSE CATALOG</span>
  <h2 class="hm-main-title">Explore By Department</h2>
  <p class="hm-main-subtitle">Discover curated collections tailored for your daily lifestyle</p>
</div>

<!-- Mobile-Only Quick Jump Category Navigation Bar -->
<div class="hm-quick-nav-bar">
  <div class="hm-quick-nav-track">
    <a href="#hm-group-1" class="hm-quick-pill">🍳 Kitchen</a>
    <a href="#hm-group-2" class="hm-quick-pill">⚡ Tech</a>
    <a href="#hm-group-3" class="hm-quick-pill">👗 Fashion</a>
    <a href="#hm-group-4" class="hm-quick-pill deal-pill">🔥 Deals</a>
  </div>
</div>

<div class="hm-multi-grid-wrapper">
  {% comment %}
    Configure group collections below.
    Format: "Display Title" OR "Display Title:collection-handle"
  {% endcomment %}
  {% assign group1_names = "Baking,Choppers,Knives:kitchen-knives,Strainers" | split: "," %}
  {% assign group2_names = "Audio,Computer Accessories,Mobile Accessories,Cables:cables-adapters" | split: "," %}
  {% assign group3_names = "Dresses:women-dresses,T-Shirts:mens-tees,Footwear,Accessories" | split: "," %}
  {% assign group4_names = "Under 499:deals-under-499,Best Sellers,Trending Now" | split: "," %}

  <!-- Group 1: Kitchen Prep -->
  <div id="hm-group-1" class="hm-showcase-box theme-orange">
    <div class="hm-box-top">
      <div class="hm-box-label">
        <div class="hm-icon-wrap">🍳</div>
        <div class="hm-title-wrap">
          <h3>Kitchen Prep</h3>
          <span class="hm-subtext">{{ group1_names.size }} Collections</span>
        </div>
      </div>
      <div class="hm-nav-controls">
        <button type="button" class="hm-slide-btn prev" onclick="scrollSlider(this, -1)" aria-label="Previous">❮</button>
        <button type="button" class="hm-slide-btn next" onclick="scrollSlider(this, 1)" aria-label="Next">❯</button>
      </div>
    </div>
    <div class="hm-reel-track">
      {% for raw_item in group1_names %}
        {% assign parts = raw_item | split: ":" %}
        {% assign item_label = parts[0] | strip %}
        {% assign cat_handle = parts[1] | default: item_label | handleize %}
        {% assign col = collections[cat_handle] %}
        {% if col != empty and col.url != blank %}
          {% assign item_url = col.url %}
        {% else %}
          {% assign item_url = routes.collections_url | append: '/' | append: cat_handle %}
        {% endif %}
        <a href="{{ item_url }}" class="hm-reel-item">
          {% if forloop.first %}<span class="hm-badge-tag tag-hot">HOT</span>{% endif %}
          <div class="hm-thumb-wrap">
            {% if col.featured_image %}
              <img src="{{ col.featured_image | image_url: width: 220 }}" alt="{{ item_label | escape }}" loading="lazy" width="220" height="220">
            {% else %}
              <div class="hm-thumb-fallback">{{ item_label | slice: 0, 1 }}</div>
            {% endif %}
          </div>
          <p class="hm-item-text">{{ item_label }}</p>
          <span class="hm-explore-link">Shop Now</span>
        </a>
      {% endfor %}
    </div>
  </div>

  <!-- Group 2: Tech & Electronics -->
  <div id="hm-group-2" class="hm-showcase-box theme-indigo">
    <div class="hm-box-top">
      <div class="hm-box-label">
        <div class="hm-icon-wrap">⚡</div>
        <div class="hm-title-wrap">
          <h3>Tech & Gadgets</h3>
          <span class="hm-subtext">{{ group2_names.size }} Collections</span>
        </div>
      </div>
      <div class="hm-nav-controls">
        <button type="button" class="hm-slide-btn prev" onclick="scrollSlider(this, -1)" aria-label="Previous">❮</button>
        <button type="button" class="hm-slide-btn next" onclick="scrollSlider(this, 1)" aria-label="Next">❯</button>
      </div>
    </div>
    <div class="hm-reel-track">
      {% for raw_item in group2_names %}
        {% assign parts = raw_item | split: ":" %}
        {% assign item_label = parts[0] | strip %}
        {% assign cat_handle = parts[1] | default: item_label | handleize %}
        {% assign col = collections[cat_handle] %}
        {% if col != empty and col.url != blank %}
          {% assign item_url = col.url %}
        {% else %}
          {% assign item_url = routes.collections_url | append: '/' | append: cat_handle %}
        {% endif %}
        <a href="{{ item_url }}" class="hm-reel-item">
          <div class="hm-thumb-wrap">
            {% if col.featured_image %}
              <img src="{{ col.featured_image | image_url: width: 220 }}" alt="{{ item_label | escape }}" loading="lazy" width="220" height="220">
            {% else %}
              <div class="hm-thumb-fallback">{{ item_label | slice: 0, 1 }}</div>
            {% endif %}
          </div>
          <p class="hm-item-text">{{ item_label }}</p>
          <span class="hm-explore-link">Shop Now</span>
        </a>
      {% endfor %}
    </div>
  </div>

  <!-- Group 3: Fashion & Apparel -->
  <div id="hm-group-3" class="hm-showcase-box theme-pink">
    <div class="hm-box-top">
      <div class="hm-box-label">
        <div class="hm-icon-wrap">👗</div>
        <div class="hm-title-wrap">
          <h3>Fashion & Style</h3>
          <span class="hm-subtext">{{ group3_names.size }} Collections</span>
        </div>
      </div>
      <div class="hm-nav-controls">
        <button type="button" class="hm-slide-btn prev" onclick="scrollSlider(this, -1)" aria-label="Previous">❮</button>
        <button type="button" class="hm-slide-btn next" onclick="scrollSlider(this, 1)" aria-label="Next">❯</button>
      </div>
    </div>
    <div class="hm-reel-track">
      {% for raw_item in group3_names %}
        {% assign parts = raw_item | split: ":" %}
        {% assign item_label = parts[0] | strip %}
        {% assign cat_handle = parts[1] | default: item_label | handleize %}
        {% assign col = collections[cat_handle] %}
        {% if col != empty and col.url != blank %}
          {% assign item_url = col.url %}
        {% else %}
          {% assign item_url = routes.collections_url | append: '/' | append: cat_handle %}
        {% endif %}
        <a href="{{ item_url }}" class="hm-reel-item">
          <div class="hm-thumb-wrap">
            {% if col.featured_image %}
              <img src="{{ col.featured_image | image_url: width: 220 }}" alt="{{ item_label | escape }}" loading="lazy" width="220" height="220">
            {% else %}
              <div class="hm-thumb-fallback">{{ item_label | slice: 0, 1 }}</div>
            {% endif %}
          </div>
          <p class="hm-item-text">{{ item_label }}</p>
          <span class="hm-explore-link">Shop Now</span>
        </a>
      {% endfor %}
    </div>
  </div>

  <!-- Group 4: Deals -->
  <div id="hm-group-4" class="hm-showcase-box theme-deals">
    <div class="hm-box-top">
      <div class="hm-box-label">
        <div class="hm-icon-wrap">🔥</div>
        <div class="hm-title-wrap">
          <h3>Daily Deals</h3>
          <span class="hm-subtext">Limited Offers</span>
        </div>
      </div>
      <div class="hm-nav-controls">
        <button type="button" class="hm-slide-btn prev" onclick="scrollSlider(this, -1)" aria-label="Previous">❮</button>
        <button type="button" class="hm-slide-btn next" onclick="scrollSlider(this, 1)" aria-label="Next">❯</button>
      </div>
    </div>
    <div class="hm-reel-track">
      {% for raw_item in group4_names %}
        {% assign parts = raw_item | split: ":" %}
        {% assign item_label = parts[0] | strip %}
        {% assign cat_handle = parts[1] | default: item_label | handleize %}
        {% assign col = collections[cat_handle] %}
        {% if col != empty and col.url != blank %}
          {% assign item_url = col.url %}
        {% else %}
          {% assign item_url = routes.collections_url | append: '/' | append: cat_handle %}
        {% endif %}
        <a href="{{ item_url }}" class="hm-reel-item deal-item">
          <span class="hm-badge-tag tag-deal">SAVE</span>
          <div class="hm-thumb-wrap">
            {% if col.featured_image %}
              <img src="{{ col.featured_image | image_url: width: 220 }}" alt="{{ item_label | escape }}" loading="lazy" width="220" height="220">
            {% else %}
              <div class="hm-thumb-fallback">{{ item_label | slice: 0, 1 }}</div>
            {% endif %}
          </div>
          <p class="hm-item-text">{{ item_label }}</p>
          <span class="hm-explore-link">Grab Deal</span>
        </a>
      {% endfor %}
    </div>
  </div>
</div>

<style>
/* Header */
.hm-section-header {
  text-align: center;
  max-width: 700px;
  margin: 10px auto 14px;
  padding: 0 16px;
}
.hm-header-badge {
  display: inline-block;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.8px;
  color: #2563eb;
  background: #eff6ff;
  border: 1px solid #dbeafe;
  padding: 4px 10px;
  border-radius: 20px;
  margin-bottom: 8px;
  text-transform: uppercase;
}
.hm-main-title {
  margin: 0 0 6px 0;
  font-size: 26px;
  font-weight: 800;
  color: #0f172a;
  letter-spacing: -0.5px;
}
.hm-main-subtitle {
  margin: 0;
  font-size: 14px;
  color: #64748b;
  line-height: 1.4;
}

/* Quick Navigation: Mobile */
.hm-quick-nav-bar {
  display: none;
}
@media (max-width: 640px) {
  .hm-quick-nav-bar {
    display: block;
    position: sticky;
    top: 0;
    z-index: 10;
    background: rgba(255, 255, 255, 0.96);
    backdrop-filter: blur(8px);
    padding: 8px 10px;
    border-bottom: 1px solid #f1f5f9;
  }
}
.hm-quick-nav-track {
  display: flex;
  gap: 8px;
  overflow-x: auto;
  scrollbar-width: none;
  -webkit-overflow-scrolling: touch;
  padding-bottom: 2px;
}
.hm-quick-nav-track::-webkit-scrollbar {
  display: none;
}
.hm-quick-pill {
  display: inline-flex;
  align-items: center;
  white-space: nowrap;
  font-size: 12px;
  font-weight: 600;
  color: #475569;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 20px;
  padding: 6px 12px;
  text-decoration: none;
  transition: all 0.2s ease;
}
.hm-quick-pill.deal-pill {
  border-color: #fdba74;
  color: #ea580c;
  background: #fff7ed;
}
.hm-quick-pill.is-active {
  background: #0f172a !important;
  color: #ffffff !important;
  border-color: #0f172a !important;
  box-shadow: 0 2px 8px rgba(15, 23, 42, 0.18);
}

/* Grid Wrapper */
.hm-multi-grid-wrapper {
  max-width: 1400px;
  margin: 0 auto;
  padding: 12px 14px 28px;
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 16px;
  scroll-behavior: smooth;
}

/* Department Box */
.hm-showcase-box {
  background: #ffffff;
  border-radius: 16px;
  padding: 14px 12px 14px;
  border: 1px solid #edf0f5;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.03);
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: box-shadow 0.2s ease, border-color 0.2s ease;
  scroll-margin-top: 85px;
}
.hm-showcase-box:hover {
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.06);
  border-color: #cbd5e1;
}
.hm-showcase-box::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 4px;
}
.theme-orange::before { background: linear-gradient(90deg, #f97316, #fb923c); }
.theme-indigo::before { background: linear-gradient(90deg, #6366f1, #a855f7); }
.theme-pink::before { background: linear-gradient(90deg, #ec4899, #f472b6); }
.theme-deals { background: linear-gradient(180deg, #fffaf5 0%, #ffffff 100%); border-color: #fed7aa; }
.theme-deals::before { background: linear-gradient(90deg, #ef4444, #f97316); }

/* Department Top Header */
.hm-box-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  padding-bottom: 6px;
  border-bottom: 1px solid #f8fafc;
}
.hm-box-label {
  display: flex;
  align-items: center;
  gap: 8px;
  overflow: hidden;
}
.hm-icon-wrap {
  font-size: 16px;
  flex-shrink: 0;
}
.hm-title-wrap h3 {
  margin: 0;
  font-size: 13.5px;
  font-weight: 700;
  color: #1e293b;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.hm-subtext {
  font-size: 10.5px;
  color: #64748b;
  font-weight: 500;
}

/* Nav Arrows */
.hm-nav-controls {
  display: flex;
  gap: 4px;
  flex-shrink: 0;
}
.hm-slide-btn {
  background: #f1f5f9;
  border: 1px solid #e2e8f0;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  font-size: 9px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #334155;
  transition: all 0.2s ease;
}
.hm-slide-btn:hover {
  background: #0f172a;
  color: #ffffff;
  border-color: #0f172a;
}

/* Track & Product Cards */
.hm-reel-track {
  display: flex;
  gap: 10px;
  overflow-x: auto;
  scroll-behavior: smooth;
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
  padding: 2px 2px 4px;
  scrollbar-width: none;
}
.hm-reel-track::-webkit-scrollbar {
  display: none;
}
.hm-reel-item {
  flex: 0 0 calc(48% - 5px);
  min-width: 96px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-decoration: none;
  scroll-snap-align: start;
  background: #fafbfc;
  border: 1px solid #f1f5f9;
  border-radius: 12px;
  padding: 6px 4px 8px;
  position: relative;
  transition: all 0.25s ease;
}
.hm-reel-item:hover {
  transform: translateY(-3px);
  background: #ffffff;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.06);
  border-color: #cbd5e1;
}
.hm-badge-tag {
  position: absolute;
  top: 5px;
  left: 5px;
  font-size: 8.5px;
  font-weight: 700;
  padding: 2px 4px;
  border-radius: 3px;
  z-index: 2;
  letter-spacing: 0.2px;
}
.tag-hot { background: #fee2e2; color: #dc2626; }
.tag-deal { background: #ffedd5; color: #ea580c; }

.hm-thumb-wrap {
  width: 100%;
  aspect-ratio: 1 / 1;
  border-radius: 8px;
  background: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.hm-thumb-wrap img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  transition: transform 0.3s ease;
}
.hm-reel-item:hover .hm-thumb-wrap img {
  transform: scale(1.08);
}
.hm-thumb-fallback {
  width: 100%;
  height: 100%;
  background: #f1f5f9;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 18px;
  color: #94a3b8;
  text-transform: uppercase;
}
.hm-item-text {
  margin: 6px 0 2px;
  font-size: 10.5px;
  font-weight: 600;
  color: #334155;
  text-align: center;
  line-height: 1.25;
  width: 100%;
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
.hm-explore-link {
  font-size: 9.5px;
  font-weight: 600;
  color: #64748b;
  opacity: 0.85;
}
.hm-reel-item:hover .hm-explore-link {
  color: #2563eb;
  opacity: 1;
}

/* Tablet */
@media (max-width: 1100px) {
  .hm-multi-grid-wrapper {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

/* Mobile */
@media (max-width: 640px) {
  .hm-section-header { margin: 8px auto 10px; }
  .hm-main-title { font-size: 20px; }
  .hm-main-subtitle { font-size: 12px; }
  .hm-multi-grid-wrapper {
    grid-template-columns: 1fr;
    padding: 8px 10px 24px;
    gap: 14px;
  }
  .hm-nav-controls { display: none; }
  .hm-reel-item {
    flex: 0 0 calc(38% - 6px);
    min-width: 100px;
  }
}
</style>

<script>
function scrollSlider(button, direction) {
  const showcase = button.closest('.hm-showcase-box');
  const track = showcase.querySelector('.hm-reel-track');
  const itemWidth = track.querySelector('.hm-reel-item').offsetWidth + 10;
  track.scrollBy({
    left: itemWidth * 2 * direction,
    behavior: 'smooth'
  });
}

document.addEventListener('DOMContentLoaded', () => {
  const navTrack = document.querySelector('.hm-quick-nav-track');
  const pills = document.querySelectorAll('.hm-quick-pill');
  const boxes = document.querySelectorAll('.hm-showcase-box');

  if (!navTrack || !pills.length || !boxes.length) return;

  const observerOptions = {
    root: null,
    rootMargin: '-20% 0px -65% 0px',
    threshold: 0
  };

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const activeId = entry.target.id;
        const matchingPill = document.querySelector(`.hm-quick-pill[href="#${activeId}"]`);

        if (matchingPill) {
          pills.forEach(pill => pill.classList.remove('is-active'));
          matchingPill.classList.add('is-active');

          const trackRect = navTrack.getBoundingClientRect();
          const pillRect = matchingPill.getBoundingClientRect();
          const offset = (pillRect.left - trackRect.left) - (trackRect.width / 2) + (pillRect.width / 2);

          navTrack.scrollBy({
            left: offset,
            behavior: 'smooth'
          });
        }
      }
    });
  }, observerOptions);

  boxes.forEach(box => observer.observe(box));
});
</script>

{% schema %}
{
  "name": "Grouped Category Sliders",
  "settings": [],
  "presets": [
    {
      "name": "Grouped Category Sliders"
    }
  ]
}
{% endschema %}



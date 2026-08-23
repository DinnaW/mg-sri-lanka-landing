<script setup>
const section = {
  index: "/03",
  title: "Electric vehicles crafted\nfor the next generation.",
  model: "MG S5 EV",
  category: "Electric Urban Crossover",
  specs: [
    "Up to 520 km estimated range",
    "Rapid charging capability",
    "Responsive electric performance",
    "Advanced driver-focused cabin",
    "Smart connected technology",
  ],
  cta: "Explore Details",
  storyTitle: "Every surface has a purpose. Every detail creates emotion.",
  storyText:
    "MG brings together expressive exterior design, a refined interior, connected technology and efficient electrified performance. The result is a vehicle experience that feels modern, premium and ready for everyday life.",
};

const heroImage =
  "https://news.mgmotor.eu/wp-content/uploads/2025/05/KV2-1040x585.jpg";

const detailCards = [
  {
    label: "Signature Lighting",
    image:
      "https://news.mgmotor.eu/wp-content/uploads/2025/05/Headlight-1040x585.jpg",
    alt: "MG signature headlight detail",
  },
  {
    label: "Charge Ready",
    image:
      "https://images.unsplash.com/photo-1617788138017-80ad40651399?auto=format&fit=crop&w=1400&q=80",
    alt: "Electric vehicle charging close-up",
  },
];

const wideImage =
  "https://news.mgmotor.eu/wp-content/uploads/2025/05/KV1-1040x585.jpg";

const bottomCards = [
  {
    label: "Wheel Design",
    image:
      "https://images.unsplash.com/photo-1503376780353-7e6692767b70?auto=format&fit=crop&w=1200&q=80",
    alt: "Premium alloy wheel detail",
  },
  {
    label: "Craftsmanship",
    image:
      "https://images.unsplash.com/photo-1492144534655-ae79c964c9d7?auto=format&fit=crop&w=1200&q=80",
    alt: "Automotive craftsmanship detail",
  },
  {
    label: "Interior",
    image:
      "https://news.mgmotor.eu/wp-content/uploads/2025/05/Grand-interior-decor-1040x585.jpg",
    alt: "MG interior cockpit",
  },
];
</script>

<template>
  <section class="mg-editorial">
    <div class="mg-editorial__container">
      <!-- TOP HEADER -->
      <div class="mg-editorial__top">
        <span class="mg-editorial__index">
          {{ section.index }}
        </span>

        <h2 class="mg-editorial__title">
          {{ section.title }}
        </h2>
      </div>

      <!-- MAIN GRID -->
      <div class="mg-editorial__grid">
        <!-- LEFT INFO -->
        <aside class="mg-editorial__specs">
          <div class="mg-editorial__specs-inner">
            <h3>{{ section.model }}</h3>
            <p class="mg-editorial__specs-category">
              {{ section.category }}
            </p>

            <ul class="mg-editorial__spec-list">
              <li v-for="item in section.specs" :key="item">
                {{ item }}
              </li>
            </ul>

            <a href="#" class="mg-editorial__cta">
              <span>{{ section.cta }}</span>

              <span class="mg-editorial__cta-icon">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                  <path
                    d="M5 12h14M14 7l5 5-5 5"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="1.5"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>
              </span>
            </a>
          </div>
        </aside>

        <!-- HERO -->
        <article class="mg-editorial__hero">
          <img
            :src="heroImage"
            alt="MG electric vehicle hero"
            loading="lazy"
            decoding="async"
          />

          <div class="mg-editorial__hero-overlay"></div>

          <div class="mg-editorial__hero-note">
            <span>MG ELECTRIC DESIGN</span>
            <strong>Crafted for modern mobility</strong>
          </div>
        </article>

        <!-- DETAIL CARD 1 -->
        <article
          v-for="card in detailCards"
          :key="card.label"
          class="mg-editorial__detail"
        >
          <div class="mg-editorial__detail-image">
            <img
              :src="card.image"
              :alt="card.alt"
              loading="lazy"
              decoding="async"
            />
          </div>

          <span class="mg-editorial__detail-label">
            {{ card.label }}
          </span>
        </article>

        <!-- WIDE STORY IMAGE -->
        <article class="mg-editorial__wide">
          <img
            :src="wideImage"
            alt="MG brand storytelling image"
            loading="lazy"
            decoding="async"
          />
        </article>

        <!-- STORY TEXT -->
        <article class="mg-editorial__story">
          <div class="mg-editorial__story-inner">
            <span class="mg-editorial__story-eyebrow">
              MG DESIGN STORY
            </span>

            <p class="mg-editorial__story-text">
              {{ section.storyText }}
            </p>
          </div>
        </article>

        <!-- BOTTOM TEXT -->
        <article class="mg-editorial__bottom-text">
          <span class="mg-editorial__bottom-kicker">ONE</span>

          <h3>
            {{ section.storyTitle }}
          </h3>
        </article>

        <!-- BOTTOM GALLERY -->
        <article
          v-for="card in bottomCards"
          :key="card.label"
          class="mg-editorial__mini"
        >
          <div class="mg-editorial__mini-image">
            <img
              :src="card.image"
              :alt="card.alt"
              loading="lazy"
              decoding="async"
            />
          </div>

          <span class="mg-editorial__mini-label">
            {{ card.label }}
          </span>
        </article>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700;800&display=swap");

.mg-editorial {
  --bg: #f5f5f3;
  --card: #ffffff;
  --text: #131313;
  --muted: #6a6a6a;
  --line: rgba(0, 0, 0, 0.08);
  --red: #e51920;

  width: 100%;
  background: var(--bg);
  color: var(--text);
  padding: clamp(72px, 6vw, 110px) 0;
  font-family: "Manrope", sans-serif;
  overflow: hidden;
}

.mg-editorial__container {
  width: min(92%, 1540px);
  margin: 0 auto;
}

.mg-editorial__top {
  position: relative;
  margin-bottom: clamp(34px, 3.2vw, 52px);
  min-height: clamp(54px, 5vw, 76px);
}

.mg-editorial__index {
  position: absolute;
  left: 0;
  top: 0;
  color: #6e6e6e;
  font-size: 0.75vw;
  font-weight: 600;
  letter-spacing: -0.02em;
}

.mg-editorial__title {
  margin: 0;
  text-align: center;
  white-space: pre-line;
  font-size: 2.6vw;
  line-height: 1.02;
  letter-spacing: -0.06em;
  font-weight: 650;
}

.mg-editorial__grid {
  display: grid;
  grid-template-columns: 1.1fr 1fr 1fr 1fr 1.15fr;
  grid-template-areas:
    "spec hero hero hero hero"
    "spec detail1 detail1 detail2 detail2"
    "wide wide wide story story"
    "bottomtext mini1 mini2 mini3 mini3";
  gap: clamp(14px, 1.15vw, 20px);
  align-items: start;
}

/* LEFT SPECS */
.mg-editorial__specs {
  grid-area: spec;
  min-height: 100%;
}

.mg-editorial__specs-inner {
  height: 100%;
  padding: clamp(8px, 0.7vw, 12px) 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.mg-editorial__specs h3 {
  margin: 0;
  font-size: 1.65vw;
  line-height: 1;
  letter-spacing: -0.05em;
  font-weight: 700;
}

.mg-editorial__specs-category {
  margin: 10px 0 24px;
  color: var(--muted);
  font-size: 0.7vw;
  font-weight: 600;
  letter-spacing: 0.02em;
  text-transform: uppercase;
}

.mg-editorial__spec-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  gap: 14px;
}

.mg-editorial__spec-list li {
  color: #4f4f4f;
  font-size: 0.8vw;
  line-height: 1.5;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--line);
}

.mg-editorial__cta {
  margin-top: 28px;
  display: inline-flex;
  align-items: center;
  gap: 12px;
  width: fit-content;
  text-decoration: none;
  color: var(--text);
  font-size: 0.78vw;
  font-weight: 600;
}

.mg-editorial__cta-icon {
  width: 34px;
  height: 34px;
  display: grid;
  place-items: center;
  border: 1px solid var(--line);
  border-radius: 999px;
  transition: all 0.3s ease;
}

.mg-editorial__cta:hover .mg-editorial__cta-icon {
  background: var(--red);
  color: #fff;
  border-color: var(--red);
}

.mg-editorial__cta-icon svg {
  width: 14px;
}

/* HERO */
.mg-editorial__hero {
  grid-area: hero;
  position: relative;
  background: var(--card);
  overflow: hidden;
  aspect-ratio: 2.4 / 1;
}

.mg-editorial__hero img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  display: block;
  transition: transform 1s ease;
}

.mg-editorial__hero:hover img {
  transform: scale(1.03);
}

.mg-editorial__hero-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0.02), rgba(0, 0, 0, 0.12));
}

.mg-editorial__hero-note {
  position: absolute;
  right: 18px;
  bottom: 18px;
  display: flex;
  flex-direction: column;
  gap: 4px;
  background: rgba(255, 255, 255, 0.88);
  padding: 12px 14px;
  backdrop-filter: blur(8px);
  max-width: 250px;
}

.mg-editorial__hero-note span {
  font-size: 0.625vw;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: #707070;
  font-weight: 700;
}

.mg-editorial__hero-note strong {
  font-size: 0.8vw;
  line-height: 1.35;
  font-weight: 600;
  letter-spacing: -0.02em;
}

/* DETAIL */
.mg-editorial__detail {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.mg-editorial__detail:nth-of-type(1) {
  grid-area: detail1;
}

.mg-editorial__detail:nth-of-type(2) {
  grid-area: detail2;
}

.mg-editorial__detail-image {
  background: var(--card);
  overflow: hidden;
  aspect-ratio: 1.35 / 1;
}

.mg-editorial__detail-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.9s ease;
}

.mg-editorial__detail:hover .mg-editorial__detail-image img {
  transform: scale(1.04);
}

.mg-editorial__detail-label {
  color: #4f4f4f;
  font-size: 0.66vw;
  font-weight: 600;
}

/* WIDE */
.mg-editorial__wide {
  grid-area: wide;
  background: var(--card);
  overflow: hidden;
  aspect-ratio: 2 / 0.9;
}

.mg-editorial__wide img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* STORY */
.mg-editorial__story {
  grid-area: story;
  min-height: 100%;
  display: flex;
  align-items: center;
}

.mg-editorial__story-inner {
  padding: clamp(20px, 2vw, 30px);
}

.mg-editorial__story-eyebrow {
  display: inline-block;
  margin-bottom: 16px;
  color: var(--red);
  font-size: 0.65vw;
  text-transform: uppercase;
  font-weight: 700;
  letter-spacing: 0.08em;
}

.mg-editorial__story-text {
  margin: 0;
  max-width: 420px;
  color: #4e4e4e;
  font-size: 0.95vw;
  line-height: 1.72;
  letter-spacing: -0.02em;
}

/* BOTTOM TEXT */
.mg-editorial__bottom-text {
  grid-area: bottomtext;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  padding-top: 4px;
}

.mg-editorial__bottom-kicker {
  display: block;
  margin-bottom: 16px;
  color: #222;
  font-size: 1.5vw;
  font-weight: 500;
  letter-spacing: -0.05em;
}

.mg-editorial__bottom-text h3 {
  margin: 0;
  max-width: 320px;
  font-size: 1.85vw;
  line-height: 1.08;
  letter-spacing: -0.05em;
  font-weight: 600;
}

/* MINI CARDS */
.mg-editorial__mini {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.mg-editorial__mini:nth-of-type(1) {
  grid-area: mini1;
}

.mg-editorial__mini:nth-of-type(2) {
  grid-area: mini2;
}

.mg-editorial__mini:nth-of-type(3) {
  grid-area: mini3;
}

.mg-editorial__mini-image {
  background: var(--card);
  overflow: hidden;
  aspect-ratio: 1.2 / 1;
}

.mg-editorial__mini:nth-of-type(3) .mg-editorial__mini-image {
  aspect-ratio: 1.55 / 1;
}

.mg-editorial__mini-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.9s ease;
}

.mg-editorial__mini:hover .mg-editorial__mini-image img {
  transform: scale(1.04);
}

.mg-editorial__mini-label {
  color: #4f4f4f;
  font-size: 0.66vw;
  font-weight: 600;
}

/* LARGE SCREENS */


/* TABLET */
@media (max-width: 1100px) {
  .mg-editorial__grid {
    grid-template-columns: 1fr 1fr;
    grid-template-areas:
      "hero hero"
      "spec spec"
      "detail1 detail2"
      "wide story"
      "bottomtext bottomtext"
      "mini1 mini2"
      "mini3 mini3";
  }

  .mg-editorial__specs-inner {
    padding: 0;
  }

  .mg-editorial__spec-list {
    grid-template-columns: 1fr 1fr;
    gap: 12px 24px;
  }

  .mg-editorial__hero {
    aspect-ratio: 1.8 / 1;
  }
}

/* MOBILE */
@media (max-width: 767px) {
  .mg-editorial {
    padding: 58px 0 68px;
  }

  .mg-editorial__container {
    width: calc(100% - 30px);
  }

  .mg-editorial__top {
    min-height: auto;
    margin-bottom: 28px;
  }

  .mg-editorial__index {
    position: static;
    display: block;
    margin-bottom: 12px;
    font-size: 10px;
  }

  .mg-editorial__title {
    text-align: left;
    font-size: 30px;
  }

  .mg-editorial__grid {
    grid-template-columns: 1fr;
    grid-template-areas:
      "hero"
      "spec"
      "detail1"
      "detail2"
      "wide"
      "story"
      "bottomtext"
      "mini1"
      "mini2"
      "mini3";
    gap: 14px;
  }

  .mg-editorial__hero,
  .mg-editorial__detail-image,
  .mg-editorial__wide,
  .mg-editorial__mini-image {
    border-radius: 0;
  }

  .mg-editorial__hero {
    aspect-ratio: 1.25 / 1;
  }

  .mg-editorial__hero-note {
    right: 12px;
    left: 12px;
    bottom: 12px;
    max-width: none;
  }

  .mg-editorial__spec-list {
    grid-template-columns: 1fr;
  }

  .mg-editorial__story-inner {
    padding: 10px 0 6px;
  }

  .mg-editorial__story-text {
    max-width: none;
    font-size: 14px;
  }

  .mg-editorial__bottom-kicker {
    margin-bottom: 10px;
    font-size: 20px;
  }

  .mg-editorial__bottom-text h3 {
    font-size: 24px;
    max-width: none;
  }
}
</style>
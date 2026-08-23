<script setup>
import { computed, onBeforeUnmount, onMounted, ref } from "vue";

/* =========================================================
   CAROUSEL STATE
========================================================= */

const activeSlide = ref(0);
let autoplayTimer = null;

/* =========================================================
   MG MOTOR EUROPE REMOTE IMAGES
   Temporary development setup.
========================================================= */

const slides = [
  {
    id: 1,
    image: "https://news.mgmotor.eu/wp-content/uploads/2025/05/KV1-1040x585.jpg",
    alt: "MGS5 EV charging in a contemporary architectural setting",
    quote:
      "Quiet, responsive and effortless — electric mobility designed to make everyday journeys feel more refined.",
    meta: "MGS5 EV · Electric Experience",
    objectPosition: "center center",
  },
  {
    id: 2,
    image: "https://news.mgmotor.eu/wp-content/uploads/2025/05/KV2-1040x585.jpg",
    alt: "MGS5 EV exterior in a premium architectural setting",
    quote:
      "Confident proportions, intelligent technology and instantly responsive performance — unmistakably MG.",
    meta: "MGS5 EV · Modern Electric Driving",
    objectPosition: "center center",
  },
  {
    id: 3,
    image: "https://news.mgmotor.eu/wp-content/uploads/2025/05/Grand-interior-decor-1040x585.jpg",
    alt: "MGS5 EV interior and cockpit",
    quote:
      "A calm, connected cabin that puts comfort, information and intuitive control exactly where the driver needs them.",
    meta: "MGS5 EV · Interior Experience",
    objectPosition: "center center",
  },
  {
    id: 4,
    image: "https://news.mgmotor.eu/wp-content/uploads/2025/05/Headlight-1040x585.jpg",
    alt: "MGS5 EV LED headlight detail",
    quote:
      "Distinctive lighting and carefully considered details create a modern MG identity from every angle.",
    meta: "MGS5 EV · Signature Design",
    objectPosition: "center center",
  },
];

const currentSlide = computed(() => slides[activeSlide.value]);

/* =========================================================
   NAVIGATION
========================================================= */

const nextSlide = () => {
  activeSlide.value = (activeSlide.value + 1) % slides.length;
};

const previousSlide = () => {
  activeSlide.value =
    (activeSlide.value - 1 + slides.length) % slides.length;
};

const selectSlide = (index) => {
  activeSlide.value = index;
  startAutoplay();
};

/* =========================================================
   AUTOPLAY
========================================================= */

const stopAutoplay = () => {
  if (!autoplayTimer) return;
  window.clearInterval(autoplayTimer);
  autoplayTimer = null;
};

const startAutoplay = () => {
  stopAutoplay();
  autoplayTimer = window.setInterval(nextSlide, 6500);
};

onMounted(startAutoplay);
onBeforeUnmount(stopAutoplay);
</script>

<template>
  <section class="mg-owners">
    <div class="mg-owners__container">
      <!-- =====================================================
           HEADER
      ====================================================== -->
      <header class="mg-owners__header">
        <div class="mg-owners__heading">
          <span class="mg-owners__eyebrow">MG EXPERIENCE</span>

          <h2>
            Great Journeys,
            <br />
            Made the MG Way.
          </h2>
        </div>

        <div class="mg-owners__header-right">
          <a href="#" class="mg-owners__cta">
            <span>Explore the Range</span>

            <span class="mg-owners__cta-arrow">
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

          <p>
            A century of motoring character, now expressed through a new
            generation of connected, electrified and intelligently designed
            MG vehicles.
          </p>
        </div>
      </header>

      <!-- =====================================================
           MAIN STAGE
      ====================================================== -->
      <div class="mg-owners__stage">
        <!-- ===================================================
             IMAGE CAROUSEL
        ==================================================== -->
        <article
          class="mg-owner-card"
          @mouseenter="stopAutoplay"
          @mouseleave="startAutoplay"
        >
          <div class="mg-owner-card__visual">
            <Transition name="owner-image" mode="out-in">
              <img
                :key="currentSlide.id"
                class="mg-owner-card__image"
                :src="currentSlide.image"
                :alt="currentSlide.alt"
                :style="{ objectPosition: currentSlide.objectPosition }"
                loading="lazy"
                decoding="async"
                draggable="false"
              />
            </Transition>

            <div class="mg-owner-card__image-overlay"></div>

            <div class="mg-owner-card__image-tag">
              <span>MGS5 EV</span>
              <i></i>
              <span>100% ELECTRIC</span>
            </div>

            <div class="mg-owner-card__index">
              <span>{{ String(activeSlide + 1).padStart(2, "0") }}</span>
              <i></i>
              <span>{{ String(slides.length).padStart(2, "0") }}</span>
            </div>
          </div>

          <!-- PREVIOUS -->
          <button
            type="button"
            class="mg-owner-card__nav mg-owner-card__nav--prev"
            aria-label="Previous MG story"
            @click="previousSlide"
          >
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path
                d="M19 12H5M10 7l-5 5 5 5"
                fill="none"
                stroke="currentColor"
                stroke-width="1.4"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </button>

          <!-- NEXT -->
          <button
            type="button"
            class="mg-owner-card__nav mg-owner-card__nav--next"
            aria-label="Next MG story"
            @click="nextSlide"
          >
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path
                d="M5 12h14M14 7l5 5-5 5"
                fill="none"
                stroke="currentColor"
                stroke-width="1.4"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </button>

          <!-- STORY COPY -->
          <div class="mg-owner-card__copy">
            <Transition name="owner-copy" mode="out-in">
              <div :key="currentSlide.id" class="mg-owner-card__copy-inner">
                <blockquote>“{{ currentSlide.quote }}”</blockquote>
                <p>{{ currentSlide.meta }}</p>
              </div>
            </Transition>

            <div class="mg-owner-card__progress">
              <button
                v-for="(slide, index) in slides"
                :key="slide.id"
                type="button"
                :class="{ 'is-active': activeSlide === index }"
                :aria-label="`Show MG story ${index + 1}`"
                @click="selectSlide(index)"
              ></button>
            </div>
          </div>
        </article>

        <!-- ===================================================
             STAT 01
        ==================================================== -->
        <article class="mg-stat mg-stat--experience">
          <div class="mg-stat__topline">
            <span>01</span>
            <i></i>
            <span>HERITAGE</span>
          </div>

          <div class="mg-stat__number">
            <strong>100</strong>
            <span>+</span>
          </div>

          <div class="mg-stat__bottom">
            <span>Years of</span>
            <strong>MG motoring heritage</strong>
          </div>
        </article>

        <!-- ===================================================
             STAT 02
        ==================================================== -->
        <article class="mg-stat mg-stat--since">
          <div class="mg-stat__topline">
            <span>02</span>
            <i></i>
            <span>SINCE</span>
          </div>

          <div class="mg-stat__number mg-stat__number--year">
            <strong>1924</strong>
          </div>

          <div class="mg-stat__bottom">
            <span>Driving forward</span>
            <strong>Since 1924</strong>
          </div>
        </article>

        <!-- ===================================================
             BRAND PROOF
        ==================================================== -->
        <aside class="mg-brand-proof">
          <div class="mg-brand-proof__mark">MG</div>

          <div class="mg-brand-proof__content">
            <span class="mg-brand-proof__eyebrow">NEW GENERATION</span>

            <div class="mg-brand-proof__title">
              <strong>Electric</strong>

              <span class="mg-brand-proof__stars" aria-hidden="true">
                <i></i>
                <i></i>
                <i></i>
                <i></i>
                <i></i>
              </span>
            </div>

            <p>Progressive design. Connected technology. Everyday usability.</p>
          </div>
        </aside>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;500;600&family=Manrope:wght@400;500;600;700&display=swap");

/* =========================================================
   SECTION
========================================================= */

.mg-owners {
  --mg-red: #e51920;
  --black: #111111;
  --page: #f2f2f1;
  --card: #fbfbfa;
  --grey: #8e908e;

  position: relative;
  width: 100%;
  overflow: hidden;
  padding: clamp(72px, 6.2vw, 105px) 0 clamp(72px, 6vw, 105px);
  background: var(--page);
  color: var(--black);
  font-family: "Manrope", sans-serif;
}

/* =========================================================
   CONTAINER
========================================================= */

.mg-owners__container {
  width: min(93%, 1500px);
  margin: 0 auto;
}

/* =========================================================
   HEADER
========================================================= */

.mg-owners__header {
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(300px, 0.72fr);
  gap: clamp(80px, 8vw, 150px);
  align-items: start;
  margin-bottom: clamp(72px, 8.5vw, 140px);
}

.mg-owners__eyebrow {
  display: block;
  margin-bottom: clamp(19px, 1.55vw, 27px);
  color: #535553;
  font-size: 0.78vw;
  font-weight: 600;
  letter-spacing: -0.01em;
}

.mg-owners__heading h2 {
  margin: 0;
  max-width: 600px;
  font-size: 3.25vw;
  font-weight: 650;
  line-height: 1.02;
  letter-spacing: -0.055em;
}

/* =========================================================
   HEADER RIGHT
========================================================= */

.mg-owners__header-right {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding-top: 15px;
}

.mg-owners__cta {
  align-self: flex-end;
  display: inline-flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  min-width: 230px;
  height: 55px;
  padding: 4px 5px 4px 20px;
  box-sizing: border-box;
  border-radius: 16px;
  background: #050505;
  color: #fff;
  text-decoration: none;
  font-size: 0.75vw;
  font-weight: 600;
  transition: transform 0.35s cubic-bezier(0.16, 1, 0.3, 1);
}

.mg-owners__cta:hover {
  transform: translateY(-2px);
}

.mg-owners__cta-arrow {
  width: 46px;
  height: 46px;
  flex: 0 0 46px;
  display: grid;
  place-items: center;
  border-radius: 13px;
  background: #fff;
  color: #111;
  transition: color 0.3s ease, background 0.3s ease;
}

.mg-owners__cta:hover .mg-owners__cta-arrow {
  color: #fff;
  background: var(--mg-red);
}

.mg-owners__cta-arrow svg {
  width: 17px;
}

.mg-owners__header-right p {
  width: min(100%, 390px);
  margin: 28px 0 0;
  color: #5e605e;
  font-size: 0.83vw;
  line-height: 1.55;
  letter-spacing: -0.015em;
}

/* =========================================================
   MAIN STAGE
========================================================= */

.mg-owners__stage {
  display: grid;
  grid-template-columns:
    minmax(0, 2.08fr)
    minmax(210px, 0.97fr)
    minmax(230px, 1.05fr)
    minmax(145px, 0.55fr);
  gap: clamp(17px, 1.5vw, 27px);
  align-items: end;
}

/* =========================================================
   CAROUSEL
========================================================= */

.mg-owner-card {
  position: relative;
  height: clamp(480px, 38vw, 610px);
  overflow: visible;
  border-radius: 16px;
  background: var(--card);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.018);
}

.mg-owner-card__visual {
  position: relative;
  width: 100%;
  height: 72%;
  overflow: hidden;
  border-radius: 16px 16px 0 0;
  background: #dededc;
}

.mg-owner-card__image {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transform: scale(1.01);
}

.mg-owner-card__image-overlay {
  position: absolute;
  inset: 0;
  pointer-events: none;
  background:
    linear-gradient(180deg, rgba(0, 0, 0, 0.03), transparent 42%),
    linear-gradient(0deg, rgba(0, 0, 0, 0.14), transparent 42%);
}

.mg-owner-card__image-tag {
  position: absolute;
  left: 18px;
  bottom: 16px;
  z-index: 6;
  display: flex;
  align-items: center;
  gap: 7px;
  padding: 7px 9px;
  border: 1px solid rgba(255, 255, 255, 0.32);
  border-radius: 999px;
  background: rgba(0, 0, 0, 0.28);
  backdrop-filter: blur(10px);
  color: #fff;
  font-size: 0.347vw;
  font-weight: 700;
  letter-spacing: 0.12em;
}

.mg-owner-card__image-tag i {
  width: 16px;
  height: 1px;
  background: rgba(255, 255, 255, 0.45);
}

.mg-owner-card__index {
  position: absolute;
  right: 17px;
  bottom: 15px;
  z-index: 6;
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 6px 8px;
  border-radius: 999px;
  background: rgba(0, 0, 0, 0.38);
  backdrop-filter: blur(9px);
  color: rgba(255, 255, 255, 0.82);
  font-size: 0.417vw;
  font-weight: 600;
  letter-spacing: 0.1em;
}

.mg-owner-card__index i {
  width: 17px;
  height: 1px;
  background: rgba(255, 255, 255, 0.45);
}

/* =========================================================
   NAV
========================================================= */

.mg-owner-card__nav {
  position: absolute;
  top: 4px;
  z-index: 20;
  width: 51px;
  height: 51px;
  display: grid;
  place-items: center;
  padding: 0;
  border: 4px solid var(--page);
  border-radius: 15px;
  background: linear-gradient(145deg, #30312f, #080808);
  color: #fff;
  cursor: pointer;
  box-sizing: content-box;
  transition: background 0.3s ease, transform 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.mg-owner-card__nav:hover {
  background: var(--mg-red);
}

.mg-owner-card__nav--prev {
  left: 0;
  transform: translate(-3px, -4px);
}

.mg-owner-card__nav--prev:hover {
  transform: translate(-6px, -4px);
}

.mg-owner-card__nav--next {
  right: 0;
  transform: translate(3px, -4px);
}

.mg-owner-card__nav--next:hover {
  transform: translate(6px, -4px);
}

.mg-owner-card__nav svg {
  width: 18px;
}

/* =========================================================
   COPY
========================================================= */

.mg-owner-card__copy {
  position: relative;
  height: 28%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: clamp(18px, 1.7vw, 26px) clamp(20px, 1.75vw, 27px) 18px;
  box-sizing: border-box;
}

.mg-owner-card__copy-inner {
  min-width: 0;
}

.mg-owner-card__copy blockquote {
  max-width: 610px;
  margin: 0;
  font-size: 1.13vw;
  font-weight: 500;
  line-height: 1.44;
  letter-spacing: -0.025em;
}

.mg-owner-card__copy p {
  margin: 17px 0 0;
  color: #666765;
  font-size: 0.73vw;
  font-weight: 500;
}

.mg-owner-card__progress {
  position: absolute;
  right: 20px;
  bottom: 20px;
  display: flex;
  gap: 5px;
}

.mg-owner-card__progress button {
  width: 16px;
  height: 2px;
  padding: 0;
  border: 0;
  background: #d1d2cf;
  cursor: pointer;
  transition: width 0.35s ease, background 0.35s ease;
}

.mg-owner-card__progress button.is-active {
  width: 28px;
  background: var(--mg-red);
}

/* =========================================================
   TRANSITIONS
========================================================= */

.owner-image-enter-active,
.owner-image-leave-active {
  transition: opacity 0.46s ease, transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.owner-image-enter-from {
  opacity: 0;
  transform: scale(1.045);
}

.owner-image-leave-to {
  opacity: 0;
  transform: scale(0.995);
}

.owner-copy-enter-active,
.owner-copy-leave-active {
  transition: opacity 0.3s ease, transform 0.4s ease;
}

.owner-copy-enter-from {
  opacity: 0;
  transform: translateY(7px);
}

.owner-copy-leave-to {
  opacity: 0;
  transform: translateY(-5px);
}

/* =========================================================
   STAT CARDS
========================================================= */

.mg-stat {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-width: 0;
  padding: clamp(24px, 2vw, 32px);
  box-sizing: border-box;
  border-radius: 16px;
  background: rgba(255, 255, 255, 0.68);
  overflow: hidden;
}

.mg-stat--experience {
  height: clamp(330px, 27vw, 430px);
}

.mg-stat--since {
  height: clamp(410px, 34vw, 520px);
}

.mg-stat__topline {
  display: flex;
  align-items: center;
  gap: 7px;
  color: rgba(0, 0, 0, 0.34);
  font-size: 0.347vw;
  font-weight: 700;
  letter-spacing: 0.12em;
}

.mg-stat__topline i {
  width: 22px;
  height: 1px;
  background: rgba(0, 0, 0, 0.14);
}

.mg-stat__number {
  display: flex;
  align-items: flex-start;
  color: #898b89;
  line-height: 1;
  margin-top: 18px;
}

.mg-stat__number strong {
  font-size: 4.4vw;
  font-weight: 400;
  line-height: 0.94;
  letter-spacing: -0.075em;
}

.mg-stat__number span {
  margin-left: 3px;
  font-size: 2.5vw;
  font-weight: 400;
  line-height: 0.9;
}

.mg-stat__number--year strong {
  font-size: 4vw;
}

.mg-stat__bottom span {
  display: block;
  margin-bottom: 4px;
  color: #707270;
  font-size: 0.68vw;
}

.mg-stat__bottom strong {
  display: block;
  color: #565856;
  font-size: 0.75vw;
  font-weight: 500;
}

.mg-stat::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: 0;
  width: 0;
  height: 3px;
  background: var(--mg-red);
  transition: width 0.55s cubic-bezier(0.16, 1, 0.3, 1);
}

.mg-stat:hover::after {
  width: 100%;
}

/* =========================================================
   BRAND PROOF
========================================================= */

.mg-brand-proof {
  align-self: end;
  display: flex;
  align-items: flex-start;
  gap: 12px;
  margin-bottom: 3px;
  padding-bottom: 3px;
}

.mg-brand-proof__mark {
  width: 30px;
  height: 30px;
  flex: 0 0 30px;
  display: grid;
  place-items: center;
  border: 2px solid var(--mg-red);
  color: var(--mg-red);
  font-family: "Barlow Condensed", sans-serif;
  font-size: 0.903vw;
  font-weight: 600;
  line-height: 1;
}

.mg-brand-proof__content {
  min-width: 0;
}

.mg-brand-proof__eyebrow {
  display: block;
  margin-bottom: 4px;
  color: #777977;
  font-size: 0.347vw;
  font-weight: 700;
  letter-spacing: 0.12em;
}

.mg-brand-proof__title {
  display: flex;
  align-items: center;
  gap: 8px;
}

.mg-brand-proof__title strong {
  font-size: 0.75vw;
  font-weight: 600;
}

.mg-brand-proof__stars {
  display: flex;
  align-items: center;
  gap: 3px;
}

.mg-brand-proof__stars i {
  width: 7px;
  height: 7px;
  display: block;
  background: #111;
  clip-path: polygon(
    50% 0%,
    61% 35%,
    98% 35%,
    68% 57%,
    79% 93%,
    50% 72%,
    21% 93%,
    32% 57%,
    2% 35%,
    39% 35%
  );
}

.mg-brand-proof__content p {
  margin: 5px 0 0;
  color: #646664;
  font-size: 0.62vw;
  line-height: 1.4;
}

/* =========================================================
   LARGE DESKTOP
========================================================= */



/* =========================================================
   SMALL DESKTOP
========================================================= */

@media (max-width: 1180px) {
  .mg-owners__header {
    gap: 60px;
    margin-bottom: 80px;
  }

  .mg-owners__stage {
    grid-template-columns: 1.8fr 0.8fr 0.9fr;
  }

  .mg-brand-proof {
    grid-column: 3;
    margin-top: 18px;
  }

  .mg-owner-card {
    height: 500px;
  }

  .mg-stat--experience {
    height: 340px;
  }

  .mg-stat--since {
    height: 420px;
  }
}

/* =========================================================
   TABLET
========================================================= */

@media (max-width: 900px) {
  .mg-owners__header {
    grid-template-columns: 1fr 0.8fr;
    gap: 40px;
    margin-bottom: 65px;
  }

  .mg-owners__heading h2 {
    font-size: 42px;
  }

  .mg-owners__stage {
    grid-template-columns: 1.55fr 0.72fr 0.72fr;
    gap: 14px;
  }

  .mg-owner-card {
    height: 470px;
  }

  .mg-owner-card__visual {
    height: 69%;
  }

  .mg-owner-card__copy {
    height: 31%;
  }

  .mg-stat--experience {
    height: 310px;
  }

  .mg-stat--since {
    height: 380px;
  }

  .mg-stat {
    padding: 20px;
  }

  .mg-stat__number strong {
    font-size: 47px;
  }
}

/* =========================================================
   MOBILE
========================================================= */

@media (max-width: 767px) {
  .mg-owners {
    padding: 58px 0 65px;
  }

  .mg-owners__container {
    width: calc(100% - 30px);
  }

  .mg-owners__header {
    display: block;
    margin-bottom: 48px;
  }

  .mg-owners__eyebrow {
    margin-bottom: 18px;
    font-size: 9px;
  }

  .mg-owners__heading h2 {
    font-size: 36px;
  }

  .mg-owners__header-right {
    display: grid;
    grid-template-columns: 1fr;
    gap: 18px;
    margin-top: 28px;
    padding: 0;
  }

  .mg-owners__cta {
    align-self: auto;
    justify-self: start;
    order: 2;
    min-width: 205px;
    height: 50px;
  }

  .mg-owners__cta-arrow {
    width: 41px;
    height: 41px;
    flex-basis: 41px;
  }

  .mg-owners__header-right p {
    order: 1;
    max-width: 330px;
    margin: 0;
    font-size: 10px;
  }

  .mg-owners__stage {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 12px;
  }

  .mg-owner-card {
    grid-column: 1 / 3;
    height: 520px;
    border-radius: 14px;
  }

  .mg-owner-card__visual {
    height: 70%;
    border-radius: 14px 14px 0 0;
  }

  .mg-owner-card__copy {
    height: 30%;
    padding: 20px;
  }

  .mg-owner-card__copy blockquote {
    max-width: 95%;
    font-size: 15px;
  }

  .mg-owner-card__copy p {
    font-size: 9px;
  }

  .mg-owner-card__nav {
    top: 3px;
    width: 45px;
    height: 45px;
    border-radius: 13px;
  }

  .mg-stat {
    height: 260px !important;
    padding: 20px;
    border-radius: 14px;
  }

  .mg-stat__number strong {
    font-size: 48px;
  }

  .mg-stat__number--year strong {
    font-size: 41px;
  }

  .mg-brand-proof {
    grid-column: 1 / 3;
    margin: 20px 0 0;
  }
}

/* =========================================================
   SMALL MOBILE
========================================================= */

@media (max-width: 440px) {
  .mg-owners__heading h2 {
    font-size: 33px;
  }

  .mg-owner-card {
    height: 475px;
  }

  .mg-owner-card__visual {
    height: 66%;
  }

  .mg-owner-card__copy {
    height: 34%;
  }

  .mg-owner-card__copy blockquote {
    font-size: 14px;
    line-height: 1.42;
  }

  .mg-owner-card__progress {
    right: 18px;
    bottom: 17px;
  }

  .mg-stat {
    height: 225px !important;
    padding: 17px;
  }

  .mg-stat__number strong {
    font-size: 40px;
  }

  .mg-stat__number--year strong {
    font-size: 34px;
  }

  .mg-stat__bottom span,
  .mg-stat__bottom strong {
    font-size: 8px;
  }
}

/* =========================================================
   REDUCED MOTION
========================================================= */

@media (prefers-reduced-motion: reduce) {
  .mg-owners *,
  .mg-owners *::before,
  .mg-owners *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
</style>

<script setup>
import { ref } from "vue";
import MgHeader from "./MgHeader.vue";

const asset = (path) =>
  `${import.meta.env.BASE_URL}${path.replace(/^\/+/, "")}`;

/* =========================================================
   HERO DATA
========================================================= */

const hero = {
  label: "100% Electric",
  model: "MG S5 EV",
  tagline: "The EV that makes sense.",
  youtubeId: "ABUkN22N0F4",

  /* Uses the real local MG cutout already in your project. */
  vehicleImage: asset("images/345.png"),

  modelLink: "#mg-range",
  testDriveLink: "#test-drive",
};

/* =========================================================
   YOUTUBE LOADING STATE
========================================================= */

const videoVisible = ref(false);

const handleVideoLoad = () => {
  window.setTimeout(() => {
    videoVisible.value = true;
  }, 1400);
};

/* =========================================================
   YOUTUBE BACKGROUND URL
========================================================= */

const youtubeUrl =
  `https://www.youtube.com/embed/${hero.youtubeId}` +
  `?autoplay=1` +
  `&mute=1` +
  `&controls=0` +
  `&loop=1` +
  `&playlist=${hero.youtubeId}` +
  `&rel=0` +
  `&playsinline=1` +
  `&disablekb=1` +
  `&fs=0` +
  `&iv_load_policy=3` +
  `&autohide=1`;
</script>

<template>
  <section class="mg-hero">
    <!-- REUSABLE MODERN HEADER + VEHICLE MEGA MENU -->
    <MgHeader
      :test-drive-link="hero.testDriveLink"
      dealer-link="#showroom"
    />

    <!-- YOUTUBE BACKGROUND -->
    <div
      class="mg-hero__video"
      :class="{
        'mg-hero__video--visible': videoVisible,
      }"
    >
      <div class="mg-hero__video-cover"></div>

      <iframe
        :src="youtubeUrl"
        :title="`${hero.model} background video`"
        frameborder="0"
        allow="autoplay; encrypted-media"
        tabindex="-1"
        @load="handleVideoLoad"
      ></iframe>
    </div>

    <!-- VIDEO OVERLAYS -->
    <div class="mg-hero__overlay"></div>
    <div class="mg-hero__bottom-shade"></div>

    <!-- HERO CONTENT -->
    <div class="mg-hero__container">
      <div class="mg-hero__intro">
        <span class="mg-hero__label">
          {{ hero.label }}
        </span>

        <h1 class="mg-hero__title">
          {{ hero.model }}
        </h1>

        <p class="mg-hero__tagline">
          {{ hero.tagline }}
        </p>
      </div>

      <!-- LOWER-RIGHT CURRENT MODEL CARD -->
      <div class="vehicle-card">
        <a :href="hero.modelLink" class="vehicle-card__image">
          <div class="vehicle-card__vehicle-bg"></div>

          <img :src="hero.vehicleImage" :alt="hero.model" />

          <div class="vehicle-card__image-shade"></div>

          <div class="vehicle-card__meta">
            <span>Featured model</span>
            <strong>{{ hero.model }}</strong>
          </div>
        </a>

        <div class="vehicle-card__bottom">
          <a :href="hero.testDriveLink" class="vehicle-card__testdrive">
            <span class="vehicle-card__icon">
              <svg viewBox="0 0 24 24" aria-hidden="true">
                <circle
                  cx="12"
                  cy="12"
                  r="8"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="1.5"
                />

                <circle
                  cx="12"
                  cy="12"
                  r="2"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="1.5"
                />

                <path
                  d="M4.6 10.5h14.8M12 14v6"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="1.5"
                  stroke-linecap="round"
                />
              </svg>
            </span>

            <span>Test Drive</span>
          </a>

          <a
            :href="hero.modelLink"
            class="vehicle-card__arrow"
            :aria-label="`Explore ${hero.model}`"
          >
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
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;500;600&family=Manrope:wght@400;500;600;700&display=swap");

.mg-hero {
  --mg-red: #e51920;

  position: relative;
  width: 100%;
  height: 100svh;
  min-height: 680px;
  overflow: hidden;
  background: #000;
  color: #fff;
  isolation: isolate;
  font-family: "Manrope", sans-serif;
}

/* VIDEO */

.mg-hero__video {
  position: absolute;
  inset: 0;
  z-index: -5;
  overflow: hidden;
  background: #000;
}

.mg-hero__video-cover {
  position: absolute;
  inset: 0;
  z-index: 5;
  pointer-events: none;
  background: #000;
  opacity: 1;
  transition: opacity .75s cubic-bezier(.16, 1, .3, 1);
}

.mg-hero__video--visible .mg-hero__video-cover {
  opacity: 0;
}

.mg-hero__video iframe {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100vw;
  height: 56.25vw;
  min-width: 177.78vh;
  min-height: 100vh;
  transform: translate(-50%, -50%);
  pointer-events: none;
  opacity: 0;
  transition: opacity .75s ease;
}

.mg-hero__video--visible iframe {
  opacity: 1;
}

/* HERO IMAGE TREATMENT */

.mg-hero__overlay {
  position: absolute;
  inset: 0;
  z-index: -4;
  background:
    linear-gradient(
      180deg,
      rgba(0, 0, 0, .26) 0%,
      rgba(0, 0, 0, .025) 30%,
      rgba(0, 0, 0, .015) 65%,
      rgba(0, 0, 0, .17) 100%
    );
}

.mg-hero__bottom-shade {
  position: absolute;
  inset: 0;
  z-index: -3;
  background:
    linear-gradient(
      0deg,
      rgba(0, 0, 0, .48) 0%,
      rgba(0, 0, 0, .1) 28%,
      transparent 52%
    );
}

/* MAIN CONTAINER */

.mg-hero__container {
  position: relative;
  width: 92vw;
  height: 100%;
  margin: 0 auto;
}

/* INTRO */

.mg-hero__intro {
  position: absolute;
  top: 7.2vw;
  left: 50%;
  width: 48vw;
  transform: translateX(-50%);
  text-align: center;
  animation: heroIntro 1s cubic-bezier(.16, 1, .3, 1) both;
}

.mg-hero__label {
  display: block;
  margin-bottom: .5vw;
  color: rgba(255, 255, 255, .78);
  font-size: .5vw;
  font-weight: 650;
  letter-spacing: .18em;
  text-transform: uppercase;
}

.mg-hero__title {
  margin: 0;
  font-family: "Barlow Condensed", sans-serif;
  font-size: 4.55vw;
  font-weight: 500;
  line-height: .92;
  letter-spacing: -.025em;
  text-transform: uppercase;
  text-shadow: 0 .25vw 1.4vw rgba(0, 0, 0, .2);
}

.mg-hero__tagline {
  margin: .55vw 0 0;
  color: rgba(255, 255, 255, .92);
  font-size: .94vw;
  font-weight: 400;
  text-shadow: 0 .2vw 1vw rgba(0, 0, 0, .28);
}

@keyframes heroIntro {
  from {
    opacity: 0;
    transform: translateX(-50%) translateY(1.3vw);
  }

  to {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }
}

/* FEATURED MODEL CARD */

.vehicle-card {
  position: absolute;
  right: 0;
  bottom: 2.5vw;
  width: 16.3vw;
  padding: .38vw;
  border: 1px solid rgba(255, 255, 255, .12);
  border-radius: .88vw;
  background: rgba(7, 7, 7, .58);
  backdrop-filter: blur(1.1vw) saturate(1.08);
  -webkit-backdrop-filter: blur(1.1vw) saturate(1.08);
  box-shadow: 0 1.5vw 4vw rgba(0, 0, 0, .22);
  animation: cardReveal 1s .2s cubic-bezier(.16, 1, .3, 1) both;
  transition: transform .35s ease, background .35s ease;
}

.vehicle-card:hover {
  transform: translateY(-.25vw);
  background: rgba(7, 7, 7, .72);
}

.vehicle-card__image {
  position: relative;
  display: block;
  width: 100%;
  aspect-ratio: 16 / 8.6;
  overflow: hidden;
  border-radius: .62vw;
  color: #fff;
  text-decoration: none;
  background: #e8e8e4;
}

.vehicle-card__vehicle-bg {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(circle at 50% 82%, rgba(0, 0, 0, .15), transparent 32%),
    linear-gradient(150deg, #f0f0ed, #d8dbdc);
}

.vehicle-card__image img {
  position: absolute;
  left: 50%;
  top: 48%;
  z-index: 2;
  width: 94%;
  height: 85%;
  object-fit: contain;
  transform: translate(-50%, -50%) scale(1);
  transition: transform .65s cubic-bezier(.16, 1, .3, 1);
}

.vehicle-card:hover .vehicle-card__image img {
  transform: translate(-50%, -52%) scale(1.045);
}

.vehicle-card__image-shade {
  position: absolute;
  inset: 0;
  z-index: 3;
  background: linear-gradient(
    0deg,
    rgba(0, 0, 0, .62) 0%,
    rgba(0, 0, 0, .04) 42%,
    transparent 67%
  );
}

.vehicle-card__meta {
  position: absolute;
  left: .78vw;
  bottom: .68vw;
  z-index: 4;
}

.vehicle-card__meta span {
  display: block;
  margin-bottom: .16vw;
  color: rgba(255, 255, 255, .6);
  font-size: .31vw;
  font-weight: 700;
  letter-spacing: .14em;
  text-transform: uppercase;
}

.vehicle-card__meta strong {
  display: block;
  font-family: "Barlow Condensed", sans-serif;
  font-size: 1.1vw;
  font-weight: 600;
  line-height: 1;
  text-transform: uppercase;
}

.vehicle-card__bottom {
  display: flex;
  align-items: center;
  gap: .38vw;
  padding-top: .38vw;
}

.vehicle-card__testdrive {
  flex: 1;
  height: 2.42vw;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: .48vw;
  padding: 0 .7vw;
  border-radius: .55vw;
  background: #fff;
  color: #080808;
  text-decoration: none;
  transition: background .3s ease, color .3s ease, transform .3s ease;
}

.vehicle-card__testdrive > span:last-child {
  font-size: .52vw;
  font-weight: 750;
  letter-spacing: .11em;
  text-transform: uppercase;
}

.vehicle-card__testdrive:hover {
  background: var(--mg-red);
  color: #fff;
  transform: translateY(-.06vw);
}

.vehicle-card__icon {
  width: .95vw;
  height: .95vw;
  display: grid;
  place-items: center;
}

.vehicle-card__icon svg {
  width: 100%;
  height: 100%;
}

.vehicle-card__arrow {
  flex: 0 0 2.42vw;
  width: 2.42vw;
  height: 2.42vw;
  display: grid;
  place-items: center;
  border: 1px solid rgba(255, 255, 255, .16);
  border-radius: .55vw;
  background: rgba(255, 255, 255, .07);
  color: #fff;
  text-decoration: none;
  transition: background .3s ease, transform .3s ease;
}

.vehicle-card__arrow svg {
  width: .84vw;
}

.vehicle-card__arrow:hover {
  background: rgba(255, 255, 255, .14);
  transform: translateX(.1vw);
}

@keyframes cardReveal {
  from {
    opacity: 0;
    transform: translateY(1.4vw);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* TABLET */

@media (max-width: 1000px) {
  .mg-hero__container {
    width: calc(100% - 4vw);
  }

  .mg-hero__intro {
    top: 9vw;
    width: 55vw;
  }

  .mg-hero__label {
    font-size: .65vw;
  }

  .mg-hero__title {
    font-size: 5.5vw;
  }

  .mg-hero__tagline {
    font-size: 1.2vw;
  }

  .vehicle-card {
    width: 21vw;
  }

  .vehicle-card__meta strong {
    font-size: 1.5vw;
  }

  .vehicle-card__testdrive,
  .vehicle-card__arrow {
    height: 3.2vw;
  }

  .vehicle-card__arrow {
    flex-basis: 3.2vw;
    width: 3.2vw;
  }

  .vehicle-card__testdrive > span:last-child {
    font-size: .7vw;
  }
}

/* MOBILE */

@media (max-width: 767px) {
  .mg-hero {
    min-height: 680px;
  }

  .mg-hero__video iframe {
    min-width: 190vh;
  }

  .mg-hero__bottom-shade {
    background: linear-gradient(
      0deg,
      rgba(0, 0, 0, .62) 0%,
      rgba(0, 0, 0, .11) 44%,
      transparent 66%
    );
  }

  .mg-hero__container {
    width: calc(100% - 30px);
  }

  .mg-hero__intro {
    top: 21vw;
    width: calc(100% - 10px);
  }

  .mg-hero__label {
    margin-bottom: 1.6vw;
    font-size: 1.8vw;
  }

  .mg-hero__title {
    font-size: 14vw;
  }

  .mg-hero__tagline {
    margin-top: 1.6vw;
    font-size: 3.4vw;
  }

  .vehicle-card {
    right: 0;
    bottom: 18px;
    width: min(62vw, 245px);
    padding: 5px;
    border-radius: 13px;
  }

  .vehicle-card__image {
    border-radius: 9px;
  }

  .vehicle-card__meta {
    left: 10px;
    bottom: 8px;
  }

  .vehicle-card__meta span {
    margin-bottom: 2px;
    font-size: 5px;
  }

  .vehicle-card__meta strong {
    font-size: 17px;
  }

  .vehicle-card__bottom {
    gap: 5px;
    padding-top: 5px;
  }

  .vehicle-card__testdrive {
    height: 36px;
    gap: 7px;
    border-radius: 8px;
  }

  .vehicle-card__testdrive > span:last-child {
    font-size: 7px;
  }

  .vehicle-card__icon {
    width: 16px;
    height: 16px;
  }

  .vehicle-card__arrow {
    flex-basis: 36px;
    width: 36px;
    height: 36px;
    border-radius: 8px;
  }

  .vehicle-card__arrow svg {
    width: 14px;
  }
}

@media (max-width: 420px) {
  .mg-hero__intro {
    top: 20vw;
  }

  .vehicle-card {
    width: 220px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .mg-hero *,
  .mg-hero *::before,
  .mg-hero *::after {
    animation-duration: .01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: .01ms !important;
  }
}
</style>

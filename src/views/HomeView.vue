<script setup>
import { ref } from "vue";

import MgPreloader from "../components/MgPreloader.vue";
import MgHero from "../components/MgHero.vue";
import MgVehicleShowcase from "../components/MgVehicleShowcase.vue";
import MgAboutStory from "../components/MgAboutStory.vue";
import MgEditorialShowcase from "../components/MgEditorialShowcase.vue";
import MgDesignGallery from "../components/MgDesignGallery.vue";
import MgCinematic3DStage from "../components/MgCinematic3DStage.vue";
import MgOwnerStories from "../components/MgOwnerStories.vue";
import MgDarkBrandStory from "../components/MgDarkBrandStory.vue";
import MgRangeSection from "../components/MgRangeSection.vue";
import MgCinematicBanner from "../components/MgCinematicBanner.vue";
import MgShowroomFooter from "../components/MgShowroomFooter.vue";

// import MgScrollStory from "../components/MgScrollStory.vue";

/* =========================================================
   PRELOADER → 3D GATE

   model-viewer is registered by main.js, but the heavy GLB
   and cinematic 3D section are not mounted until the cockpit
   preloader has completely finished.
========================================================= */

const preloaderFinished = ref(false);

const handlePreloaderFinished = () => {
  preloaderFinished.value = true;
};
</script>

<template>
  <MgPreloader @finished="handlePreloaderFinished" />

  <main class="mg-page">
    <!-- 01 — Brand / campaign opening -->
    <MgHero />

    <!-- 02 — Immediate product discovery -->
    <MgVehicleShowcase />

    <!-- 03 — Engineering / driving story -->
    <MgAboutStory />

    <!-- 04 — Design and craftsmanship -->
    <MgEditorialShowcase />

    <!-- 05 — Dark brand statement / tonal reset -->
    <MgDarkBrandStory />

    <!-- 06 — Detail-led gallery -->
    <MgDesignGallery />

    <!-- 07 — Interactive product immersion -->
    <MgCinematic3DStage v-if="preloaderFinished" />

    <!-- 08 — Real-world confidence / social proof -->
    <MgOwnerStories />

    <!-- 09 — Conversion-oriented model selection -->
    <div id="mg-range">
      <MgRangeSection />
    </div>

    <!-- 10 — Emotional campaign close -->
    <MgCinematicBanner />

    <!-- 11 — Showroom / contact / footer -->
    <MgShowroomFooter />

    <!-- <MgScrollStory /> -->
  </main>
</template>

<style scoped>
.mg-page {
  width: 100%;
  overflow: hidden;
}
</style>

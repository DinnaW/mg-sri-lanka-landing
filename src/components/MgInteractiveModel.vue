<script setup>
import {
  computed,
  nextTick,
  onBeforeUnmount,
  onMounted,
  ref,
} from "vue";

/* =========================================================
   MODEL

   This component itself is mounted only after MgPreloader
   finishes, so assigning this src is safe and reliable.
========================================================= */

const modelPath = `${import.meta.env.BASE_URL}models/mg-car.glb`;
const modelViewer = ref(null);
const modelLoaded = ref(false);
const modelLoading = ref(true);
const modelError = ref(false);

/* =========================================================
   UI STATE
========================================================= */

const selectedColorId = ref("silver");
const activeFeatureId = ref("overview");
const useFilterFallback = ref(false);

/* =========================================================
   LOCAL CINEMATIC MG IMAGES

   Stored inside public/images/mg-hmi so the feature cards no
   longer depend on external image URLs at runtime.
========================================================= */

const asset = (path) =>
  `${import.meta.env.BASE_URL}${path.replace(/^\/+/, "")}`;

const MG_IMAGES = {
  overview: asset("images/mg-hmi/overview.jpg"),
  lighting: asset("images/mg-hmi/lighting.jpg"),
  comfort: asset("images/mg-hmi/comfort.jpg"),
  performance: asset("images/mg-hmi/performance.jpg"),
  technology: asset("images/mg-hmi/technology.jpg"),
};

/* =========================================================
   EXTERIOR COLOURS
========================================================= */

const colours = [
  {
    id: "white",
    name: "Pearl White",
    hex: "#ebeae5",
    filter: "brightness(1.08) saturate(.28)",
  },
  {
    id: "red",
    name: "Dynamic Red",
    hex: "#b71720",
    filter:
      "sepia(.48) saturate(4.4) hue-rotate(325deg) brightness(.82)",
  },
  {
    id: "black",
    name: "Black Pearl",
    hex: "#111111",
    filter: "brightness(.32) saturate(.55)",
  },
  {
    id: "silver",
    name: "Sterling Silver",
    hex: "#bec2c1",
    filter: "grayscale(.78) saturate(.26) brightness(.96)",
  },
  {
    id: "grey",
    name: "Camden Grey",
    hex: "#747a78",
    filter: "grayscale(.7) saturate(.35) brightness(.68)",
  },
  {
    id: "blue",
    name: "Dynamic Blue",
    hex: "#405872",
    filter: "sepia(.18) saturate(1.6) hue-rotate(170deg) brightness(.68)",
  },
];

/* =========================================================
   FEATURE CARDS
========================================================= */

const features = [
  {
    id: "overview",
    index: "00",
    eyebrow: "Interactive exterior",
    title: "Explore every angle",
    description:
      "Discover the MG from every perspective. Rotate the vehicle to explore its proportions, surfaces and distinctive design.",
    meta: "360° exterior",
    image: MG_IMAGES.overview,
    imagePosition: "50% 52%",
    camera: "32deg 82deg 60%",
  },
  {
    id: "lighting",
    index: "01",
    eyebrow: "Signature lighting",
    title: "See further",
    description:
      "Distinctive LED lighting combines confident road presence with clear visibility and unmistakable MG character.",
    meta: "Intelligent LED",
    image: MG_IMAGES.lighting,
    imagePosition: "50% 50%",
    camera: "19deg 82deg 57%",
  },
  {
    id: "comfort",
    index: "02",
    eyebrow: "Interior experience",
    title: "Made around you",
    description:
      "A spacious and refined cabin brings together thoughtful materials, driver-focused ergonomics and everyday comfort.",
    meta: "Premium cabin",
    image: MG_IMAGES.comfort,
    imagePosition: "50% 50%",
    camera: "-18deg 79deg 59%",
  },
  {
    id: "performance",
    index: "03",
    eyebrow: "Electric performance",
    title: "Instant response",
    description:
      "Responsive electric power delivery and confident chassis dynamics create effortless performance for every journey.",
    meta: "Electric drive",
    image: MG_IMAGES.performance,
    imagePosition: "50% 52%",
    camera: "145deg 82deg 60%",
  },
  {
    id: "technology",
    index: "04",
    eyebrow: "Connected technology",
    title: "Everything in reach",
    description:
      "Digital displays and connected systems place useful information exactly where the driver needs it.",
    meta: "Smart cockpit",
    image: MG_IMAGES.technology,
    imagePosition: "50% 50%",
    camera: "-35deg 78deg 58%",
  },
];

/* =========================================================
   COMPUTED
========================================================= */

const selectedColour = computed(() =>
  colours.find((colour) => colour.id === selectedColorId.value) || colours[0]
);

const activeFeature = computed(() =>
  features.find((feature) => feature.id === activeFeatureId.value) ||
  features[0]
);

const currentCamera = computed(() => activeFeature.value.camera);
const shouldRotate = computed(() => activeFeatureId.value === "overview");

/* =========================================================
   COLOUR MATERIAL UTILITIES
========================================================= */

const hexToFactor = (hex) => {
  const clean = hex.replace("#", "");

  return [
    parseInt(clean.slice(0, 2), 16) / 255,
    parseInt(clean.slice(2, 4), 16) / 255,
    parseInt(clean.slice(4, 6), 16) / 255,
    1,
  ];
};

const getBodyMaterials = () => {
  const viewer = modelViewer.value;

  if (!viewer?.model) return [];

  const materials = viewer.model.materials || [];
  const keywords = [
    "carpaint",
    "bodypaint",
    "paint",
    "body",
    "exterior",
    "shell",
  ];

  return materials.filter((material) => {
    const name = (material.name || "").toLowerCase();
    return keywords.some((keyword) => name.includes(keyword));
  });
};

const applyVehicleColour = async (colour) => {
  selectedColorId.value = colour.id;

  await nextTick();

  if (!modelLoaded.value) return;

  const bodyMaterials = getBodyMaterials();

  if (bodyMaterials.length) {
    try {
      const factor = hexToFactor(colour.hex);

      bodyMaterials.forEach((material) => {
        material.pbrMetallicRoughness?.setBaseColorFactor(factor);
      });

      useFilterFallback.value = false;
      return;
    } catch (error) {
      console.warn("MG body colour update failed:", error);
    }
  }

  /*
   * The supplied GLB contains a material named "carpaint",
   * so normally this fallback will not be used. It remains as
   * a safety net if the model is replaced later.
   */
  useFilterFallback.value = true;
};

const modelFilterStyle = computed(() => {
  if (!useFilterFallback.value) return {};

  return {
    filter: selectedColour.value.filter,
  };
});

/* =========================================================
   FEATURE CONTROLS
========================================================= */

const selectFeature = (feature) => {
  activeFeatureId.value = feature.id;
};

const showOverview = () => {
  activeFeatureId.value = "overview";
};

/* =========================================================
   MODEL EVENTS
========================================================= */

const handleModelLoad = async () => {
  modelLoaded.value = true;
  modelLoading.value = false;
  modelError.value = false;

  await nextTick();
  await applyVehicleColour(selectedColour.value);
};

const handleModelError = (event) => {
  modelLoading.value = false;
  modelError.value = true;

  console.error("MG 3D model failed to load:", event);
};

const handleImageError = (event) => {
  event.currentTarget?.classList.add("is-missing");
};

/* =========================================================
   KEYBOARD
========================================================= */

const handleKeyDown = (event) => {
  if (event.key === "Escape") showOverview();
};

onMounted(() => {
  window.addEventListener("keydown", handleKeyDown);
});

onBeforeUnmount(() => {
  window.removeEventListener("keydown", handleKeyDown);
});
</script>

<template>
  <section class="mg-hmi">
    <div class="mg-hmi__scene">
      <!-- =================================================
           CINEMATIC ENVIRONMENT
      ================================================== -->
      <div class="mg-hmi__sky"></div>
      <div class="mg-hmi__sun"></div>
      <div class="mg-hmi__mountains mg-hmi__mountains--back"></div>
      <div class="mg-hmi__mountains mg-hmi__mountains--front"></div>
      <div class="mg-hmi__ground"></div>
      <div class="mg-hmi__mist"></div>

      <!-- =================================================
           TOP RIGHT
      ================================================== -->
      <div class="mg-hmi__top-actions">
        <div>
          <span>100% ELECTRIC</span>
          <strong>MG</strong>
        </div>

        <button
          type="button"
          class="mg-hmi__overview-button"
          @click="showOverview"
        >
          <svg viewBox="0 0 24 24" aria-hidden="true">
            <path
              d="M4 12a8 8 0 1 0 2.3-5.6M4 5v4h4"
              fill="none"
              stroke="currentColor"
              stroke-width="1.4"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
          360° VIEW
        </button>
      </div>

      <!-- =================================================
           ACTIVE FEATURE COPY
      ================================================== -->
      <Transition name="feature-copy" mode="out-in">
        <div :key="activeFeature.id" class="mg-hmi__feature-copy">
          <span>{{ activeFeature.eyebrow }}</span>
          <h2>{{ activeFeature.title }}</h2>
          <p>{{ activeFeature.description }}</p>

          <div class="mg-hmi__feature-meta">
            <i></i>
            <span>{{ activeFeature.meta }}</span>
          </div>
        </div>
      </Transition>

      <!-- =================================================
           SMALL VEHICLE HUD
      ================================================== -->
      <div class="mg-hmi__hud">
        <div>
          <span>RANGE</span>
          <strong>480</strong>
          <small>KM</small>
        </div>

        <i></i>

        <div>
          <span>DRIVE</span>
          <strong>EV</strong>
          <small>MODE</small>
        </div>
      </div>

      <!-- =================================================
           3D VEHICLE

           Direct src is intentional. The entire component is
           mounted by HomeView only after the preloader ends.
      ================================================== -->
      <div class="mg-hmi__vehicle-zone">
        <model-viewer
          ref="modelViewer"
          class="mg-hmi__model"
          :class="{
            'mg-hmi__model--loaded': modelLoaded,
          }"
          :style="modelFilterStyle"
          :src="modelPath"
          alt="Interactive MG electric vehicle"
          loading="eager"
          reveal="auto"
          camera-controls
          :auto-rotate="shouldRotate"
          auto-rotate-delay="1000"
          rotation-per-second="1.05deg"
          disable-zoom
          disable-pan
          interaction-prompt="none"
          :camera-orbit="currentCamera"
          min-camera-orbit="auto 75deg 56%"
          max-camera-orbit="auto 86deg 63%"
          camera-target="auto auto auto"
          field-of-view="20deg"
          min-field-of-view="20deg"
          max-field-of-view="20deg"
          interpolation-decay="85"
          orbit-sensitivity="0.65"
          shadow-intensity="1.35"
          shadow-softness="0.8"
          exposure="1.08"
          environment-image="neutral"
          touch-action="pan-y"
          @load="handleModelLoad"
          @error="handleModelError"
        ></model-viewer>

        <div class="mg-hmi__vehicle-shadow"></div>

        <Transition name="model-loader">
          <div
            v-if="modelLoading && !modelLoaded && !modelError"
            class="mg-hmi__loader"
          >
            <div class="mg-hmi__loader-ring">
              <span></span>
              <i></i>
            </div>
            <strong>Preparing your MG</strong>
            <small>Loading interactive vehicle</small>
          </div>
        </Transition>

        <div v-if="modelError" class="mg-hmi__error">
          <strong>Vehicle preview unavailable</strong>
          <span>Check public/models/mg-car.glb</span>
        </div>
      </div>

      <!-- =================================================
           VERTICAL COLOUR SELECTOR
      ================================================== -->
      <div class="mg-hmi__colour-selector">
        <div class="mg-hmi__colour-info">
          <span>EXTERIOR</span>
          <strong>{{ selectedColour.name }}</strong>
        </div>

        <div class="mg-hmi__colour-list">
          <button
            v-for="colour in colours"
            :key="colour.id"
            type="button"
            class="mg-colour"
            :class="{
              'mg-colour--active': selectedColorId === colour.id,
            }"
            :aria-label="colour.name"
            :title="colour.name"
            @click="applyVehicleColour(colour)"
          >
            <span :style="{ background: colour.hex }"></span>
          </button>
        </div>

        <span class="mg-hmi__colour-index">
          {{ String(colours.findIndex((c) => c.id === selectedColorId) + 1).padStart(2, "0") }}
        </span>
      </div>

      <!-- =================================================
           DRAG INDICATOR
      ================================================== -->
      <Transition name="drag">
        <div v-if="modelLoaded" class="mg-hmi__drag">
          <svg viewBox="0 0 24 24" aria-hidden="true">
            <path
              d="M4 12a8 8 0 0 1 13.7-5.6L20 8.7M20 4v4.7h-4.7M20 12a8 8 0 0 1-13.7 5.6L4 15.3M4 20v-4.7h4.7"
              fill="none"
              stroke="currentColor"
              stroke-width="1.3"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
          <span>Drag to explore</span>
        </div>
      </Transition>

      <!-- =================================================
           BOTTOM HMI FEATURE CARDS
      ================================================== -->
      <div class="mg-console">
        <div class="mg-console__rail">
          <div class="mg-console__temperature">
            <strong>22°</strong>
            <span>AUTO</span>
          </div>

          <span class="mg-console__rail-line"></span>

          <button type="button" aria-label="Vehicle">
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path
                d="M5 15h14M7 15l1.5-5h7L17 15M8 18h0M16 18h0"
                fill="none"
                stroke="currentColor"
                stroke-width="1.4"
                stroke-linecap="round"
              />
            </svg>
          </button>

          <button type="button" aria-label="Vehicle status">
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <circle
                cx="12"
                cy="12"
                r="7"
                fill="none"
                stroke="currentColor"
                stroke-width="1.3"
              />
              <path
                d="M12 8v4l3 2"
                fill="none"
                stroke="currentColor"
                stroke-width="1.3"
                stroke-linecap="round"
              />
            </svg>
          </button>
        </div>

        <div class="mg-console__features">
          <button
            v-for="feature in features"
            :key="feature.id"
            type="button"
            class="mg-feature-card"
            :class="{
              'mg-feature-card--active': activeFeatureId === feature.id,
            }"
            @click="selectFeature(feature)"
          >
            <img
              class="mg-feature-card__image"
              :src="feature.image"
              :alt="feature.title"
              :style="{
                objectPosition: feature.imagePosition,
              }"
              loading="lazy"
              decoding="async"
              @error="handleImageError"
            />

            <div class="mg-feature-card__overlay"></div>
            <div class="mg-feature-card__glow"></div>

            <div class="mg-feature-card__top">
              <span>{{ feature.index }}</span>
              <span class="mg-feature-card__dot"><i></i></span>
            </div>

            <div class="mg-feature-card__content">
              <span>{{ feature.eyebrow }}</span>
              <strong>{{ feature.title }}</strong>
              <small>{{ feature.meta }}</small>
            </div>

            <span class="mg-feature-card__line"></span>
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;500;600&family=Manrope:wght@400;500;600;700&display=swap");

/* =========================================================
   ROOT
========================================================= */

.mg-hmi {
  --red: #e51920;
  position: relative;
  width: 100%;
  overflow: hidden;
  background: #e5e9e8;
  color: #111;
  font-family: "Manrope", sans-serif;
}

/* =========================================================
   SCENE
========================================================= */

.mg-hmi__scene {
  position: relative;
  width: 100%;
  min-height: clamp(760px, 58vw, 970px);
  overflow: hidden;
  isolation: isolate;
  background: linear-gradient(
    180deg,
    #dae4ea 0%,
    #eef1ee 42%,
    #e5e7e2 68%,
    #d5d6d0 100%
  );
}

/* =========================================================
   ENVIRONMENT
========================================================= */

.mg-hmi__sky {
  position: absolute;
  inset: 0 0 35%;
  z-index: -10;
  background:
    radial-gradient(
      ellipse at 50% 15%,
      rgba(255, 255, 255, 0.98),
      rgba(255, 255, 255, 0.3) 35%,
      transparent 67%
    ),
    linear-gradient(180deg, #d9e3e9, #edf0ec);
}

.mg-hmi__sky::after {
  content: "";
  position: absolute;
  inset: 20% -10% 0;
  opacity: 0.5;
  filter: blur(26px);
  background:
    radial-gradient(
      ellipse at 12% 42%,
      rgba(255, 255, 255, 0.8) 0 7%,
      transparent 17%
    ),
    radial-gradient(
      ellipse at 32% 34%,
      rgba(255, 255, 255, 0.72) 0 8%,
      transparent 17%
    ),
    radial-gradient(
      ellipse at 70% 36%,
      rgba(255, 255, 255, 0.72) 0 8%,
      transparent 18%
    ),
    radial-gradient(
      ellipse at 88% 42%,
      rgba(255, 255, 255, 0.82) 0 7%,
      transparent 17%
    );
}

.mg-hmi__sun {
  position: absolute;
  left: 50%;
  top: 3%;
  z-index: -9;
  width: 35vw;
  height: 29vw;
  transform: translateX(-50%);
  border-radius: 50%;
  opacity: 0.75;
  filter: blur(45px);
  background: radial-gradient(
    circle,
    rgba(255, 252, 238, 0.98),
    rgba(255, 243, 206, 0.19) 46%,
    transparent 72%
  );
}

.mg-hmi__mountains {
  position: absolute;
  left: -5%;
  width: 110%;
  z-index: -7;
  clip-path: polygon(
    0 73%,
    9% 51%,
    16% 62%,
    25% 40%,
    34% 62%,
    45% 47%,
    54% 69%,
    66% 46%,
    73% 64%,
    84% 42%,
    92% 61%,
    100% 47%,
    100% 100%,
    0 100%
  );
}

.mg-hmi__mountains--back {
  bottom: 29%;
  height: 25%;
  opacity: 0.22;
  background: #8f9fa2;
  filter: blur(3px);
}

.mg-hmi__mountains--front {
  bottom: 23%;
  height: 22%;
  opacity: 0.13;
  transform: scaleX(-1);
  background: #6d7c7d;
}

.mg-hmi__ground {
  position: absolute;
  inset: 57% -10% 0;
  z-index: -6;
  background: radial-gradient(
    ellipse at 50% 28%,
    rgba(255, 255, 255, 0.94),
    rgba(246, 247, 241, 0.74) 43%,
    #d4d8d1 100%
  );
}

.mg-hmi__ground::after {
  content: "";
  position: absolute;
  inset: 0;
  opacity: 0.2;
  background: repeating-linear-gradient(
    174deg,
    transparent 0 27px,
    rgba(255, 255, 255, 0.6) 28px,
    transparent 30px
  );
}

.mg-hmi__mist {
  position: absolute;
  left: -10%;
  right: -10%;
  top: 40%;
  height: 36%;
  z-index: 2;
  opacity: 0.32;
  filter: blur(18px);
  pointer-events: none;
  background: linear-gradient(
    180deg,
    transparent,
    rgba(255, 255, 255, 0.76) 48%,
    transparent
  );
}

/* =========================================================
   TOP RIGHT
========================================================= */

.mg-hmi__top-actions {
  position: absolute;
  right: 4.5%;
  top: clamp(28px, 3vw, 48px);
  z-index: 50;
  display: flex;
  align-items: center;
  gap: 25px;
}

.mg-hmi__top-actions > div {
  text-align: right;
}

.mg-hmi__top-actions > div span {
  display: block;
  margin-bottom: 2px;
  color: var(--red);
  font-size: 6px;
  font-weight: 700;
  letter-spacing: 0.16em;
}

.mg-hmi__top-actions > div strong {
  font-family: "Barlow Condensed", sans-serif;
  font-size: 25px;
  font-weight: 500;
}

.mg-hmi__overview-button {
  height: 35px;
  padding: 0 14px;
  display: flex;
  align-items: center;
  gap: 7px;
  border: 1px solid rgba(0, 0, 0, 0.15);
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.32);
  backdrop-filter: blur(13px);
  -webkit-backdrop-filter: blur(13px);
  color: #111;
  font-family: "Manrope", sans-serif;
  font-size: 6px;
  font-weight: 700;
  letter-spacing: 0.13em;
  cursor: pointer;
  transition:
    background 0.3s ease,
    color 0.3s ease,
    transform 0.3s ease;
}

.mg-hmi__overview-button svg {
  width: 13px;
  height: 13px;
}

.mg-hmi__overview-button:hover {
  background: #111;
  color: #fff;
  transform: translateY(-1px);
}

/* =========================================================
   FEATURE COPY
========================================================= */

.mg-hmi__feature-copy {
  position: absolute;
  left: 4.5%;
  top: 14%;
  z-index: 35;
  width: min(270px, 22vw);
}

.mg-hmi__feature-copy > span {
  display: block;
  margin-bottom: 8px;
  color: var(--red);
  font-size: 6px;
  font-weight: 700;
  letter-spacing: 0.17em;
  text-transform: uppercase;
}

.mg-hmi__feature-copy h2 {
  margin: 0;
  font-size: clamp(24px, 2.15vw, 36px);
  font-weight: 600;
  line-height: 1;
  letter-spacing: -0.045em;
}

.mg-hmi__feature-copy p {
  max-width: 245px;
  margin: 13px 0 0;
  color: rgba(0, 0, 0, 0.5);
  font-size: 9px;
  line-height: 1.72;
}

.mg-hmi__feature-meta {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 17px;
}

.mg-hmi__feature-meta i {
  width: 28px;
  height: 1px;
  background: rgba(0, 0, 0, 0.34);
}

.mg-hmi__feature-meta span {
  font-size: 6px;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(0, 0, 0, 0.38);
}

.feature-copy-enter-active,
.feature-copy-leave-active {
  transition:
    opacity 0.25s ease,
    transform 0.45s cubic-bezier(0.16, 1, 0.3, 1);
}

.feature-copy-enter-from {
  opacity: 0;
  transform: translateY(12px);
}

.feature-copy-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}

/* =========================================================
   HUD
========================================================= */

.mg-hmi__hud {
  position: absolute;
  right: 4.5%;
  top: 17%;
  z-index: 35;
  display: flex;
  align-items: center;
  gap: 18px;
  padding: 11px 15px;
  border: 1px solid rgba(255, 255, 255, 0.57);
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.25);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
}

.mg-hmi__hud > div {
  min-width: 43px;
  text-align: center;
}

.mg-hmi__hud span {
  display: block;
  margin-bottom: 3px;
  font-size: 5px;
  font-weight: 700;
  letter-spacing: 0.13em;
  color: rgba(0, 0, 0, 0.34);
}

.mg-hmi__hud strong {
  font-family: "Barlow Condensed", sans-serif;
  font-size: 20px;
  font-weight: 500;
}

.mg-hmi__hud small {
  margin-left: 2px;
  font-size: 5px;
  color: rgba(0, 0, 0, 0.4);
}

.mg-hmi__hud > i {
  width: 1px;
  height: 28px;
  background: rgba(0, 0, 0, 0.1);
}

/* =========================================================
   3D VEHICLE
========================================================= */

.mg-hmi__vehicle-zone {
  position: absolute;
  left: 15%;
  top: 3%;
  z-index: 10;
  width: 71%;
  height: 69%;
}

.mg-hmi__model {
  position: absolute;
  inset: 0;
  z-index: 10;
  display: block;
  width: 100%;
  height: 100%;
  opacity: 0;
  transform: translateY(10px) scale(0.985);
  background: transparent;
  --poster-color: transparent;
  transition:
    opacity 0.7s ease,
    transform 1s cubic-bezier(0.16, 1, 0.3, 1),
    filter 0.35s ease;
}

.mg-hmi__model--loaded {
  opacity: 1;
  transform: translateY(0) scale(1);
}

.mg-hmi__vehicle-shadow {
  position: absolute;
  left: 50%;
  top: 79%;
  z-index: 3;
  width: 52%;
  height: 6%;
  transform: translateX(-50%);
  border-radius: 50%;
  background: rgba(0, 0, 0, 0.34);
  opacity: 0.45;
  filter: blur(27px);
}

/* =========================================================
   MODEL LOADER / ERROR
========================================================= */

.mg-hmi__loader,
.mg-hmi__error {
  position: absolute;
  left: 50%;
  top: 51%;
  z-index: 30;
  transform: translate(-50%, -50%);
  text-align: center;
}

.mg-hmi__loader-ring {
  position: relative;
  width: 46px;
  height: 46px;
  margin: 0 auto 12px;
  border: 1px solid rgba(0, 0, 0, 0.1);
  border-radius: 50%;
}

.mg-hmi__loader-ring span {
  position: absolute;
  inset: -1px;
  border: 1px solid transparent;
  border-top-color: var(--red);
  border-radius: 50%;
  animation: modelSpin 1.1s linear infinite;
}

.mg-hmi__loader-ring i {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 4px;
  height: 4px;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  background: var(--red);
}

@keyframes modelSpin {
  to {
    transform: rotate(360deg);
  }
}

.mg-hmi__loader strong,
.mg-hmi__error strong {
  display: block;
  font-size: 9px;
  font-weight: 600;
}

.mg-hmi__loader small,
.mg-hmi__error span {
  display: block;
  margin-top: 4px;
  font-size: 6px;
  color: rgba(0, 0, 0, 0.4);
}

.model-loader-leave-active {
  transition: opacity 0.3s ease;
}

.model-loader-leave-to {
  opacity: 0;
}

/* =========================================================
   VERTICAL COLOUR SELECTOR
========================================================= */

.mg-hmi__colour-selector {
  position: absolute;
  right: 4.7%;
  top: 39%;
  z-index: 46;
  width: 92px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  padding: 15px 10px 12px;
  box-sizing: border-box;
  border: 1px solid rgba(255, 255, 255, 0.7);
  border-radius: 18px;
  background: rgba(242, 245, 243, 0.57);
  backdrop-filter: blur(22px) saturate(1.1);
  -webkit-backdrop-filter: blur(22px) saturate(1.1);
  box-shadow:
    0 15px 35px rgba(32, 42, 44, 0.1),
    inset 0 1px rgba(255, 255, 255, 0.82);
}

.mg-hmi__colour-info {
  width: 100%;
  text-align: center;
}

.mg-hmi__colour-info span {
  display: block;
  margin-bottom: 4px;
  font-size: 5px;
  font-weight: 700;
  letter-spacing: 0.14em;
  color: rgba(0, 0, 0, 0.36);
}

.mg-hmi__colour-info strong {
  display: block;
  overflow: hidden;
  font-size: 7px;
  font-weight: 600;
  line-height: 1.25;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.mg-hmi__colour-list {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 7px;
  padding: 3px 0;
}

.mg-hmi__colour-list::before {
  content: "";
  position: absolute;
  left: 50%;
  top: 0;
  bottom: 0;
  z-index: -1;
  width: 1px;
  transform: translateX(-50%);
  background: linear-gradient(
    transparent,
    rgba(0, 0, 0, 0.08) 20%,
    rgba(0, 0, 0, 0.08) 80%,
    transparent
  );
}

.mg-colour {
  position: relative;
  width: 29px;
  height: 29px;
  flex: 0 0 29px;
  padding: 0;
  border: 1px solid transparent;
  border-radius: 50%;
  background: rgba(248, 250, 248, 0.72);
  cursor: pointer;
  transition:
    transform 0.25s ease,
    border-color 0.25s ease,
    box-shadow 0.25s ease;
}

.mg-colour > span {
  position: absolute;
  inset: 4px;
  border: 1px solid rgba(0, 0, 0, 0.13);
  border-radius: 50%;
  box-shadow:
    inset 0 2px 4px rgba(255, 255, 255, 0.62),
    0 3px 7px rgba(0, 0, 0, 0.09);
}

.mg-colour:hover {
  transform: scale(1.1);
}

.mg-colour--active {
  border-color: rgba(0, 0, 0, 0.45);
  transform: scale(1.1);
  box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.38);
}

.mg-colour--active::after {
  content: "";
  position: absolute;
  left: -9px;
  top: 50%;
  width: 4px;
  height: 4px;
  transform: translateY(-50%);
  border-radius: 50%;
  background: var(--red);
  box-shadow: 0 0 7px rgba(229, 25, 32, 0.45);
}

.mg-hmi__colour-index {
  font-family: "Barlow Condensed", sans-serif;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.08em;
  color: rgba(0, 0, 0, 0.34);
}

/* =========================================================
   DRAG
========================================================= */

.mg-hmi__drag {
  position: absolute;
  left: 50%;
  top: 64%;
  z-index: 30;
  display: flex;
  align-items: center;
  gap: 6px;
  transform: translateX(-50%);
  color: rgba(0, 0, 0, 0.36);
  pointer-events: none;
}

.mg-hmi__drag svg {
  width: 14px;
  height: 14px;
}

.mg-hmi__drag span {
  font-size: 6px;
  font-weight: 600;
  letter-spacing: 0.07em;
}

.drag-enter-active,
.drag-leave-active {
  transition: opacity 0.35s ease;
}

.drag-enter-from,
.drag-leave-to {
  opacity: 0;
}

/* =========================================================
   BOTTOM HMI CONSOLE
========================================================= */

.mg-console {
  position: absolute;
  left: 2.3%;
  right: 2.3%;
  bottom: 2.5%;
  z-index: 80;
  height: clamp(190px, 15vw, 230px);
  display: grid;
  grid-template-columns: 54px minmax(0, 1fr);
  gap: 10px;
  padding: 10px;
  box-sizing: border-box;
  border: 1px solid rgba(255, 255, 255, 0.64);
  border-radius: 18px;
  background: rgba(239, 243, 242, 0.68);
  backdrop-filter: blur(28px) saturate(1.15);
  -webkit-backdrop-filter: blur(28px) saturate(1.15);
  box-shadow:
    0 27px 70px rgba(31, 40, 42, 0.15),
    inset 0 1px rgba(255, 255, 255, 0.78);
}

.mg-console__rail {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  padding: 10px 5px;
  border-radius: 13px;
  background: rgba(255, 255, 255, 0.28);
}

.mg-console__temperature {
  text-align: center;
}

.mg-console__temperature strong {
  display: block;
  font-family: "Barlow Condensed", sans-serif;
  font-size: 21px;
  font-weight: 500;
}

.mg-console__temperature span {
  display: block;
  font-size: 5px;
  font-weight: 700;
  letter-spacing: 0.11em;
  color: rgba(0, 0, 0, 0.38);
}

.mg-console__rail-line {
  width: 1px;
  height: 29px;
  background: linear-gradient(
    transparent,
    rgba(0, 0, 0, 0.15),
    transparent
  );
}

.mg-console__rail button {
  width: 31px;
  height: 31px;
  display: grid;
  place-items: center;
  padding: 0;
  border: 0;
  border-radius: 9px;
  background: transparent;
  color: rgba(0, 0, 0, 0.55);
  cursor: pointer;
  transition: background 0.25s ease;
}

.mg-console__rail button:hover {
  background: rgba(255, 255, 255, 0.6);
}

.mg-console__rail svg {
  width: 16px;
  height: 16px;
}

/* =========================================================
   FEATURE CARDS
========================================================= */

.mg-console__features {
  display: grid;
  grid-template-columns: repeat(5, minmax(0, 1fr));
  gap: 9px;
  min-width: 0;
  overflow: hidden;
}

.mg-feature-card {
  position: relative;
  min-width: 0;
  overflow: hidden;
  padding: 0;
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 13px;
  background: #22272a;
  color: #fff;
  text-align: left;
  isolation: isolate;
  cursor: pointer;
  transition:
    transform 0.45s cubic-bezier(0.16, 1, 0.3, 1),
    box-shadow 0.4s ease;
}

.mg-feature-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 16px 32px rgba(20, 27, 30, 0.18);
}

.mg-feature-card__image {
  position: absolute;
  inset: 0;
  z-index: -4;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transform: scale(1.015);
  filter: saturate(0.92) contrast(1.05);
  transition:
    transform 1.05s cubic-bezier(0.16, 1, 0.3, 1),
    filter 0.5s ease;
}

.mg-feature-card:hover .mg-feature-card__image {
  transform: scale(1.075);
  filter: saturate(1) contrast(1.05);
}

.mg-feature-card__image.is-missing {
  opacity: 0;
}

.mg-feature-card__overlay {
  position: absolute;
  inset: 0;
  z-index: -3;
  background: linear-gradient(
    180deg,
    rgba(4, 6, 8, 0.12) 0%,
    rgba(4, 6, 8, 0.02) 32%,
    rgba(3, 5, 7, 0.36) 62%,
    rgba(3, 5, 7, 0.9) 100%
  );
}

.mg-feature-card__glow {
  position: absolute;
  left: 50%;
  top: -32%;
  z-index: -2;
  width: 130%;
  height: 70%;
  transform: translateX(-50%);
  border-radius: 50%;
  opacity: 0.17;
  filter: blur(20px);
  background: #fff;
}

.mg-feature-card__top {
  position: absolute;
  left: 11px;
  right: 11px;
  top: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.mg-feature-card__top > span:first-child {
  font-family: "Barlow Condensed", sans-serif;
  font-size: 14px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.66);
}

.mg-feature-card__dot {
  width: 19px;
  height: 19px;
  display: grid;
  place-items: center;
  border: 1px solid rgba(255, 255, 255, 0.39);
  border-radius: 50%;
  backdrop-filter: blur(8px);
}

.mg-feature-card__dot i {
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.75);
}

.mg-feature-card__content {
  position: absolute;
  left: 11px;
  right: 11px;
  bottom: 13px;
}

.mg-feature-card__content > span {
  display: block;
  margin-bottom: 4px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  font-size: 5px;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.57);
}

.mg-feature-card__content strong {
  display: block;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  font-size: 11px;
  font-weight: 600;
}

.mg-feature-card__content small {
  display: block;
  margin-top: 5px;
  font-size: 6px;
  color: rgba(255, 255, 255, 0.52);
}

.mg-feature-card__line {
  position: absolute;
  left: 12px;
  right: calc(100% - 12px);
  bottom: 0;
  height: 2px;
  background: var(--red);
  transition: right 0.5s cubic-bezier(0.16, 1, 0.3, 1);
}

.mg-feature-card--active {
  box-shadow: 0 17px 34px rgba(18, 24, 27, 0.22);
}

.mg-feature-card--active .mg-feature-card__line {
  right: 12px;
}

.mg-feature-card--active .mg-feature-card__dot {
  border-color: rgba(229, 25, 32, 0.7);
}

.mg-feature-card--active .mg-feature-card__dot i {
  background: var(--red);
  box-shadow: 0 0 8px rgba(229, 25, 32, 0.7);
}

/* =========================================================
   LARGE DESKTOP
========================================================= */

@media (min-width: 1800px) {
  .mg-hmi__scene {
    min-height: 54vw;
  }

  .mg-hmi__feature-copy {
    width: 17vw;
  }

  .mg-hmi__feature-copy p {
    font-size: 0.52vw;
  }

  .mg-hmi__vehicle-zone {
    left: 16%;
    width: 69%;
  }

  .mg-hmi__colour-selector {
    right: 5.5%;
    top: 39%;
    width: 5.2vw;
    min-width: 92px;
  }
}

/* =========================================================
   TABLET
========================================================= */

@media (max-width: 1100px) {
  .mg-hmi__scene {
    min-height: 810px;
  }

  .mg-hmi__feature-copy {
    width: 210px;
  }

  .mg-hmi__vehicle-zone {
    left: 7%;
    width: 87%;
    height: 66%;
  }

  .mg-hmi__hud {
    display: none;
  }

  .mg-hmi__colour-selector {
    right: 3%;
    top: 37%;
  }

  .mg-console__features {
    grid-template-columns: none;
    grid-auto-flow: column;
    grid-auto-columns: 170px;
    overflow-x: auto;
    scrollbar-width: none;
  }

  .mg-console__features::-webkit-scrollbar {
    display: none;
  }
}

/* =========================================================
   MOBILE
========================================================= */

@media (max-width: 767px) {
  .mg-hmi__scene {
    min-height: 820px;
  }

  .mg-hmi__top-actions {
    top: 20px;
    right: 15px;
  }

  .mg-hmi__top-actions > div {
    display: none;
  }

  .mg-hmi__overview-button {
    height: 31px;
    padding: 0 11px;
  }

  .mg-hmi__feature-copy {
    left: 15px;
    top: 9%;
    width: min(230px, 66vw);
  }

  .mg-hmi__feature-copy h2 {
    font-size: 27px;
  }

  .mg-hmi__feature-copy p {
    font-size: 8px;
  }

  .mg-hmi__vehicle-zone {
    left: -12%;
    top: 14%;
    width: 124%;
    height: 48%;
  }

  /* Keep colour swatches vertical on mobile as requested. */
  .mg-hmi__colour-selector {
    right: 10px;
    top: 31%;
    width: 64px;
    gap: 9px;
    padding: 11px 7px 9px;
    border-radius: 15px;
  }

  .mg-hmi__colour-info strong {
    font-size: 6px;
  }

  .mg-hmi__colour-list {
    gap: 5px;
  }

  .mg-colour {
    width: 24px;
    height: 24px;
    flex-basis: 24px;
  }

  .mg-colour--active::after {
    left: -7px;
  }

  .mg-hmi__drag {
    top: 60%;
  }

  .mg-console {
    left: 10px;
    right: 10px;
    bottom: 10px;
    height: 225px;
    grid-template-columns: 1fr;
    padding: 8px;
    border-radius: 16px;
  }

  .mg-console__rail {
    display: none;
  }

  .mg-console__features {
    grid-template-columns: none;
    grid-auto-flow: column;
    grid-auto-columns: 145px;
    gap: 7px;
    overflow-x: auto;
  }

  .mg-feature-card__content strong {
    font-size: 10px;
  }
}

/* =========================================================
   SMALL MOBILE
========================================================= */

@media (max-width: 420px) {
  .mg-hmi__scene {
    min-height: 790px;
  }

  .mg-hmi__feature-copy {
    top: 8.5%;
    width: 215px;
  }

  .mg-hmi__feature-copy p {
    max-width: 190px;
  }

  .mg-hmi__vehicle-zone {
    top: 14%;
    height: 43%;
  }

  .mg-hmi__colour-selector {
    top: 29.5%;
    width: 60px;
  }

  .mg-colour {
    width: 22px;
    height: 22px;
    flex-basis: 22px;
  }

  .mg-console {
    height: 215px;
  }
}

/* =========================================================
   REDUCED MOTION
========================================================= */

@media (prefers-reduced-motion: reduce) {
  .mg-hmi *,
  .mg-hmi *::before,
  .mg-hmi *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
</style>

<script setup>
import { ref } from "vue";

const asset = (path) =>
  `${import.meta.env.BASE_URL}${path.replace(/^\/+/, "")}`;


/* =========================================================
   ACTIVE SPEC
========================================================= */

const activeSpec = ref(null);


/* =========================================================
   VEHICLE IMAGE

   Existing MG transparent vehicle image.
========================================================= */

const carImage = asset("images/123.png");


/* =========================================================
   TOP ECO CARDS
========================================================= */

const drivingCards = [
  {
    id: 1,

    label:
      "All-Wheel Electric Drive",

    title:
      "Confident electric performance",

    /*
     * Eco / renewable-energy landscape
     */
    background:
      "https://images.unsplash.com/photo-1466611653911-95081537e5b7?auto=format&fit=crop&w=1800&q=88",

    backgroundPosition:
      "center 48%",

    carPosition:
      "center",

    carClass:
      "eco-car--wind",

    alt:
      "MG electric vehicle in renewable energy landscape",
  },

  {
    id: 2,

    label:
      "Environmental Friendliness",

    title:
      "Electric mobility meets nature",

    /*
     * Green mountain / natural landscape
     */
    background:
      "https://images.unsplash.com/photo-1441974231531-c6227db76b6e?auto=format&fit=crop&w=1800&q=88",

    backgroundPosition:
      "center 58%",

    carPosition:
      "center",

    carClass:
      "eco-car--forest",

    alt:
      "MG electric vehicle in green natural landscape",
  },
];


/* =========================================================
   SPEC DATA
========================================================= */

const specs = [
  {
    id:
      "range",

    value:
      "4630",

    unit:
      "mm",

    description:
      "The zero-emissions crossover is 4630 millimeters long.",

    detail:
      "Balanced proportions give the MG a confident SUV presence while keeping the vehicle practical for everyday electric mobility.",
  },

  {
    id:
      "motor",

    value:
      "2500",

    unit:
      "rpm",

    description:
      "At just 2,500rpm, performance should be unlike most SUVs.",

    detail:
      "Electric power is delivered immediately and smoothly, creating responsive performance without compromising refinement.",

    background:
      "https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?auto=format&fit=crop&w=1600&q=88",

    backgroundPosition:
      "center 58%",
  },

  {
    id:
      "wheelbase",

    value:
      "2830",

    unit:
      "mm",

    description:
      "A compact crossover with a wheelbase stretching to 2830 mm.",

    detail:
      "The long wheelbase helps create more usable cabin space while supporting a stable, planted driving character.",
  },
];


/* =========================================================
   TOGGLE
========================================================= */

const toggleSpec = (id) => {
  activeSpec.value =
    activeSpec.value === id
      ? null
      : id;
};
</script>


<template>
  <section class="mg-dynamics">

    <div class="mg-dynamics__container">


      <!-- =====================================================
           TOP INTRO
      ====================================================== -->

      <header class="mg-dynamics__intro">

        <div class="mg-dynamics__headline">

          <h2>
            Enhanced Driving Dynamics
            <br />
            and Revealed Legacy of the
            <br />
            MG Electric Drive
          </h2>

        </div>


        <div class="mg-dynamics__intro-copy">

          <p>
            Driving dynamics are shaped by a
            low-mounted battery, balanced weight
            distribution and an advanced electric
            platform. The result is an MG that feels
            composed, responsive and ready for every
            journey.
          </p>

        </div>

      </header>



      <!-- =====================================================
           ECO IMAGE CARDS
      ====================================================== -->

      <div class="mg-dynamics__features">

        <article
          v-for="card in drivingCards"
          :key="card.id"
          class="mg-eco-card"
        >


          <!-- =================================================
               CARD TOP
          ================================================== -->

          <div class="mg-eco-card__top">

            <span class="mg-eco-card__pill">
              {{ card.label }}
            </span>


            <button
              type="button"
              class="mg-eco-card__arrow"
              aria-label="Explore feature"
            >

              <svg
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path
                  d="M5 12h14M14 7l5 5-5 5"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="1.5"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>

            </button>

          </div>



          <!-- =================================================
               ECO SCENE
          ================================================== -->

          <div class="mg-eco-card__scene">


            <!-- LANDSCAPE -->

            <img
              class="mg-eco-card__background"
              :src="card.background"
              :style="{
                objectPosition:
                  card.backgroundPosition
              }"
              alt=""
              loading="lazy"
              decoding="async"
            />


            <!-- CINEMATIC GRADIENT -->

            <div class="mg-eco-card__shade"></div>


            <!-- SOFT SUNLIGHT -->

            <div class="mg-eco-card__light"></div>


            <!-- ROAD / GROUND -->

            <div class="mg-eco-card__ground"></div>


            <!-- MG VEHICLE -->

            <img
              class="mg-eco-card__car"
              :class="card.carClass"
              :src="carImage"
              :alt="card.alt"
              loading="lazy"
              decoding="async"
              draggable="false"
            />


            <!-- CAR SHADOW -->

            <div
              class="mg-eco-card__car-shadow"
            ></div>


            <!-- ECO BADGE -->

            <div class="mg-eco-card__eco">

              <span
                class="mg-eco-card__eco-icon"
              >

                <svg
                  viewBox="0 0 24 24"
                  aria-hidden="true"
                >
                  <path
                    d="M19 4c-7 .4-11 3.3-11 8.1 0 2.2 1.5 4 3.7 4.3 4.7.7 7.3-4 7.3-12.4ZM5 20c1.6-4.4 4.3-7.5 8.3-9.5"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="1.4"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>

              </span>

              <span>
                ZERO TAILPIPE EMISSIONS
              </span>

            </div>


            <!-- SMALL COPY -->

            <div
              class="mg-eco-card__caption"
            >

              <span>
                ELECTRIC MOBILITY
              </span>

              <strong>
                {{ card.title }}
              </strong>

            </div>


          </div>

        </article>

      </div>



      <!-- =====================================================
           SPEC INTRO
      ====================================================== -->

      <div class="mg-specs__intro">

        <div>

          <h3>
            Exploring Specifications,
            <br />
            Performance Product
          </h3>

        </div>


        <div>

          <p>
            Let’s talk specs. Efficient electric
            engineering is combined with practical
            proportions, generous interior space and
            performance developed for everyday driving.
          </p>

        </div>

      </div>



      <!-- =====================================================
           SPECIFICATIONS
      ====================================================== -->

      <div class="mg-specs">

        <article
          v-for="spec in specs"
          :key="spec.id"
          class="mg-spec"
          :class="{
            'mg-spec--open':
              activeSpec === spec.id,

            'mg-spec--visual':
              spec.background
          }"
        >


          <!-- =================================================
               PRIMARY SPEC ROW
          ================================================== -->

          <div class="mg-spec__row">


            <!-- PLUS -->

            <button
              type="button"
              class="mg-spec__plus"
              :aria-label="
                `More information about ${spec.value}${spec.unit}`
              "
              @click="
                toggleSpec(spec.id)
              "
            >

              <span></span>

              <span></span>

            </button>



            <!-- NUMBER -->

            <div class="mg-spec__value">

              <strong>
                {{ spec.value }}
              </strong>

              <span>
                {{ spec.unit }}
              </span>

            </div>



            <!-- DESCRIPTION -->

            <div class="mg-spec__description">

              <p>
                {{ spec.description }}
              </p>


              <button
                type="button"
                class="mg-spec__arrow"
                aria-label="View specification"
                @click="
                  toggleSpec(spec.id)
                "
              >

                <svg
                  viewBox="0 0 24 24"
                  aria-hidden="true"
                >

                  <path
                    d="M5 12h14M14 7l5 5-5 5"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="1.45"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />

                </svg>

              </button>

            </div>


          </div>



          <!-- =================================================
               ECO VISUAL ROW
          ================================================== -->

          <div
            v-if="spec.background"
            class="mg-spec__eco-row"
          >


            <!-- MICRO COPY -->

            <div class="mg-spec__micro">

              <p>
                {{ spec.detail }}
              </p>

            </div>



            <!-- IMAGE -->

            <div class="mg-spec__visual-card">


              <img
                class="
                  mg-spec__visual-background
                "
                :src="spec.background"
                :style="{
                  objectPosition:
                    spec.backgroundPosition
                }"
                alt=""
                loading="lazy"
                decoding="async"
              />


              <div
                class="
                  mg-spec__visual-shade
                "
              ></div>


              <div
                class="
                  mg-spec__visual-ground
                "
              ></div>


              <img
                class="
                  mg-spec__visual-car
                "
                :src="carImage"
                alt="MG electric crossover"
                loading="lazy"
                decoding="async"
              />


              <div
                class="
                  mg-spec__visual-car-shadow
                "
              ></div>


              <!-- LABEL -->

              <div
                class="
                  mg-spec__visual-label
                "
              >

                <span>
                  ELECTRIC
                </span>

                <strong>
                  Zero-emission driving
                </strong>

              </div>


            </div>


          </div>



          <!-- =================================================
               EXPANDED DETAIL
          ================================================== -->

          <Transition name="spec-expand">

            <div
              v-if="
                activeSpec === spec.id &&
                !spec.background
              "
              class="mg-spec__expanded"
            >

              <p>
                {{ spec.detail }}
              </p>

            </div>

          </Transition>


        </article>

      </div>


    </div>

  </section>
</template>


<style scoped>

@import url(
  "https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700&display=swap"
);


/* =========================================================
   SECTION
========================================================= */

.mg-dynamics {
  --black:
    #171717;

  --muted:
    #666864;

  --line:
    #d9dad6;

  --page:
    #f7f7f4;

  --green:
    #637461;

  --red:
    #e51920;


  position:
    relative;

  width:
    100%;

  overflow:
    hidden;

  padding:
    clamp(
      70px,
      5.5vw,
      96px
    )
    0
    clamp(
      76px,
      6vw,
      104px
    );

  background:
    var(--page);

  color:
    var(--black);

  font-family:
    "Manrope",
    sans-serif;
}


/* =========================================================
   SUBTLE ECO BACKGROUND
========================================================= */

.mg-dynamics::before {
  content:
    "";

  position:
    absolute;

  right:
    -12vw;

  top:
    24%;

  width:
    37vw;

  height:
    37vw;

  border-radius:
    50%;

  pointer-events:
    none;

  opacity:
    .2;

  filter:
    blur(100px);

  background:
    rgba(
      128,
      151,
      121,
      .22
    );
}


/* =========================================================
   CONTAINER
========================================================= */

.mg-dynamics__container {
  position:
    relative;

  z-index:
    2;

  width:
    min(
      91%,
      1420px
    );

  margin:
    0
    auto;
}


/* =========================================================
   INTRO
========================================================= */

.mg-dynamics__intro {
  display:
    grid;

  grid-template-columns:
    minmax(
      0,
      1fr
    )
    minmax(
      0,
      1fr
    );

  align-items:
    start;

  gap:
    clamp(
      32px,
      4vw,
      65px
    );

  margin-bottom:
    clamp(
      35px,
      3.4vw,
      52px
    );
}


.mg-dynamics__headline h2 {
  max-width:
    650px;

  margin:
    0;

  font-size: 2.65vw;

  font-weight:
    650;

  line-height:
    1.08;

  letter-spacing:
    -.05em;
}


.mg-dynamics__intro-copy {
  display:
    flex;

  justify-content:
    flex-start;
}


.mg-dynamics__intro-copy p {
  max-width:
    600px;

  margin:
    5px
    0
    0;

  color:
    var(--muted);

  font-size: .68vw;

  line-height:
    1.62;
}


/* =========================================================
   TWO ECO CARDS
========================================================= */

.mg-dynamics__features {
  display:
    grid;

  grid-template-columns:
    repeat(
      2,
      minmax(
        0,
        1fr
      )
    );

  gap:
    clamp(
      14px,
      1.25vw,
      20px
    );

  margin-bottom:
    clamp(
      55px,
      5vw,
      76px
    );
}


/* =========================================================
   CARD
========================================================= */

.mg-eco-card {
  min-width:
    0;
}


/* =========================================================
   CARD HEADER
========================================================= */

.mg-eco-card__top {
  display:
    flex;

  align-items:
    center;

  justify-content:
    space-between;

  gap:
    15px;

  min-height:
    39px;

  margin-bottom:
    9px;
}


.mg-eco-card__pill {
  display:
    inline-flex;

  align-items:
    center;

  min-height:
    29px;

  padding:
    0
    15px;

  border-radius:
    6px;

  background:
    #343533;

  color:
    #fff;

  font-size: .49vw;

  font-weight:
    500;
}


.mg-eco-card__arrow {
  width:
    32px;

  height:
    32px;

  display:
    grid;

  place-items:
    center;

  padding:
    0;

  border:
    0;

  background:
    transparent;

  color:
    #151515;

  cursor:
    pointer;

  transition:
    transform
    .3s
    ease;
}


.mg-eco-card__arrow:hover {
  transform:
    translateX(
      4px
    );
}


.mg-eco-card__arrow svg {
  width:
    17px;
}


/* =========================================================
   ECO SCENE
========================================================= */

.mg-eco-card__scene {
  position:
    relative;

  width:
    100%;

  aspect-ratio:
    1.5 / 1;

  overflow:
    hidden;

  border-radius:
    7px;

  background:
    #b9c5b5;

  isolation:
    isolate;

  box-shadow:
    0
    20px
    50px
    rgba(
      31,
      42,
      32,
      .09
    );
}


/* =========================================================
   BACKGROUND
========================================================= */

.mg-eco-card__background {
  position:
    absolute;

  inset:
    0;

  z-index:
    -8;

  width:
    100%;

  height:
    100%;

  object-fit:
    cover;

  transform:
    scale(
      1.035
    );

  filter:
    saturate(
      .84
    )
    contrast(
      .96
    );

  transition:
    transform
    1.6s
    cubic-bezier(
      .16,
      1,
      .3,
      1
    ),
    filter
    .7s
    ease;
}


.mg-eco-card:hover
.mg-eco-card__background {
  transform:
    scale(
      1.075
    );

  filter:
    saturate(
      .96
    )
    contrast(
      1
    );
}


/* =========================================================
   CINEMATIC SHADE
========================================================= */

.mg-eco-card__shade {
  position:
    absolute;

  inset:
    0;

  z-index:
    -6;

  background:
    linear-gradient(
      180deg,
      rgba(
        246,
        249,
        242,
        .02
      )
      0%,
      rgba(
        17,
        27,
        19,
        .02
      )
      42%,
      rgba(
        11,
        20,
        13,
        .28
      )
      100%
    );
}


/* =========================================================
   SOFT LIGHT
========================================================= */

.mg-eco-card__light {
  position:
    absolute;

  left:
    43%;

  top:
    -25%;

  z-index:
    -5;

  width:
    60%;

  height:
    70%;

  border-radius:
    50%;

  opacity:
    .42;

  filter:
    blur(
      35px
    );

  background:
    radial-gradient(
      circle,
      rgba(
        255,
        248,
        205,
        .78
      ),
      transparent
      66%
    );
}


/* =========================================================
   GROUND TREATMENT
========================================================= */

.mg-eco-card__ground {
  position:
    absolute;

  left:
    -10%;

  right:
    -10%;

  bottom:
    -15%;

  z-index:
    -4;

  height:
    37%;

  opacity:
    .32;

  filter:
    blur(
      8px
    );

  background:
    radial-gradient(
      ellipse,
      rgba(
        24,
        29,
        23,
        .65
      ),
      transparent
      67%
    );
}


/* =========================================================
   MG CAR
========================================================= */

.mg-eco-card__car {
  position:
    absolute;

  z-index:
    5;

  display:
    block;

  object-fit:
    contain;

  filter:
    drop-shadow(
      0
      20px
      17px
      rgba(
        0,
        0,
        0,
        .22
      )
    );

  pointer-events:
    none;

  user-select:
    none;

  transition:
    transform
    1.1s
    cubic-bezier(
      .16,
      1,
      .3,
      1
    );
}


/* FIRST SCENE */

.eco-car--wind {
  left:
    16%;

  bottom:
    7%;

  width:
    70%;

  transform:
    rotate(
      -.5deg
    );
}


/* SECOND SCENE */

.eco-car--forest {
  left:
    14%;

  bottom:
    6%;

  width:
    73%;

  transform:
    rotate(
      .5deg
    );
}


.mg-eco-card:hover
.mg-eco-card__car {
  transform:
    translateY(
      -4px
    )
    scale(
      1.015
    );
}


/* =========================================================
   SHADOW
========================================================= */

.mg-eco-card__car-shadow {
  position:
    absolute;

  left:
    27%;

  bottom:
    8%;

  z-index:
    2;

  width:
    49%;

  height:
    6%;

  border-radius:
    50%;

  opacity:
    .55;

  filter:
    blur(
      16px
    );

  background:
    rgba(
      0,
      0,
      0,
      .46
    );
}


/* =========================================================
   ECO BADGE
========================================================= */

.mg-eco-card__eco {
  position:
    absolute;

  right:
    15px;

  top:
    15px;

  z-index:
    20;

  display:
    flex;

  align-items:
    center;

  gap:
    7px;

  padding:
    7px
    9px;

  border:
    1px
    solid
    rgba(
      255,
      255,
      255,
      .38
    );

  border-radius:
    999px;

  background:
    rgba(
      21,
      36,
      25,
      .28
    );

  backdrop-filter:
    blur(
      12px
    );

  color:
    #fff;

  font-size: 0.347vw;

  font-weight:
    700;

  letter-spacing:
    .12em;
}


.mg-eco-card__eco-icon {
  width:
    16px;

  height:
    16px;

  display:
    grid;

  place-items:
    center;

  border-radius:
    50%;

  background:
    rgba(
      255,
      255,
      255,
      .13
    );
}


.mg-eco-card__eco-icon svg {
  width:
    11px;

  height:
    11px;
}


/* =========================================================
   CARD CAPTION
========================================================= */

.mg-eco-card__caption {
  position:
    absolute;

  left:
    16px;

  bottom:
    15px;

  z-index:
    20;

  color:
    #fff;
}


.mg-eco-card__caption span {
  display:
    block;

  margin-bottom:
    4px;

  color:
    rgba(
      255,
      255,
      255,
      .63
    );

  font-size: 0.347vw;

  font-weight:
    700;

  letter-spacing:
    .14em;
}


.mg-eco-card__caption strong {
  font-size: .85vw;

  font-weight:
    600;

  letter-spacing:
    -.02em;
}


/* =========================================================
   SPEC INTRO
========================================================= */

.mg-specs__intro {
  display:
    grid;

  grid-template-columns:
    minmax(
      0,
      1fr
    )
    minmax(
      0,
      1fr
    );

  gap:
    clamp(
      32px,
      4vw,
      65px
    );

  align-items:
    start;

  margin-bottom:
    37px;
}


.mg-specs__intro h3 {
  margin:
    0;

  font-size: 2.3vw;

  font-weight:
    650;

  line-height:
    1.07;

  letter-spacing:
    -.048em;
}


.mg-specs__intro p {
  max-width:
    610px;

  margin:
    4px
    0
    0;

  color:
    var(--muted);

  font-size: .66vw;

  line-height:
    1.6;
}


/* =========================================================
   SPEC TABLE
========================================================= */

.mg-specs {
  border-top:
    1px
    solid
    var(--line);
}


.mg-spec {
  border-bottom:
    1px
    solid
    var(--line);
}


/* =========================================================
   SPEC ROW
========================================================= */

.mg-spec__row {
  display:
    grid;

  grid-template-columns:
    48px
    minmax(
      215px,
      .95fr
    )
    minmax(
      0,
      1.05fr
    );

  gap:
    clamp(
      12px,
      1.4vw,
      23px
    );

  align-items:
    center;

  min-height:
    92px;
}


/* =========================================================
   PLUS
========================================================= */

.mg-spec__plus {
  position:
    relative;

  width:
    36px;

  height:
    36px;

  display:
    grid;

  place-items:
    center;

  padding:
    0;

  border:
    1px
    solid
    #888b85;

  border-radius:
    50%;

  background:
    transparent;

  color:
    #252525;

  cursor:
    pointer;

  transition:
    color
    .25s
    ease,
    background
    .25s
    ease;
}


.mg-spec__plus span {
  position:
    absolute;

  width:
    11px;

  height:
    1px;

  background:
    currentColor;

  transition:
    transform
    .3s
    ease;
}


.mg-spec__plus
span:last-child {
  transform:
    rotate(
      90deg
    );
}


.mg-spec--open
.mg-spec__plus
span:last-child {
  transform:
    rotate(
      0deg
    );
}


.mg-spec__plus:hover {
  color:
    #fff;

  background:
    #20221f;
}


/* =========================================================
   VALUE
========================================================= */

.mg-spec__value {
  display:
    flex;

  align-items:
    baseline;
}


.mg-spec__value strong {
  font-size: 3vw;

  font-weight:
    650;

  line-height:
    1;

  letter-spacing:
    -.06em;
}


.mg-spec__value span {
  margin-left:
    2px;

  color:
    #575a56;

  font-size: 1vw;

  font-weight:
    500;
}


/* =========================================================
   DESCRIPTION
========================================================= */

.mg-spec__description {
  display:
    grid;

  grid-template-columns:
    minmax(
      0,
      1fr
    )
    30px;

  gap:
    15px;

  align-items:
    center;
}


.mg-spec__description p {
  max-width:
    480px;

  margin:
    0;

  font-size: .74vw;

  font-weight:
    600;

  line-height:
    1.43;

  letter-spacing:
    -.016em;
}


.mg-spec__arrow {
  width:
    30px;

  height:
    30px;

  display:
    grid;

  place-items:
    center;

  padding:
    0;

  border:
    0;

  background:
    transparent;

  cursor:
    pointer;

  transition:
    transform
    .3s
    ease;
}


.mg-spec__arrow:hover {
  transform:
    translateX(
      4px
    );
}


.mg-spec__arrow svg {
  width:
    17px;
}


/* =========================================================
   ECO VISUAL ROW
========================================================= */

.mg-spec__eco-row {
  display:
    grid;

  grid-template-columns:
    48px
    minmax(
      215px,
      .95fr
    )
    minmax(
      0,
      1.05fr
    );

  gap:
    clamp(
      12px,
      1.4vw,
      23px
    );

  padding:
    0
    0
    29px;
}


/* =========================================================
   MICRO COPY
========================================================= */

.mg-spec__micro {
  grid-column:
    2;

  align-self:
    end;

  padding-right:
    35px;
}


.mg-spec__micro p {
  max-width:
    235px;

  margin:
    0;

  color:
    #6c706a;

  font-size: .46vw;

  line-height:
    1.58;
}


/* =========================================================
   VISUAL
========================================================= */

.mg-spec__visual-card {
  position:
    relative;

  grid-column:
    3;

  justify-self:
    start;

  width:
    min(
      100%,
      480px
    );

  aspect-ratio:
    2.05 / 1;

  overflow:
    hidden;

  border-radius:
    5px;

  background:
    #bfc7b8;

  isolation:
    isolate;
}


/* BACKGROUND */

.mg-spec__visual-background {
  position:
    absolute;

  inset:
    0;

  z-index:
    -6;

  width:
    100%;

  height:
    100%;

  object-fit:
    cover;

  filter:
    saturate(
      .78
    )
    contrast(
      .95
    );

  transform:
    scale(
      1.04
    );
}


/* SHADE */

.mg-spec__visual-shade {
  position:
    absolute;

  inset:
    0;

  z-index:
    -4;

  background:
    linear-gradient(
      180deg,
      rgba(
        255,
        255,
        255,
        .05
      ),
      rgba(
        17,
        26,
        17,
        .2
      )
    );
}


/* GROUND */

.mg-spec__visual-ground {
  position:
    absolute;

  left:
    10%;

  right:
    10%;

  bottom:
    -4%;

  z-index:
    0;

  height:
    25%;

  border-radius:
    50%;

  opacity:
    .45;

  filter:
    blur(
      16px
    );

  background:
    rgba(
      0,
      0,
      0,
      .46
    );
}


/* CAR */

.mg-spec__visual-car {
  position:
    absolute;

  left:
    20%;

  bottom:
    3%;

  z-index:
    3;

  width:
    66%;

  height:
    auto;

  object-fit:
    contain;

  filter:
    drop-shadow(
      0
      11px
      9px
      rgba(
        0,
        0,
        0,
        .22
      )
    );
}


.mg-spec__visual-car-shadow {
  position:
    absolute;

  left:
    29%;

  bottom:
    6%;

  z-index:
    1;

  width:
    49%;

  height:
    7%;

  border-radius:
    50%;

  opacity:
    .5;

  filter:
    blur(
      10px
    );

  background:
    rgba(
      0,
      0,
      0,
      .4
    );
}


/* LABEL */

.mg-spec__visual-label {
  position:
    absolute;

  left:
    12px;

  top:
    11px;

  z-index:
    5;

  padding:
    7px
    9px;

  border:
    1px
    solid
    rgba(
      255,
      255,
      255,
      .33
    );

  border-radius:
    4px;

  background:
    rgba(
      12,
      26,
      17,
      .22
    );

  backdrop-filter:
    blur(
      9px
    );

  color:
    #fff;
}


.mg-spec__visual-label span {
  display:
    block;

  margin-bottom:
    2px;

  color:
    rgba(
      255,
      255,
      255,
      .62
    );

  font-size: 0.278vw;

  font-weight:
    700;

  letter-spacing:
    .14em;
}


.mg-spec__visual-label strong {
  display:
    block;

  font-size: 0.486vw;

  font-weight:
    600;
}


/* =========================================================
   EXPANDED
========================================================= */

.mg-spec__expanded {
  display:
    grid;

  grid-template-columns:
    48px
    minmax(
      215px,
      .95fr
    )
    minmax(
      0,
      1.05fr
    );

  gap:
    clamp(
      12px,
      1.4vw,
      23px
    );

  padding:
    0
    0
    27px;
}


.mg-spec__expanded p {
  grid-column:
    3;

  max-width:
    470px;

  margin:
    0;

  color:
    #6c706a;

  font-size: 0.625vw;

  line-height:
    1.6;
}


/* =========================================================
   TRANSITION
========================================================= */

.spec-expand-enter-active,
.spec-expand-leave-active {
  transition:
    opacity
    .3s
    ease,
    transform
    .35s
    ease;
}


.spec-expand-enter-from,
.spec-expand-leave-to {
  opacity:
    0;

  transform:
    translateY(
      -7px
    );
}


/* =========================================================
   LARGE SCREENS
========================================================= */




/* =========================================================
   TABLET
========================================================= */

@media (
  max-width:
  1000px
) {

  .mg-dynamics__headline h2 {
    font-size:
      34px;
  }


  .mg-specs__intro h3 {
    font-size:
      31px;
  }


  .mg-spec__row,
  .mg-spec__eco-row,
  .mg-spec__expanded {
    grid-template-columns:
      44px
      minmax(
        175px,
        .8fr
      )
      1.2fr;
  }


  .mg-spec__visual-card {
    width:
      100%;
  }


  .eco-car--wind {
    left:
      10%;

    width:
      78%;
  }


  .eco-car--forest {
    left:
      9%;

    width:
      80%;
  }

}


/* =========================================================
   MOBILE
========================================================= */

@media (
  max-width:
  767px
) {

  .mg-dynamics {
    padding:
      58px
      0
      66px;
  }


  .mg-dynamics__container {
    width:
      calc(
        100% - 30px
      );
  }


  /* INTRO */

  .mg-dynamics__intro {
    grid-template-columns:
      1fr;

    gap:
      18px;

    margin-bottom:
      34px;
  }


  .mg-dynamics__headline h2 {
    font-size:
      29px;
  }


  .mg-dynamics__headline h2 br {
    display:
      none;
  }


  .mg-dynamics__intro-copy p {
    max-width:
      460px;

    font-size:
      9px;
  }


  /* ECO CARDS */

  .mg-dynamics__features {
    grid-template-columns:
      1fr;

    gap:
      27px;

    margin-bottom:
      54px;
  }


  .mg-eco-card__top {
    min-height:
      32px;

    margin-bottom:
      8px;
  }


  .mg-eco-card__pill {
    min-height:
      27px;

    padding:
      0
      12px;
  }


  .mg-eco-card__scene {
    aspect-ratio:
      1.38 / 1;
  }


  .eco-car--wind,
  .eco-car--forest {
    left:
      8%;

    bottom:
      8%;

    width:
      84%;
  }


  .mg-eco-card__eco {
    right:
      11px;

    top:
      11px;
  }


  /* SPEC INTRO */

  .mg-specs__intro {
    grid-template-columns:
      1fr;

    gap:
      17px;

    margin-bottom:
      28px;
  }


  .mg-specs__intro h3 {
    font-size:
      28px;
  }


  .mg-specs__intro h3 br {
    display:
      none;
  }


  .mg-specs__intro p {
    font-size:
      9px;
  }


  /* SPEC */

  .mg-spec__row {
    grid-template-columns:
      38px
      .9fr
      1.15fr;

    gap:
      10px;

    min-height:
      91px;
  }


  .mg-spec__plus {
    width:
      31px;

    height:
      31px;
  }


  .mg-spec__value strong {
    font-size:
      31px;
  }


  .mg-spec__value span {
    font-size:
      11px;
  }


  .mg-spec__description {
    grid-template-columns:
      1fr
      22px;

    gap:
      5px;
  }


  .mg-spec__description p {
    font-size:
      8px;
  }


  .mg-spec__arrow {
    width:
      22px;

    height:
      22px;
  }


  .mg-spec__arrow svg {
    width:
      13px;
  }


  /* ECO SPEC */

  .mg-spec__eco-row {
    grid-template-columns:
      38px
      .9fr
      1.15fr;

    gap:
      10px;

    padding-bottom:
      22px;
  }


  .mg-spec__micro {
    padding-right:
      4px;
  }


  .mg-spec__micro p {
    font-size:
      6px;
  }


  .mg-spec__visual-car {
    left:
      13%;

    width:
      77%;
  }


  .mg-spec__expanded {
    grid-template-columns:
      38px
      .9fr
      1.15fr;

    gap:
      10px;
  }


  .mg-spec__expanded p {
    font-size:
      7px;
  }

}


/* =========================================================
   SMALL MOBILE
========================================================= */

@media (
  max-width:
  470px
) {

  .mg-spec__row {
    grid-template-columns:
      33px
      .78fr
      1.22fr;

    min-height:
      85px;
  }


  .mg-spec__plus {
    width:
      27px;

    height:
      27px;
  }


  .mg-spec__plus span {
    width:
      9px;
  }


  .mg-spec__value strong {
    font-size:
      27px;
  }


  .mg-spec__value span {
    font-size:
      9px;
  }


  .mg-spec__description p {
    font-size:
      7px;
  }


  .mg-spec__eco-row {
    grid-template-columns:
      33px
      .78fr
      1.22fr;
  }


  .mg-spec__micro p {
    font-size:
      5.4px;
  }


  .mg-spec__visual-label {
    display:
      none;
  }

}

</style>
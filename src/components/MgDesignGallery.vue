<script setup>
import {
  computed,
  onBeforeUnmount,
  onMounted,
  ref,
} from "vue";


/* =========================================================
   GALLERY DATA

   Temporary internet images from MG Motor Europe.
   You can move these into /public later.
========================================================= */

const gallery = [
  {
    id: 1,

    eyebrow:
      "Exterior Design",

    title:
      "Sculpted proportions",

    description:
      "Clean surfaces, confident proportions and carefully resolved details create a contemporary electric SUV with unmistakable MG character.",

    image:
      "https://news.mgmotor.eu/wp-content/uploads/2025/05/Right-front-45-degrees-1040x585.jpg",

    alt:
      "MGS5 EV exterior design",

    position:
      "center",
  },

  {
    id: 2,

    eyebrow:
      "Interior",

    title:
      "Designed around you",

    description:
      "The cabin combines generous space, intuitive digital controls and refined materials to create a calm environment built around the driver.",

    image:
      "https://news.mgmotor.eu/wp-content/uploads/2025/05/Grand-interior-decor-1040x585.jpg",

    alt:
      "MGS5 EV premium interior",

    position:
      "center",
  },

  {
    id: 3,

    eyebrow:
      "Driver Interface",

    title:
      "Everything within reach",

    description:
      "A focused steering-wheel design and intelligently positioned controls keep essential functions close without overwhelming the driving experience.",

    image:
      "https://news.mgmotor.eu/wp-content/uploads/2025/05/Steering-wheel-1040x585.jpg",

    alt:
      "MGS5 EV steering wheel",

    position:
      "center",
  },

  {
    id: 4,

    eyebrow:
      "Lighting",

    title:
      "A signature after dark",

    description:
      "Distinctive LED lighting gives the MGS5 EV its recognisable visual identity while improving clarity and confidence on the road.",

    image:
      "https://news.mgmotor.eu/wp-content/uploads/2025/05/Headlight-1040x585.jpg",

    alt:
      "MGS5 EV LED headlight",

    position:
      "center",
  },

  {
    id: 5,

    eyebrow:
      "Exterior Character",

    title:
      "Confident from every angle",

    description:
      "Strong shoulders, balanced surfacing and aerodynamic detailing give the vehicle a composed stance from front to rear.",

    image:
      "https://news.mgmotor.eu/wp-content/uploads/2025/05/Right-front-30-degrees-1040x585.jpg",

    alt:
      "MGS5 EV front exterior",

    position:
      "center",
  },

  {
    id: 6,

    eyebrow:
      "Electric Lifestyle",

    title:
      "Built for the journey",

    description:
      "Electric performance meets practical everyday mobility in an MG designed for urban life, open roads and everything in between.",

    image:
      "https://news.mgmotor.eu/wp-content/uploads/2025/05/KV1-1040x585.jpg",

    alt:
      "MGS5 EV lifestyle",

    position:
      "center",
  },

  {
    id: 7,

    eyebrow:
      "MG Experience",

    title:
      "Progressive by design",

    description:
      "Every element has been developed to make electric mobility feel natural, engaging and unmistakably MG.",

    image:
      "https://news.mgmotor.eu/wp-content/uploads/2025/05/KV2-1040x585.jpg",

    alt:
      "MGS5 EV electric vehicle",

    position:
      "center",
  },
];


/* =========================================================
   STATE
========================================================= */

const activeIndex =
  ref(1);


/* =========================================================
   COMPUTED
========================================================= */

const activeItem =
  computed(() => {

    return gallery[
      activeIndex.value
    ];

  });


const previousPreview =
  computed(() => {

    const index =
      (
        activeIndex.value -
        1 +
        gallery.length
      ) %
      gallery.length;


    return gallery[
      index
    ];

  });


const visibleThumbnails =
  computed(() => {

    return gallery.filter(
      (_, index) =>
        index !==
        activeIndex.value
    );

  });


/* =========================================================
   NAVIGATION
========================================================= */

const nextSlide = () => {

  activeIndex.value =
    (
      activeIndex.value +
      1
    ) %
    gallery.length;

};


const previousSlide = () => {

  activeIndex.value =
    (
      activeIndex.value -
      1 +
      gallery.length
    ) %
    gallery.length;

};


const selectSlide =
  (item) => {

    const index =
      gallery.findIndex(
        (slide) =>
          slide.id ===
          item.id
      );


    if (
      index !== -1
    ) {

      activeIndex.value =
        index;

    }

  };


/* =========================================================
   IMAGE FALLBACK
========================================================= */

const handleImageError =
  (event) => {

    event.currentTarget
      ?.classList
      .add(
        "is-missing"
      );

  };


/* =========================================================
   KEYBOARD
========================================================= */

const handleKeyboard =
  (event) => {

    if (
      event.key ===
      "ArrowRight"
    ) {

      nextSlide();

    }


    if (
      event.key ===
      "ArrowLeft"
    ) {

      previousSlide();

    }

  };


onMounted(() => {

  window.addEventListener(
    "keydown",
    handleKeyboard
  );

});


onBeforeUnmount(() => {

  window.removeEventListener(
    "keydown",
    handleKeyboard
  );

});
</script>


<template>

  <section class="mg-gallery">

    <div class="mg-gallery__container">


      <!-- =====================================================
           HEADER
      ====================================================== -->

      <header class="mg-gallery__header">


        <!-- SMALL LABEL -->

        <div class="mg-gallery__label">

          <span>
            Gallery
          </span>

        </div>



        <!-- MAIN HEADING -->

        <div class="mg-gallery__heading">

          <h2>

            A closer look at the design
            <br />
            and craftsmanship of MG.

          </h2>

        </div>



        <!-- DESCRIPTION -->

        <div class="mg-gallery__intro">

          <p>
            Explore the details that shape the MGS5 EV —
            from expressive exterior surfaces to its
            driver-focused interior, signature lighting
            and considered electric design.
          </p>

        </div>


      </header>



      <!-- =====================================================
           GALLERY STAGE
      ====================================================== -->

      <div class="mg-gallery__stage">


        <!-- ===================================================
             LEFT PREVIEW
        ==================================================== -->

        <button
          type="button"

          class="
            mg-gallery__side-preview
          "

          :aria-label="
            `View ${previousPreview.title}`
          "

          @click="
            previousSlide
          "
        >

          <img
            :src="
              previousPreview.image
            "

            :alt="
              previousPreview.alt
            "

            loading="
              lazy
            "

            decoding="
              async
            "

            @error="
              handleImageError
            "
          />


          <span>
            PREVIOUS
          </span>

        </button>



        <!-- ===================================================
             MAIN IMAGE
        ==================================================== -->

        <div class="mg-gallery__main">


          <Transition
            name="
              gallery-main
            "

            mode="
              out-in
            "
          >

            <img
              :key="
                activeItem.id
              "

              class="
                mg-gallery__main-image
              "

              :src="
                activeItem.image
              "

              :alt="
                activeItem.alt
              "

              :style="{
                objectPosition:
                  activeItem.position
              }"

              loading="
                eager
              "

              decoding="
                async
              "

              @error="
                handleImageError
              "
            />

          </Transition>



          <!-- SMALL IMAGE LABEL -->

          <div
            class="
              mg-gallery__main-label
            "
          >

            <span>
              MG
            </span>

            <i></i>

            <span>
              {{
                activeItem.eyebrow
              }}
            </span>

          </div>


        </div>



        <!-- ===================================================
             RIGHT THUMBNAILS
        ==================================================== -->

        <div class="mg-gallery__rail">

          <button
            v-for="
              item in visibleThumbnails
            "

            :key="
              item.id
            "

            type="
              button
            "

            class="
              mg-gallery__thumb
            "

            :aria-label="
              `View ${item.title}`
            "

            @click="
              selectSlide(
                item
              )
            "
          >

            <img
              :src="
                item.image
              "

              :alt="
                item.alt
              "

              loading="
                lazy
              "

              decoding="
                async
              "

              @error="
                handleImageError
              "
            />


            <span
              class="
                mg-gallery__thumb-index
              "
            >

              {{
                String(
                  item.id
                ).padStart(
                  2,
                  "0"
                )
              }}

            </span>

          </button>

        </div>



        <!-- ===================================================
             CONTROLS / DESCRIPTION
        ==================================================== -->

        <aside class="mg-gallery__details">


          <!-- ARROWS -->

          <div
            class="
              mg-gallery__controls
            "
          >

            <button
              type="
                button
              "

              aria-label="
                Previous image
              "

              @click="
                previousSlide
              "
            >

              <svg
                viewBox="
                  0 0 24 24
                "
                aria-hidden="
                  true
                "
              >

                <path
                  d="M19 12H5M10 7l-5 5 5 5"

                  fill="
                    none
                  "

                  stroke="
                    currentColor
                  "

                  stroke-width="
                    1.4
                  "

                  stroke-linecap="
                    round
                  "

                  stroke-linejoin="
                    round
                  "
                />

              </svg>

            </button>



            <button
              type="
                button
              "

              aria-label="
                Next image
              "

              @click="
                nextSlide
              "
            >

              <svg
                viewBox="
                  0 0 24 24
                "
                aria-hidden="
                  true
                "
              >

                <path
                  d="M5 12h14M14 7l5 5-5 5"

                  fill="
                    none
                  "

                  stroke="
                    currentColor
                  "

                  stroke-width="
                    1.4
                  "

                  stroke-linecap="
                    round
                  "

                  stroke-linejoin="
                    round
                  "
                />

              </svg>

            </button>

          </div>



          <!-- COPY -->

          <Transition
            name="
              gallery-copy
            "

            mode="
              out-in
            "
          >

            <div
              :key="
                activeItem.id
              "

              class="
                mg-gallery__copy
              "
            >

              <span>
                {{
                  activeItem.eyebrow
                }}
              </span>


              <h3>
                {{
                  activeItem.title
                }}
              </h3>


              <p>
                {{
                  activeItem.description
                }}
              </p>

            </div>

          </Transition>


        </aside>



        <!-- ===================================================
             NUMBER
        ==================================================== -->

        <div
          class="
            mg-gallery__counter
          "
        >

          <strong>
            {{
              String(
                activeIndex + 1
              ).padStart(
                2,
                "0"
              )
            }}
          </strong>


          <span>
            /
            {{
              String(
                gallery.length
              ).padStart(
                2,
                "0"
              )
            }}
          </span>

        </div>


      </div>


    </div>

  </section>

</template>


<style scoped>

@import url(
  "https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;500;600&family=Manrope:wght@400;500;600;700&display=swap"
);


/* =========================================================
   ROOT
========================================================= */

.mg-gallery {

  --black:
    #111111;

  --muted:
    #777976;

  --red:
    #e51920;

  --bg:
    #f7f7f5;


  position:
    relative;


  width:
    100%;


  overflow:
    hidden;


  padding:
    clamp(
      74px,
      6vw,
      105px
    )
    0
    clamp(
      78px,
      6vw,
      108px
    );


  background:
    var(--bg);


  color:
    var(--black);


  font-family:
    "Manrope",
    sans-serif;

}


/* =========================================================
   PAGE WIDTH
========================================================= */

.mg-gallery__container {

  width:
    min(
      92%,
      1480px
    );


  margin:
    0
    auto;

}


/* =========================================================
   HEADER

   Reference:
   tiny label / large heading / description
========================================================= */

.mg-gallery__header {

  display:
    grid;


  grid-template-columns:
    .6fr
    2.35fr
    1.05fr;


  gap:
    clamp(
      36px,
      4.5vw,
      76px
    );


  align-items:
    start;


  margin-bottom:
    clamp(
      45px,
      4vw,
      65px
    );

}


/* =========================================================
   LABEL
========================================================= */

.mg-gallery__label {

  padding-top:
    clamp(
      5px,
      .45vw,
      8px
    );

}


.mg-gallery__label span {

  font-size: .48vw;


  font-weight:
    650;


  letter-spacing:
    -.02em;

}


/* =========================================================
   HEADING
========================================================= */

.mg-gallery__heading h2 {

  max-width:
    760px;


  margin:
    0;


  font-size: 3.05vw;


  font-weight:
    500;


  line-height:
    1.02;


  letter-spacing:
    -.057em;

}


/* =========================================================
   INTRO
========================================================= */

.mg-gallery__intro {

  padding-top:
    clamp(
      4px,
      .4vw,
      7px
    );

}


.mg-gallery__intro p {

  max-width:
    330px;


  margin:
    0;


  color:
    #747674;


  font-size: .52vw;


  line-height:
    1.6;


  letter-spacing:
    -.01em;

}


/* =========================================================
   STAGE

   Based on the visual architecture of the supplied reference.
========================================================= */

.mg-gallery__stage {

  position:
    relative;


  display:
    grid;


  grid-template-columns:
    .66fr
    2.9fr
    2.25fr;


  grid-template-rows:
    auto
    auto;


  column-gap:
    clamp(
      15px,
      1.15vw,
      20px
    );


  row-gap:
    clamp(
      22px,
      2vw,
      32px
    );


  min-height:
    clamp(
      520px,
      40vw,
      680px
    );

}


/* =========================================================
   LEFT PREVIEW
========================================================= */

.mg-gallery__side-preview {

  position:
    relative;


  grid-column:
    1;


  grid-row:
    1;


  align-self:
    start;


  width:
    100%;


  aspect-ratio:
    1.22 / 1;


  overflow:
    hidden;


  padding:
    0;


  border:
    0;


  background:
    #e9e9e7;


  cursor:
    pointer;

}


.mg-gallery__side-preview img {

  width:
    100%;


  height:
    100%;


  display:
    block;


  object-fit:
    cover;


  transition:
    transform
    .8s
    cubic-bezier(
      .16,
      1,
      .3,
      1
    );

}


.mg-gallery__side-preview:hover img {

  transform:
    scale(
      1.045
    );

}


.mg-gallery__side-preview > span {

  position:
    absolute;


  left:
    8px;


  bottom:
    8px;


  padding:
    4px
    6px;


  background:
    rgba(
      255,
      255,
      255,
      .8
    );


  color:
    rgba(
      0,
      0,
      0,
      .62
    );


  font-size: 0.278vw;


  font-weight:
    700;


  letter-spacing:
    .12em;

}


/* =========================================================
   MAIN IMAGE
========================================================= */

.mg-gallery__main {

  position:
    relative;


  grid-column:
    2;


  grid-row:
    1 / 3;


  width:
    100%;


  height:
    clamp(
      430px,
      34vw,
      570px
    );


  overflow:
    hidden;


  background:
    #e9e9e7;

}


/* =========================================================
   MAIN IMAGE
========================================================= */

.mg-gallery__main-image {

  position:
    absolute;


  inset:
    0;


  display:
    block;


  width:
    100%;


  height:
    100%;


  object-fit:
    cover;


  transform:
    scale(
      1.005
    );

}


/* =========================================================
   SMALL MAIN IMAGE LABEL
========================================================= */

.mg-gallery__main-label {

  position:
    absolute;


  left:
    clamp(
      12px,
      1vw,
      17px
    );


  bottom:
    clamp(
      11px,
      .9vw,
      15px
    );


  z-index:
    8;


  display:
    flex;


  align-items:
    center;


  gap:
    7px;


  padding:
    7px
    9px;


  background:
    rgba(
      247,
      247,
      245,
      .78
    );


  backdrop-filter:
    blur(
      10px
    );


  -webkit-backdrop-filter:
    blur(
      10px
    );


  color:
    #111;


  font-size: .3vw;


  font-weight:
    700;


  letter-spacing:
    .12em;


  text-transform:
    uppercase;

}


.mg-gallery__main-label i {

  width:
    15px;


  height:
    1px;


  background:
    var(--red);

}


/* =========================================================
   IMAGE TRANSITION
========================================================= */

.gallery-main-enter-active,
.gallery-main-leave-active {

  transition:

    opacity
    .46s
    ease,

    transform
    .85s
    cubic-bezier(
      .16,
      1,
      .3,
      1
    );

}


.gallery-main-enter-from {

  opacity:
    0;


  transform:
    scale(
      1.025
    );

}


.gallery-main-leave-to {

  opacity:
    0;


  transform:
    scale(
      .995
    );

}


/* =========================================================
   THUMBNAIL RAIL
========================================================= */

.mg-gallery__rail {

  grid-column:
    3;


  grid-row:
    1;


  display:
    grid;


  grid-auto-flow:
    column;


  grid-auto-columns:
    clamp(
      100px,
      8vw,
      135px
    );


  align-content:
    start;


  gap:
    clamp(
      8px,
      .65vw,
      11px
    );


  width:
    calc(
      100% + 14vw
    );


  overflow-x:
    auto;


  overflow-y:
    hidden;


  padding-bottom:
    6px;


  scroll-snap-type:
    x proximity;


  scrollbar-width:
    none;

}


.mg-gallery__rail::-webkit-scrollbar {

  display:
    none;

}


/* =========================================================
   THUMBNAIL
========================================================= */

.mg-gallery__thumb {

  position:
    relative;


  width:
    100%;


  aspect-ratio:
    1.18 / 1;


  overflow:
    hidden;


  padding:
    0;


  border:
    0;


  background:
    #e9e9e7;


  cursor:
    pointer;


  scroll-snap-align:
    start;

}


.mg-gallery__thumb img {

  display:
    block;


  width:
    100%;


  height:
    100%;


  object-fit:
    cover;


  filter:
    saturate(
      .9
    );


  transition:

    transform
    .7s
    cubic-bezier(
      .16,
      1,
      .3,
      1
    ),

    filter
    .35s
    ease;

}


.mg-gallery__thumb:hover img {

  transform:
    scale(
      1.055
    );


  filter:
    saturate(
      1
    );

}


.mg-gallery__thumb-index {

  position:
    absolute;


  right:
    6px;


  bottom:
    6px;


  display:
    grid;


  place-items:
    center;


  min-width:
    19px;


  height:
    19px;


  padding:
    0
    4px;


  box-sizing:
    border-box;


  background:
    rgba(
      255,
      255,
      255,
      .82
    );


  color:
    #111;


  font-family:
    "Barlow Condensed",
    sans-serif;


  font-size: 0.486vw;

}


/* =========================================================
   DETAILS AREA
========================================================= */

.mg-gallery__details {

  grid-column:
    3;


  grid-row:
    2;


  align-self:
    end;


  display:
    grid;


  grid-template-columns:
    1fr;


  width:
    min(
      260px,
      70%
    );


  margin-top:
    auto;

}


/* =========================================================
   CONTROLS
========================================================= */

.mg-gallery__controls {

  display:
    flex;


  align-items:
    center;


  gap:
    7px;


  margin-bottom:
    clamp(
      20px,
      1.7vw,
      28px
    );

}


.mg-gallery__controls button {

  width:
    clamp(
      31px,
      2vw,
      35px
    );


  height:
    clamp(
      31px,
      2vw,
      35px
    );


  display:
    grid;


  place-items:
    center;


  padding:
    0;


  border:
    1px
    solid
    rgba(
      0,
      0,
      0,
      .45
    );


  border-radius:
    50%;


  background:
    transparent;


  color:
    #111;


  cursor:
    pointer;


  transition:

    color
    .3s
    ease,

    background
    .3s
    ease,

    transform
    .3s
    ease;

}


.mg-gallery__controls button:hover {

  background:
    #111;


  color:
    #fff;


  transform:
    scale(
      1.05
    );

}


.mg-gallery__controls svg {

  width:
    14px;

}


/* =========================================================
   DETAILS COPY
========================================================= */

.mg-gallery__copy > span {

  display:
    block;


  margin-bottom:
    6px;


  color:
    var(--red);


  font-size: .32vw;


  font-weight:
    700;


  letter-spacing:
    .14em;


  text-transform:
    uppercase;

}


.mg-gallery__copy h3 {

  margin:
    0
    0
    9px;


  font-size: .9vw;


  font-weight:
    600;


  line-height:
    1.13;


  letter-spacing:
    -.035em;

}


.mg-gallery__copy p {

  max-width:
    230px;


  margin:
    0;


  color:
    #767875;


  font-size: .43vw;


  line-height:
    1.62;

}


/* =========================================================
   COPY TRANSITION
========================================================= */

.gallery-copy-enter-active,
.gallery-copy-leave-active {

  transition:

    opacity
    .25s
    ease,

    transform
    .4s
    cubic-bezier(
      .16,
      1,
      .3,
      1
    );

}


.gallery-copy-enter-from {

  opacity:
    0;


  transform:
    translateY(
      8px
    );

}


.gallery-copy-leave-to {

  opacity:
    0;


  transform:
    translateY(
      -5px
    );

}


/* =========================================================
   COUNTER
========================================================= */

.mg-gallery__counter {

  position:
    absolute;


  right:
    0;


  bottom:
    0;


  display:
    flex;


  align-items:
    baseline;


  color:
    #111;


  font-family:
    "Barlow Condensed",
    sans-serif;


  line-height:
    .85;

}


.mg-gallery__counter strong {

  font-size: 4.4vw;


  font-weight:
    400;


  letter-spacing:
    -.055em;

}


.mg-gallery__counter span {

  margin-left:
    3px;


  color:
    rgba(
      0,
      0,
      0,
      .08
    );


  font-size: 3.8vw;


  font-weight:
    400;


  letter-spacing:
    -.05em;

}


/* =========================================================
   BROKEN IMAGE
========================================================= */

.mg-gallery img.is-missing {

  opacity:
    0;


  background:
    #ececea;

}


/* =========================================================
   LARGE SCREENS / VW SYSTEM
========================================================= */




/* =========================================================
   TABLET
========================================================= */

@media (
  max-width:
  1050px
) {

  .mg-gallery__header {

    grid-template-columns:
      .45fr
      1.8fr
      .8fr;


    gap:
      30px;

  }


  .mg-gallery__heading h2 {

    font-size:
      36px;

  }


  .mg-gallery__stage {

    grid-template-columns:
      .55fr
      2.5fr
      1.5fr;


    min-height:
      550px;

  }


  .mg-gallery__main {

    height:
      450px;

  }


  .mg-gallery__rail {

    grid-auto-columns:
      100px;


    width:
      calc(
        100% + 10vw
      );

  }


  .mg-gallery__details {

    width:
      90%;

  }

}


/* =========================================================
   MOBILE
========================================================= */

@media (
  max-width:
  767px
) {

  .mg-gallery {

    padding:
      58px
      0
      65px;

  }


  .mg-gallery__container {

    width:
      calc(
        100% - 30px
      );

  }


  /* HEADER */

  .mg-gallery__header {

    display:
      block;


    margin-bottom:
      35px;

  }


  .mg-gallery__label {

    padding:
      0;


    margin-bottom:
      15px;

  }


  .mg-gallery__label span {

    font-size:
      8px;

  }


  .mg-gallery__heading h2 {

    max-width:
      440px;


    font-size:
      30px;

  }


  .mg-gallery__heading h2 br {

    display:
      none;

  }


  .mg-gallery__intro {

    padding:
      0;


    margin-top:
      18px;

  }


  .mg-gallery__intro p {

    max-width:
      340px;


    font-size:
      8px;

  }


  /* STAGE */

  .mg-gallery__stage {

    display:
      flex;


    flex-direction:
      column;


    min-height:
      0;


    gap:
      14px;

  }


  /* HIDE SEPARATE LEFT PREVIEW */

  .mg-gallery__side-preview {

    display:
      none;

  }


  /* MAIN */

  .mg-gallery__main {

    order:
      1;


    width:
      100%;


    height:
      auto;


    aspect-ratio:
      1.28 / 1;

  }


  /* THUMBNAILS */

  .mg-gallery__rail {

    order:
      2;


    display:
      grid;


    grid-auto-flow:
      column;


    grid-auto-columns:
      105px;


    width:
      100%;


    gap:
      8px;


    padding:
      0
      0
      5px;

  }


  /* DETAILS */

  .mg-gallery__details {

    order:
      3;


    width:
      100%;


    display:
      grid;


    grid-template-columns:
      82px
      minmax(
        0,
        1fr
      );


    gap:
      18px;


    align-items:
      start;


    margin-top:
      12px;

  }


  .mg-gallery__controls {

    margin:
      0;


    gap:
      6px;

  }


  .mg-gallery__controls button {

    width:
      36px;


    height:
      36px;

  }


  .mg-gallery__copy h3 {

    font-size:
      15px;

  }


  .mg-gallery__copy p {

    max-width:
      300px;


    font-size:
      7px;

  }


  /* COUNTER */

  .mg-gallery__counter {

    position:
      relative;


    order:
      4;


    align-self:
      flex-end;


    margin-top:
      15px;

  }


  .mg-gallery__counter strong {

    font-size:
      48px;

  }


  .mg-gallery__counter span {

    font-size:
      42px;

  }

}


/* =========================================================
   SMALL MOBILE
========================================================= */

@media (
  max-width:
  420px
) {

  .mg-gallery__heading h2 {

    font-size:
      28px;

  }


  .mg-gallery__main {

    aspect-ratio:
      1.08 / 1;

  }


  .mg-gallery__rail {

    grid-auto-columns:
      91px;

  }


  .mg-gallery__details {

    grid-template-columns:
      76px
      1fr;


    gap:
      14px;

  }

}


/* =========================================================
   REDUCED MOTION
========================================================= */

@media (
  prefers-reduced-motion:
  reduce
) {

  .mg-gallery *,
  .mg-gallery *::before,
  .mg-gallery *::after {

    animation-duration:
      .01ms !important;


    animation-iteration-count:
      1 !important;


    transition-duration:
      .01ms !important;

  }

}

</style>
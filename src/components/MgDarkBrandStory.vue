<template>
  <section class="mg-night-drive">
    <div
      class="mg-night-drive__noise"
      aria-hidden="true"
    ></div>

    <!-- =====================================================
         FULL-BLEED HERO IMAGE
         IMAGE STARTS WITH THE SECTION
    ====================================================== -->

    <div class="mg-night-drive__visual">
      <img
        class="mg-night-drive__image"
        src="\images\ChatGPT Image Aug 23, 2026, 05_44_38 PM.png"
        alt="MG vehicle on a dramatic mountain road"
        loading="lazy"
        decoding="async"
      />

      <!-- cinematic treatments -->
      <div
        class="mg-night-drive__shade"
        aria-hidden="true"
      ></div>

      <div
        class="mg-night-drive__vignette"
        aria-hidden="true"
      ></div>

      <div
        class="mg-night-drive__light"
        aria-hidden="true"
      ></div>

      <!-- ===================================================
           MAIN HEADING + DESCRIPTION
           NOW SITS DIRECTLY ON IMAGE
      ==================================================== -->

      <header class="mg-night-drive__header">
        <div class="mg-night-drive__heading">
          <span class="mg-night-drive__chapter">
            05 / MG STORY
          </span>

          <h2>
            Made to move you.
          </h2>
        </div>

        <div class="mg-night-drive__header-copy">
          <p>
            From the first MG in 1924 to a new generation
            of electrified vehicles, every chapter has been
            shaped around one idea — make the drive feel memorable.
          </p>
        </div>
      </header>

      <!-- ===================================================
           SMALL TOP META
      ==================================================== -->

      <div class="mg-night-drive__visual-meta">
        <div class="mg-night-drive__visual-id">
          <span>MG</span>
          <i></i>
          <span>SINCE 1924</span>
        </div>

        <span>
          DRIVEN FORWARD
        </span>
      </div>

      <!-- ===================================================
           MAIN STATEMENT
      ==================================================== -->

      

      <!-- ===================================================
           HERITAGE STAT
      ==================================================== -->

      <div class="mg-night-drive__metric">
        <strong>
          100+
        </strong>

        <span>
          Years of MG
        </span>

        <i></i>

        <small>
          A century of driving character,
          reimagined for what comes next.
        </small>
      </div>

      <!-- ===================================================
           IMAGE BOTTOM DETAILS
      ==================================================== -->

      <div class="mg-night-drive__image-footer">
        <div>
          <span>DESIGN</span>
          <strong>Expressive</strong>
        </div>

        <div>
          <span>PERFORMANCE</span>
          <strong>Responsive</strong>
        </div>

        <div>
          <span>TECHNOLOGY</span>
          <strong>Intuitive</strong>
        </div>
      </div>
    </div>

    <!-- =====================================================
         LOWER DARK CONTENT
    ====================================================== -->

    <div class="mg-night-drive__lower">
      <div class="mg-night-drive__lower-label">
        <span>
          THE MG SPIRIT
        </span>
      </div>

      <div class="mg-night-drive__lower-copy">
        <p>
          Expressive design, responsive performance and technology
          that stays focused on the driver. The character evolves.
          The feeling remains unmistakably MG.
        </p>
      </div>

      <div class="mg-night-drive__lower-action">
        <a
          href="#mg-range"
          class="mg-night-drive__cta"
        >
          <span>
            Discover the range
          </span>

          <span
            class="mg-night-drive__cta-icon"
            aria-hidden="true"
          >
            ↗
          </span>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>

/* =========================================================
   ROOT
========================================================= */

.mg-night-drive {
  --red: #e51920;

  position: relative;
  width: 100%;
  overflow: hidden;
  isolation: isolate;

  background: #070707;
  color: #fff;

  font-family:
    "Manrope",
    sans-serif;
}


/* =========================================================
   NOISE
========================================================= */

.mg-night-drive__noise {
  position: absolute;
  inset: 0;

  z-index: 20;

  pointer-events: none;

  opacity: .02;

  background-image:
    url(
      "data:image/svg+xml,%3Csvg viewBox='0 0 180 180' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.86' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='.55'/%3E%3C/svg%3E"
    );
}


/* =========================================================
   FULL IMAGE

   IMAGE STARTS IMMEDIATELY WITH SECTION
========================================================= */

.mg-night-drive__visual {
  position: relative;

  width: 100vw;
  height: 48vw;

  min-height: 42vw;

  overflow: hidden;
  isolation: isolate;

  background: #0b0b0b;
}


/* =========================================================
   IMAGE
========================================================= */

.mg-night-drive__image {
  position: absolute;
  inset: 0;

  z-index: 0;

  width: 100%;
  height: 100%;

  display: block;

  object-fit: cover;
  object-position: center 49%;

  filter:
    saturate(.82)
    brightness(.66)
    contrast(1.1);

  transform:
    scale(1.01);

  transition:
    transform
    2.2s
    cubic-bezier(.16, 1, .3, 1),
    filter
    .9s
    ease;
}


.mg-night-drive__visual:hover
.mg-night-drive__image {
  transform:
    scale(1.027);

  filter:
    saturate(.9)
    brightness(.7)
    contrast(1.08);
}


/* =========================================================
   IMAGE SHADE

   Darker at top-left for heading readability.
========================================================= */

.mg-night-drive__shade {
  position: absolute;
  inset: 0;

  z-index: 1;

  pointer-events: none;

  background:

    linear-gradient(
      180deg,
      rgba(0, 0, 0, .72) 0%,
      rgba(0, 0, 0, .35) 21%,
      rgba(0, 0, 0, .08) 44%,
      rgba(0, 0, 0, .18) 66%,
      rgba(0, 0, 0, .78) 100%
    ),

    linear-gradient(
      90deg,
      rgba(0, 0, 0, .56) 0%,
      rgba(0, 0, 0, .28) 29%,
      transparent 63%
    );
}


/* =========================================================
   VIGNETTE
========================================================= */

.mg-night-drive__vignette {
  position: absolute;
  inset: 0;

  z-index: 2;

  box-shadow:
    inset
    0
    0
    9vw
    rgba(0, 0, 0, .43);

  pointer-events: none;
}


/* =========================================================
   LIGHT BLOOM
========================================================= */

.mg-night-drive__light {
  position: absolute;

  left: -10vw;
  top: 14%;

  z-index: 2;

  width: 42vw;
  height: 30vw;

  border-radius: 50%;

  opacity: .2;

  filter:
    blur(5vw);

  background:
    radial-gradient(
      circle,
      rgba(255, 243, 213, .38),
      transparent 67%
    );

  pointer-events: none;
}


/* =========================================================
   MAIN HEADER OVER IMAGE
========================================================= */

.mg-night-drive__header {
  position: absolute;

  left: 4.5vw;
  right: 4.5vw;
  top: 4.1vw;

  z-index: 8;

  display: grid;

  grid-template-columns:
    minmax(0, 1.3fr)
    minmax(0, .7fr);

  gap: 8vw;

  align-items: start;
}


/* =========================================================
   CHAPTER
========================================================= */

.mg-night-drive__chapter {
  display: block;

  margin-bottom: .9vw;

  color:
    rgba(255, 255, 255, .42);

  font-size: .4vw;

  font-weight: 700;

  letter-spacing: .17em;
}


/* =========================================================
   MAIN HEADING
========================================================= */

.mg-night-drive__heading h2 {
  margin: 0;

  color: #fff;

  font-size: 3vw;

  font-weight: 550;

  line-height: .98;

  letter-spacing: -.06em;

  text-shadow:
    0
    .8vw
    2.4vw
    rgba(0, 0, 0, .4);
}


/* =========================================================
   DESCRIPTION
========================================================= */

.mg-night-drive__header-copy {
  justify-self: end;

  width: 24vw;

  padding-top: .3vw;
}


.mg-night-drive__header-copy p {
  margin: 0;

  color:
    rgba(255, 255, 255, .63);

  font-size: .66vw;

  line-height: 1.7;

  text-shadow:
    0
    .5vw
    1.6vw
    rgba(0, 0, 0, .45);
}


/* =========================================================
   SMALL IMAGE META
========================================================= */

.mg-night-drive__visual-meta {
  position: absolute;

  left: 4.5vw;
  right: 4.5vw;
  top: 12.6vw;

  z-index: 7;

  display: flex;

  align-items: center;

  justify-content: space-between;

  color:
    rgba(255, 255, 255, .37);

  font-size: .36vw;

  font-weight: 700;

  letter-spacing: .16em;
}


/* =========================================================
   VISUAL ID
========================================================= */

.mg-night-drive__visual-id {
  display: flex;

  align-items: center;

  gap: .65vw;
}


.mg-night-drive__visual-id i {
  width: 2.3vw;
  height: 1px;

  background:
    rgba(255, 255, 255, .29);
}


/* =========================================================
   STATEMENT
========================================================= */

.mg-night-drive__statement {
  position: absolute;

  left: 4.5vw;
  bottom: 5vw;

  z-index: 8;

  max-width: 39vw;
}


/* NUMBER */

.mg-night-drive__statement-number {
  display: block;

  margin-bottom: .72vw;

  color:
    rgba(255, 255, 255, .42);

  font-family:
    "Barlow Condensed",
    sans-serif;

  font-size: .73vw;

  letter-spacing: .08em;
}


/* TITLE */

.mg-night-drive__statement h3 {
  margin: 0;

  color: #fff;

  font-size: 3.35vw;

  font-weight: 550;

  line-height: .9;

  letter-spacing: -.065em;

  text-transform: uppercase;

  text-shadow:
    0
    1vw
    2.5vw
    rgba(0, 0, 0, .5);
}


/* =========================================================
   RED MOTION LINE
========================================================= */

.mg-night-drive__statement-line {
  width: 10vw;
  height: 1px;

  margin-top: 1.5vw;

  overflow: hidden;

  background:
    rgba(255, 255, 255, .16);
}


.mg-night-drive__statement-line span {
  display: block;

  width: 44%;
  height: 100%;

  background: var(--red);

  animation:
    mgNightLine
    3.4s
    ease-in-out
    infinite;
}


@keyframes mgNightLine {
  0%,
  100% {
    transform:
      translateX(-110%);
  }

  50% {
    transform:
      translateX(250%);
  }
}


/* =========================================================
   METRIC
========================================================= */

.mg-night-drive__metric {
  position: absolute;

  right: 5vw;
  top: 51%;

  z-index: 8;

  width: 12.5vw;

  transform:
    translateY(-50%);
}


.mg-night-drive__metric strong {
  display: block;

  color: var(--red);

  font-family:
    "Barlow Condensed",
    sans-serif;

  font-size: 5vw;

  font-weight: 500;

  line-height: .78;

  letter-spacing: -.05em;
}


.mg-night-drive__metric span {
  display: block;

  margin-top: 1.1vw;

  color:
    rgba(255, 255, 255, .75);

  font-size: .66vw;

  font-weight: 550;
}


.mg-night-drive__metric i {
  display: block;

  width: 100%;
  height: 1px;

  margin:
    1.25vw
    0
    1vw;

  background:
    linear-gradient(
      90deg,
      rgba(255, 255, 255, .6),
      rgba(255, 255, 255, .08)
    );
}


.mg-night-drive__metric small {
  display: block;

  width: 10vw;

  color:
    rgba(255, 255, 255, .4);

  font-size: .49vw;

  line-height: 1.6;
}


/* =========================================================
   IMAGE FOOTER DETAILS
========================================================= */

.mg-night-drive__image-footer {
  position: absolute;

  right: 4.5vw;
  bottom: 2vw;

  z-index: 8;

  display: flex;

  gap: 2.8vw;
}


.mg-night-drive__image-footer div {
  min-width: 6vw;
}


.mg-night-drive__image-footer span {
  display: block;

  margin-bottom: .3vw;

  color:
    rgba(255, 255, 255, .31);

  font-size: .34vw;

  font-weight: 700;

  letter-spacing: .15em;
}


.mg-night-drive__image-footer strong {
  color:
    rgba(255, 255, 255, .7);

  font-size: .54vw;

  font-weight: 500;
}


/* =========================================================
   LOWER DARK AREA
========================================================= */

.mg-night-drive__lower {
  width: 91vw;

  margin: 0 auto;

  display: grid;

  grid-template-columns:
    .45fr
    1.25fr
    .5fr;

  gap: 4vw;

  align-items: start;

  padding:
    3.4vw
    0
    5vw;
}


/* =========================================================
   LABEL
========================================================= */

.mg-night-drive__lower-label span {
  color:
    rgba(255, 255, 255, .28);

  font-size: .4vw;

  font-weight: 700;

  letter-spacing: .16em;
}


/* =========================================================
   LOWER COPY
========================================================= */

.mg-night-drive__lower-copy p {
  max-width: 32vw;

  margin: 0;

  color:
    rgba(255, 255, 255, .49);

  font-size: .64vw;

  line-height: 1.72;
}


/* =========================================================
   CTA
========================================================= */

.mg-night-drive__lower-action {
  justify-self: end;
}


.mg-night-drive__cta {
  display: inline-flex;

  align-items: center;

  gap: .9vw;

  color: #fff;

  text-decoration: none;

  font-size: .6vw;

  font-weight: 600;
}


.mg-night-drive__cta-icon {
  width: 2.35vw;
  height: 2.35vw;

  display: grid;

  place-items: center;

  border:
    1px
    solid
    rgba(255, 255, 255, .15);

  border-radius: 50%;

  background:
    rgba(255, 255, 255, .04);

  font-size: .72vw;

  transition:
    background
    .3s
    ease,
    border-color
    .3s
    ease,
    transform
    .3s
    ease;
}


.mg-night-drive__cta:hover
.mg-night-drive__cta-icon {
  background: var(--red);

  border-color: var(--red);

  transform:
    translate(
      .12vw,
      -.12vw
    );
}


/* =========================================================
   TABLET
========================================================= */

@media (
  max-width: 1000px
) {

  .mg-night-drive__visual {
    height: 62vw;
  }


  .mg-night-drive__header {
    top: 5vw;

    grid-template-columns:
      1.2fr
      .8fr;

    gap: 5vw;
  }


  .mg-night-drive__chapter {
    font-size: .65vw;
  }


  .mg-night-drive__heading h2 {
    font-size: 4.3vw;
  }


  .mg-night-drive__header-copy {
    width: 31vw;
  }


  .mg-night-drive__header-copy p {
    font-size: 1vw;
  }


  .mg-night-drive__visual-meta {
    top: 17vw;

    font-size: .6vw;
  }


  .mg-night-drive__statement h3 {
    font-size: 4.8vw;
  }


  .mg-night-drive__statement-number {
    font-size: 1vw;
  }


  .mg-night-drive__metric {
    width: 16vw;
  }


  .mg-night-drive__metric strong {
    font-size: 6.8vw;
  }


  .mg-night-drive__metric span {
    font-size: .94vw;
  }


  .mg-night-drive__metric small {
    width: 14vw;

    font-size: .72vw;
  }


  .mg-night-drive__image-footer span {
    font-size: .52vw;
  }


  .mg-night-drive__image-footer strong {
    font-size: .78vw;
  }


  .mg-night-drive__lower {
    width: 92vw;
  }


  .mg-night-drive__lower-label span {
    font-size: .65vw;
  }


  .mg-night-drive__lower-copy p {
    font-size: .95vw;
  }


  .mg-night-drive__cta {
    font-size: .9vw;
  }


  .mg-night-drive__cta-icon {
    width: 3.2vw;
    height: 3.2vw;

    font-size: 1vw;
  }

}


/* =========================================================
   MOBILE
========================================================= */

@media (
  max-width: 767px
) {

  .mg-night-drive__visual {
    height: 138vw;

    min-height: 0;
  }


  .mg-night-drive__image {
    object-position:
      65%
      center;
  }


  .mg-night-drive__shade {
    background:

      linear-gradient(
        180deg,
        rgba(0, 0, 0, .72) 0%,
        rgba(0, 0, 0, .38) 24%,
        transparent 47%,
        rgba(0, 0, 0, .74) 100%
      ),

      linear-gradient(
        90deg,
        rgba(0, 0, 0, .48),
        transparent 73%
      );
  }


  /* HEADER */

  .mg-night-drive__header {
    left: 15px;
    right: 15px;
    top: 9vw;

    display: block;
  }


  .mg-night-drive__chapter {
    margin-bottom: 2.5vw;

    font-size: 1.55vw;
  }


  .mg-night-drive__heading h2 {
    font-size: 8vw;
  }


  .mg-night-drive__header-copy {
    width: 80vw;

    margin-top: 4vw;
  }


  .mg-night-drive__header-copy p {
    font-size: 2.25vw;
  }


  /* META */

  .mg-night-drive__visual-meta {
    left: 15px;
    right: 15px;
    top: 39vw;

    font-size: 1.3vw;
  }


  .mg-night-drive__visual-id {
    gap: 1.4vw;
  }


  .mg-night-drive__visual-id i {
    width: 6vw;
  }


  /* STATEMENT */

  .mg-night-drive__statement {
    left: 5vw;
    bottom: 15vw;

    max-width: 75vw;
  }


  .mg-night-drive__statement-number {
    margin-bottom: 1.7vw;

    font-size: 2vw;
  }


  .mg-night-drive__statement h3 {
    font-size: 8.8vw;
  }


  .mg-night-drive__statement-line {
    width: 24vw;

    margin-top: 4vw;
  }


  /* METRIC */

  .mg-night-drive__metric {
    right: 5vw;
    top: 50%;

    width: 27vw;
  }


  .mg-night-drive__metric strong {
    font-size: 12vw;
  }


  .mg-night-drive__metric span {
    margin-top: 2.5vw;

    font-size: 2vw;
  }


  .mg-night-drive__metric i {
    margin:
      2.7vw
      0
      2vw;
  }


  .mg-night-drive__metric small {
    width: 25vw;

    font-size: 1.55vw;
  }


  /* HIDE SMALL DETAILS */

  .mg-night-drive__image-footer {
    display: none;
  }


  /* LOWER */

  .mg-night-drive__lower {
    display: block;

    width:
      calc(
        100% -
        30px
      );

    padding:
      7vw
      0
      12vw;
  }


  .mg-night-drive__lower-label span {
    display: block;

    margin-bottom: 3vw;

    font-size: 1.55vw;
  }


  .mg-night-drive__lower-copy p {
    max-width: 84vw;

    font-size: 2.25vw;
  }


  .mg-night-drive__lower-action {
    margin-top: 5vw;
  }


  .mg-night-drive__cta {
    gap: 2.5vw;

    font-size: 2.25vw;
  }


  .mg-night-drive__cta-icon {
    width: 8.5vw;
    height: 8.5vw;

    font-size: 2.7vw;
  }

}


/* =========================================================
   REDUCED MOTION
========================================================= */

@media (
  prefers-reduced-motion: reduce
) {

  .mg-night-drive *,
  .mg-night-drive *::before,
  .mg-night-drive *::after {
    animation-duration:
      .01ms !important;

    animation-iteration-count:
      1 !important;

    transition-duration:
      .01ms !important;
  }

}

</style>
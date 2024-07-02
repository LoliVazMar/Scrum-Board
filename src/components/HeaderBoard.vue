<template>
  <div class="header_container">
    <div class="header_title">Scrum Board</div>
    <div class="header_subtitle"><span class="by">by</span> Loli<span class="dev">Dev</span></div>
  </div>
</template>

<style lang="scss">
@import '../assets/base.scss';

.header_container {
  display: flex;
  justify-content: space-between;
  background-color: rgba(0, 0, 0, 0.33);
  font-size: clamp(0.2rem, 1vw + 1rem, 2rem);
  font-weight: 700;

  .header_title,
  .header_subtitle {
    padding: 1rem 4rem;
  }

  .header_subtitle {
    font-weight: 700;
    .by {
      font-size: 1rem;
      font-style: italic;
      font-weight: 500;
    }
    .dev {
      color: $primary-color;
      font-weight: 700;
    }
  }
}

@media (width <= 600px) {
  .header_container {
    flex-wrap: wrap;

    .header_title,
    .header_subtitle {
      padding: 0.5rem 2rem;
    }

    .header_subtitle {
      text-align: right;
    }
  }
}

@media (width <= 325px) {
  .header_container {
    flex-direction: column;
    justify-content: center;

    .header_title,
    .header_subtitle {
      padding: 0.1rem 1rem;
    }
  }
}
</style>

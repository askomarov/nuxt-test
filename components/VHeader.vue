<script setup>
const headerLinks = [
  {
    href: "#",
    text: "Mobile Top Up",
    isActive: true,
  },
  {
    href: "/about",
    text: "About",
  },
  {
    href: "/about",
    text: "Rate",
  },
  {
    href: "/about",
    text: "Help",
  },
];
function toggelMenu() {
  console.log("click");
}
</script>
<template lang="pug">
header.header
  .header__wrapper.container
    .header__start
      a(href="#").header__logo
        img(src="assets/icon-logo.svg" width="40" height="40" alt="some desctr text")
        span.header__logo-text Global
    .header__middle
      nav.header__nav.nav
        button(type="button" aria-expanded="false" @click="toggelMenu").nav__toggle
          span.nav__toggle-icon
        ul.nav__wrapper
          li(v-for="link, index  in headerLinks" :key="index").nav__item
            NuxtLink(:to="`${link.href}`" class="nav__item-link" :class="{ 'nav__item-link--active' : index == 0}") {{ link.text }}
    a(href="").header__sing-up-btn.main-btn Sign Up
</template>

<style lang="scss" scoped>
.header {
  position: relative;
  z-index: 2;
  background-color: transparent;
  @media (min-width: $width-tablet) {
    padding-top: 26px;
    padding-bottom: 30px;
  }
}

.header__wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 1rem;
  padding-bottom: 1rem;
}

@media (min-width: $width-tablet) {
  .header__wrapper {
    padding-top: 0;
    padding-bottom: 0;
  }
}
.header__middle {
  @media (min-width: $width-tablet) {
    margin-left: 22px;
  }
}
@media (max-width: $width-tablet) {
  .header__end {
    padding-right: 4rem;
  }
}
.header__logo {
  display: grid;
  align-items: center;
  justify-content: start;
  grid-template-columns: auto auto;
  gap: 8px;
}
.header__logo-text {
  font-weight: 500;
  font-size: 25px;
  line-height: 38px;
  color: #2d2d2d;
}
@media (min-width: $width-tablet) {
  .nav__wrapper {
    display: flex;
    gap: 8px;
    align-items: center;
    flex-wrap: wrap;
    justify-content: flex-start;
  }
}

@media (max-width: $width-tablet) {
  .nav__wrapper {
    position: absolute;
    top: 100%;
    right: 0;
    left: 0;
    z-index: 5;
    background-color: rgba($color: #ecf7f3, $alpha: 0.7);
    backdrop-filter: blur(4px);
    visibility: hidden;
    opacity: 0;
    transform: translateX(-100%);
    transition: transform $transition, opacity $transition,
      visibility $transition;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .nav__wrapper.active {
    visibility: visible;
    opacity: 1;
    transform: translateX(0);
  }
}
.nav__item-link {
  font-size: 2rem;
  padding: 5px 18px;
  display: block;
  line-height: 24px;
  color: #4a4a4a;
  position: relative;

  &::after {
    width: 0px;
    content: "";
    transition: width 0.3s ease 0s;
    position: absolute;
    bottom: 2px;
    left: 50%;
    transform: translateX(-50%);
    height: 1.5px;
    background-color: #fb9b69;
  }

  &:hover {
    color: #000;
    &::after {
      width: 21px;
    }
  }
  @media (min-width: $width-tablet) {
    font-size: 1rem;
  }
}

.nav__item-link--active {
  &::after {
    width: 21px;
  }
}
.nav__toggle {
  display: none;
}
.nav__toggle-icon {
  width: 42px;
  height: 4px;
  border-radius: 4px;
  background-color: $primary-color;
}
.nav__toggle[aria-expanded="true"] {
  gap: 4px;

  &::after,
  &::before {
    width: 25%;
  }
}

@media (max-width: $width-tablet) {
  .nav__toggle {
    display: grid;
    justify-content: stretch;
    gap: 8px;
    align-items: center;
    width: 42px;
    position: relative;
    background-color: transparent;
    transition: gap 0.3s ease 0s;

    &::after,
    &::before {
      display: block;
      content: "";
      width: 42px;
      height: 4px;
      border-radius: 4px;
      background-color: $primary-color;
      transition: width 0.3s ease 0s;
    }
    &::after {
      justify-self: end;
      width: 32px;
    }
  }
}

.header__sing-up-btn {
  flex: 0 0 auto;
  white-space: nowrap;
  padding: 9px 23px;
  line-height: 24px;
  font-weight: 500;
}
</style>

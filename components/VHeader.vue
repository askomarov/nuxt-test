<script>
import { ref } from "vue";

export default {
  setup() {
    const isOpened = ref(false);
    const headerLinks = [
      {
        href: "/",
        text: "Player",
      },
      {
        href: "/about",
        text: "About",
      },
      {
        href: "/team",
        text: "Team",
      },
      {
        href: "/contact",
        text: "Contact",
      },
      // search
      {
        href: "/search",
        text: "TV Shows",
      },
    ];
    return { isOpened, headerLinks };
  },
};
</script>

<template lang="pug">
header.header
  .header__wrapper.container
    .header__start
      NuxtLink(to="/").header__logo
        img(src="assets/icon-logo.svg" width="40" height="40" alt="some desctr text")
        span.header__logo-text Global
    .header__middle
      nav.header__nav.nav
        button(type="button" :aria-expanded="isOpened" @click="isOpened = !isOpened").nav__toggle
          span.nav__toggle-icon
        ul.nav__wrapper
          li(v-for="link, index  in headerLinks" :key="index").nav__item
            NuxtLink(:to="`${link.href}`" class="nav__item-link") {{ link.text }}
    a.header__sing-up-btn.main-btn Sign Up
</template>

<style lang="scss" scoped>
.header {
  position: relative;
  z-index: 2;
  background-color: transparent;
  @media (min-width: 768px) {
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

@media (min-width: 768px) {
  .header__wrapper {
    padding-top: 0;
    padding-bottom: 0;
  }
}
.header__middle {
  @media (min-width: 768px) {
    margin-left: 22px;
  }
}
@media (max-width: 768px) {
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
@media (min-width: 768px) {
  .nav__wrapper {
    display: flex;
    gap: 8px;
    align-items: center;
    flex-wrap: wrap;
    justify-content: flex-start;
  }
}

@media (max-width: 768px) {
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
    transition: transform 0.3s ease 0s, opacity 0.3s ease 0s,
      visibility 0.3s ease 0s;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
}
.nav__item-link {
  font-size: 1rem;
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
}

.nav__item-link.router-link-exact-active {
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
  background-color: var(--primary-color);
}
.nav__toggle[aria-expanded="true"] {
  gap: 4px;

  &::after,
  &::before {
    width: 25%;
  }
  ~ .nav__wrapper {
    visibility: visible;
    opacity: 1;
    transform: translateX(0);
  }
}

@media (max-width: 768px) {
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
      background-color: var(--primary-color);
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

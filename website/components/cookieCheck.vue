<template>
    <transition appear name="fade">
      <div class="cookieCheck" v-show="!accepted">
        <div class="columns">
          <div class="column is-one-third-desktop is-offset-one-third-desktop is-half-tablet is-offset-one-quarter-tablet">
            <div class="content">
                <slot>This website uses cookies for reasons of functionality, convenience, and statistics. For information on deleting the cookies, please consult your browser’s help function.</slot>
                <slot>Some Eclipse Foundation pages use cookies to better serve you when you return to the site. You can set your browser to notify you before you receive a cookie or turn off cookies. If you do so, however, some areas of some sites may not function properly. To read Eclipse Foundation Privacy Policy <a href="">click here.</a></slot>
            </div>
            <div class="buttons">
              <div class="button is-success" @click="accept">Got it!</div>
            </div>
          </div>
        </div>
      </div>
    </transition>
</template>

<script>
import * as Cookie from 'tiny-cookie';

export default {
  data() {
    return {
      accepted: true
    };
  },
  created() {
    this.lSAvaialable = true;
    this.cookieName = 'eclipse_cookieconsent_status';
    try {
      const name = 'localStorateTest';
      localStorage.setItem(name, name);
      localStorage.removeItem(name);
    } catch(e) {
      this.lSAvaialable = false;
    }
    this.accepted = this.hasAccepted();
  },
  methods: {
    hasAccepted() {
      if(process.server) {
        return true;
      }
      return this.lSAvaialable
        ? localStorage.getItem(this.cookieName)
        : Cookie.get(this.cookieName);
    },
    accept() {
      const now = (new Date()).toISOString();
      this.lSAvaialable
        ? localStorage.setItem(this.cookieName, now)
        : Cookie.set(this.cookieName, now);
      this.accepted = this.hasAccepted();
    }
  }
};
</script>

<style lang="scss">
@import "~assets/variables.scss";

.cookieCheck {
  position: fixed;
  width: 100%;
  bottom: 0;
  .column {
    background-color: $bosch-windsor;
    border: 1px solid $bosch-lima;
    color: white;
    box-shadow: 5px 5px 10px 3px $bosch-aluminium;
    .content {
      padding: 0.5rem;
      margin-bottom: 0;
    }
    .buttons {
      padding-left: 0.5rem;
      margin-bottom: 0;
    }
  }
  &.fade-enter-active, &.fade-leave-active {
    transition: opacity .5s;
  }
  &.fade-enter, &.fade-leave-to {
    opacity: 0;
  }
}

@media screen and (min-width: 769px) {
  .cookieCheck {
    bottom: 2rem;
    .column {
      border-radius: 1rem;
    }
  }
}
@media print {
  .cookieCheck {
    display: none;
  }
}
</style>

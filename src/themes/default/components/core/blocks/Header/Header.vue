<template>
<div class="header">
    <header class="brdr-bottom bg-white brdr-c-alto"  :class="{ 'is-visible': navVisible }">
        <div class="container">
            <div class="row between-xs middle-xs px15" v-if="!isCheckout">
                <div class="col-md-3 middle-xs">
                    <div>
                        <hamburger-icon class="p15 icon bg-lightgray"/>
                    </div>
                </div>
                <div class="col-md-6 center-xs">
                    <div>
                        <logo width="36px" height="41px"/>
                    </div>
                </div>
                <div class="col-md-3 end-xs">
                    <div class="inline-flex">
                        <search-icon class="p15 icon hidden-xs" />
                        <wishlist-icon class="p15 icon hidden-xs" />
                        <microcart-icon class="p15 icon" />
                        <account-icon class="p15 icon hidden-xs" />
                    </div>
                </div>
            </div>
             <div class="row between-xs middle-xs px15 py5" v-if="isCheckout">
                <div class="col-md-3 middle-xs">
                    <div>
                        <router-link to="/" class="c-lightgray-secondary">Return to shopping</router-link>
                    </div>
                </div>
                <div class="col-md-6 center-xs">
                    <logo width="36px" height="41px"/>
                </div>
                <div class="col-md-3 end-xs">
                    <div>
                        <a href="#" @click="gotoAccount" class="c-lightgray-secondary">Login to your account</a>
                    </div>
                </div>
            </div>
        </div>
    </header>
    <div class="header-placeholder">
    </div>
</div>
</template>

<script>
import { coreComponent } from 'lib/themes'
import { mapState } from 'vuex'

import Logo from '../../Logo.vue'

import AccountIcon from './AccountIcon.vue'
import MicrocartIcon from './MicrocartIcon.vue'
import HamburgerIcon from './HamburgerIcon.vue'
import SearchIcon from './SearchIcon.vue'
import WishlistIcon from './WishlistIcon.vue'

export default {
  data () {
    return {
      isCheckout: false,
      navVisible: true
    }
  },
  created () {
    if (this.$route.path === '/checkout') {
      this.isCheckout = true
    }
  },
  beforeMount () {
    let didScroll
    let lastScrollTop = 0
    const delta = 5
    const navbarHeight = 54

    window.addEventListener('scroll', () => {
      didScroll = true
    })

    setInterval(() => {
      if (didScroll) {
        hasScrolled.apply(this)
        didScroll = false
      }
    }, 250)

    function hasScrolled () {
      let st = document.scrollingElement.scrollTop

      if (Math.abs(lastScrollTop - st) <= delta) {
        return
      }
      if (st > lastScrollTop && st > navbarHeight) {
        this.navVisible = false
      } else {
        this.navVisible = true
      }
      lastScrollTop = st
    }
  },
  watch: {
    '$route.path': function () {
      if (this.$route.path === '/checkout') {
        this.isCheckout = true
        this.menuFixed = true
      } else {
        this.isCheckout = false
        this.menuFixed = false
      }
    }
  },
  computed: {
    ...mapState({
      isOpenLogin: state => state.ui.signUp
    })
  },
  methods: {
    gotoAccount () {
      this.$store.commit('ui/setSignUp', !this.isOpenLogin)
    }
  },
  components: {
    AccountIcon,
    MicrocartIcon,
    HamburgerIcon,
    WishlistIcon,
    SearchIcon,
    Logo
  },
  mixins: [coreComponent('core/blocks/Header/Header')]
}
</script>

<style scoped>
    header {
        position: fixed;
        height: 54px;
        top: -54px;
        width: 100%;
        z-index: 2;
        transition: top 0.2s ease-in-out;
    }
    .icon {
        opacity: 0.6;
    }
    .icon:hover {
        background-color: #F2F2F2;
        cursor: pointer;
        opacity: 1;
    }
    .header-placeholder {
        height: 54px;
    }
    .links {
        text-decoration: undeline;
    }
    .is-visible {
        top: 0 !important;
    }
</style>

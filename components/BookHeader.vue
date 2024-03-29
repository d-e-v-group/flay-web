<template>
  <div>
    <div class="section-spacer"></div>
    <div class="section-spacer half"></div>
    <header class="book-header" data-st-animate_in_class>
      <div class="inner">
        <figure class="image">
          <FadeImage
            v-if="image"
            :srcset="image.node.srcSet"
            :sizes="image.node.sizes"
            :src="image.node.mediaItemUrl"
            :alt="image.node.altText"
          />
          <div class="message">All Books Are Signed!</div>
        </figure>
        <div class="content">
          <div class="price" v-if="price">${{ price }}</div>
          <h1 v-if="header">{{ header }}</h1>
          <div v-if="content" v-html="content"></div>
          <p>All books are autographed by Bobby prior to shipping.</p>

          <form ref="mainForm" class="form" target="_blank" action="https://www.paypal.com/cgi-bin/webscr" method="post">
            <input type="hidden" name="business" value="cookbooks@boldfood.net" />
            <input type="hidden" name="cmd" value="_cart" />
            <input type="hidden" name="add" value="1" />
            <input v-if="header" type="hidden" name="item_name" :value="header" />
            <input v-if="price" type="hidden" name="amount" :value="price" />
            <input type="hidden" name="currency_code" value="USD" />
          </form>

          <form ref="hackForm" target="_self" action="https://www.paypal.com/cgi-bin/webscr" method="post">
            <input type="hidden" name="business" value="cookbooks@boldfood.net" />
            <input type="hidden" name="cmd" value="_cart" />
            <input type="hidden" name="display" value="1" />
          </form>

          <div class="cta" v-if="link">
            <a class="button primary" @click="submitMain" target="_blank" tabindex="0"
              >Buy now with <img src="/images/logo-paypal.svg" alt=""
            /></a>
          </div>
          <!-- <ul class="cart-controls">
            <li @click="submitHack">View Cart</li>
            <li @click="submitHack">Checkout</li>
          </ul> -->
        </div>
      </div>
    </header>
  </div>
</template>

<script>
import scrollTriggerHub from '~/mixins/ScrollTriggerHub'

export default {
  mixins: [scrollTriggerHub],
  props: {
    image: null,
    bigLetter: null,
    price: null,
    header: null,
    content: null,
    link: null,
  },
  data() {
    return {}
  },
  computed: {
    getBigLetter() {
      if (this.bigLetter == null) {
        return this.header.substr(0, 1)
      }
    },
  },
  mounted() {},
  methods: {
    submitMain: function() {
      this.$refs.mainForm.submit()
    },
    submitHack: function() {
      this.$refs.hackForm.submit()
    },
  },
}
</script>

<style lang="scss" scoped>
.book-header {
  padding: 0 10px;
  overflow: hidden;

  @include breakpoint(medium) {
    min-height: none;
  }

  .inner {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    height: 100%;
    padding: 44px 8vw 0;

    @include breakpoint(medium) {
      flex-direction: column;
      align-items: center;
      padding: 54px 0 0;
    }

    .image {
      position: relative;
      width: 41%;
      flex-shrink: 0;
      opacity: 0;
      transform: translateY(-30px);
      transition: 3s transform, 1s opacity;

      @include breakpoint(large) {
        width: 45%;
      }

      @include breakpoint(medium) {
        width: 65vw;
        min-width: 241px;
      }

      img {
        position: relative;
        display: block;
        width: 100%;
        height: auto;
        border: 1px solid $black;
      }

      .message {
        position: absolute;
        top: 60px;
        right: 0;
        border: 1px solid $black;
        background: $white;
        font-size: 14px;
        padding: 0.7em 0.7em 0.5em;
        user-select: none;
        text-align: center;
        opacity: 0;
        transform: translate(50%, 100%);
        transition: 3s transform, 1s opacity;

        @include breakpoint(large) {
          top: 100%;
          right: 50%;
          transform: translate(50%, -50%);
        }
      }
    }

    .content {
      width: 41%;
      flex-shrink: 0;
      opacity: 0;
      transform: translateX(30px);
      transition: 3s transform, 1s opacity;

      @include breakpoint(large) {
        width: 45%;
      }

      @include breakpoint(medium) {
        width: auto;
        padding: 100px 25px 60px;
        text-align: center;
      }

      @include breakpoint(small) {
        padding: 70px 25px 60px;
      }

      .price {
        font-size: 14px;
        margin-bottom: 2.5em;
        font-weight: bold;
      }

      h1 {
        @extend .h3;
      }

      .form {
        display: flex;
        flex-direction: column;
        width: 100%;
        margin-bottom: 30px;

        @include breakpoint(medium) {
          margin-top: 70px;
        }

        .form_inner {
          padding: 0;
          display: flex;
          justify-content: space-between;
          border-bottom: 1px solid $black;
          padding-bottom: 15px;
          margin-bottom: 10px;

          @include breakpoint(small) {
            flex-direction: column;
            align-items: flex-start;
            border-bottom: none;
            margin-bottom: 0;
          }

          input {
            font-size: 14px;
            color: $black;
            border: none;
            outline: none;
            background: transparent;
            appearance: none;
            border-radius: 0;
            flex: 1;
            font-size: 16px;
            @include breakpoint(small) {
              font-size: 15px;
              width: 100%;
              border-bottom: 1px solid $black;
              padding-bottom: 0.6em;
            }

            &::placeholder {
              font-size: 14px;
              color: #c1c1c1;
            }
          }

          button {
            font-size: 20px;
            border: none;
            outline: none;
            background: transparent;
            appearance: none;
            padding: 0;
            @include fork_hover;
            font-weight: bold;
            @include breakpoint(small) {
              font-size: 15px;
              margin-top: 10px;
            }
          }
        }

        span {
          text-align: right;
          font-size: 14px;
          color: #c1c1c1;
          pointer-events: none;

          @include breakpoint(small) {
            position: relative;
            transform: translateY(calc(-200% - 3px));
          }
        }
      }

      .cta {
        margin-bottom: 30px;
        .button {
          @include fork_hover;
          width: 100%;
          display: flex;
          align-items: center;
          justify-content: center;
          font-weight: bold;
          padding-left: 10px;
          padding-right: 10px;

          @include breakpoint(small) {
            font-weight: bold;
          }

          img {
            margin-left: 15px;
          }
        }
      }
      .cart-controls {
        li {
          margin-bottom: 14px;
          @include fork_hover;
        }
      }
    }
  }

  &.animate-in {
    .inner {
      .image {
        opacity: 1;
        transform: translateY(0px);

        .message {
          opacity: 1;
          transform: translate(50%, 0px);

          @include breakpoint(large) {
            transform: translate(50%, -50%);
          }
        }
      }

      .content {
        opacity: 1;
        transform: translateX(0px);
      }
    }
  }
}
</style>

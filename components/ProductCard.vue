<template>
  <article class="card" v-bind:class="{ visible: visible }" v-on:click="$router.push({ path: `/shop/${data.slug}` })">
    <div to="/shop" class="inner">
      <div class="content">
        <div class="content-wrapper">
          <!-- Title -->
          <h4>{{ data.title }}</h4>
          <!-- Image -->
          <div class="image">
            <FadeImage
              v-if="data.featuredImage"
              :srcset="data.featuredImage.node.srcSet"
              :sizes="data.featuredImage.node.sizes"
              :src="data.featuredImage.node.mediaItemUrl"
              :alt="data.featuredImage.node.altText"
              :width="data.featuredImage.node.mediaDetails.width"
              :height="data.featuredImage.node.mediaDetails.height"
            />
          </div>
          <!-- Description -->
          <div v-if="data.Descriptions.thumbnailDescription">
            <p>{{ data.Descriptions.thumbnailDescription }}</p>
          </div>
          <div v-else-if="data.Descriptions.featuredDescription">
            <p>{{ data.Descriptions.featuredDescription }}</p>
          </div>
          <div v-else v-html="data.content.substr(0, 100) + '...'"></div>
          <!-- Price -->
          <div class="price">${{ data.ProductPrice.productPrice }}</div>
          <div class="cta">
            <nuxt-link :to="`/shop/${data.slug}`" class="button primary" v-if="data.CardLink.cardLinkText">{{
              data.CardLink.cardLinkText
            }}</nuxt-link>
          </div>
        </div>
      </div>
    </div>
  </article>
</template>

<script>
export default {
  props: {
    data: Object,
  },
  data() {
    return {
      visible: false,
    }
  },
  computed: {},
  mounted() {
    setTimeout(() => {
      this.visible = true
    }, 250)
  },
  methods: {},
}
</script>

<style lang="scss" scoped>
.card {
  position: relative;
  width: 33.333333%;
  flex-shrink: 0;
  padding: 5px;
  @include fork_hover;
  opacity: 0;
  transform: translateY(10px);
  transition: 3s transform, 1s opacity;

  &.visible {
    opacity: 1;
    transform: translateY(0px);
  }

  @include breakpoint(medium) {
    width: 50%;
  }

  @include breakpoint(small) {
    width: 100%;
    padding: 0 0 10px;
  }

  .inner {
    display: flex;
    height: 100%;
    min-height: 610px;

    @include breakpoint(small) {
      min-height: 465px;
    }

    &:hover {
      .content {
        .content-wrapper {
          .image {
            img {
              transform: translate(-10px, -10px);
              filter: grayscale(0%);
            }
          }
        }
      }
    }

    .content {
      display: flex;
      flex-direction: column;
      justify-content: center;
      text-align: center;
      border: 1px solid $black;
      width: 100%;

      .content-wrapper {
        padding: 5vw;
        display: flex;
        flex-direction: column;
        align-items: center;

        h4 {
          @include breakpoint(small) {
            margin-bottom: 1.3em;
          }
        }

        .image {
          position: relative;
          width: 130px;
          height: auto;
          margin-bottom: 20px;

          @include breakpoint(small) {
            width: 92px;
            margin: 0 auto 20px;
          }

          &:after {
            content: '';
            display: block;
            position: absolute;
            top: 0;
            left: 0;
            bottom: 1px;
            right: 1px;
            border: 1px solid $black;
            z-index: 0;
          }

          img {
            position: relative;
            display: block;
            width: 100%;
            height: auto;
            transform: translate(0px, 0px);
            filter: grayscale(100%);
            z-index: 1;
            transition: 0.5s transform, 1s filter linear;

            @include breakpoint(small) {
              transform: translate(-10px, -10px);
              filter: grayscale(0%);
              border: 1px solid $black;
            }
          }
        }
        ::v-deep p {
          font-size: 14px;
        }
        .price {
          font-size: 14px;
          font-weight: bold;
          margin-top: 1em;
          margin-bottom: 2em;
        }
        .cta {
        }
      }
    }
  }

  &:nth-of-type(9n + 1) {
    .content {
      background-color: $soft-blue;
    }
  }

  &:nth-of-type(9n + 2) {
    .content {
      background-color: $soft-lime-green;
    }
  }

  &:nth-of-type(9n + 3) {
    .content {
      background-color: $light-green;
    }
  }

  &:nth-of-type(9n + 4) {
    .content {
      background-color: $strong-lime-green;
    }
  }

  &:nth-of-type(9n + 5) {
    .content {
      background-color: $grayish-yellow;
    }
  }

  &:nth-of-type(9n + 6) {
    .content {
      background-color: $soft-blue2;
    }
  }

  &:nth-of-type(9n + 7) {
    .content {
      background-color: $dark-lime-green;
    }
  }

  &:nth-of-type(9n + 8) {
    .content {
      background-color: $dark-green;
    }
  }

  &:nth-of-type(9n + 9) {
    .content {
      background-color: $bright-blue;
    }
  }
}
</style>

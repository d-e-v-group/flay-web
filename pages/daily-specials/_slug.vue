<template>
  <div v-if="dailySpecial" class="daily-special-single-page">
    <PrimaryHeader
      :image="dailySpecial.featuredImage != null ? dailySpecial.featuredImage.node : null"
      :preheader="cats"
      :header="dailySpecial.title"
      :content="dailySpecial.content"
      :linkType="dailySpecial.HeaderCTALink.linkType"
      :link="dailySpecial.HeaderCTALink.link != null ? dailySpecial.HeaderCTALink.link : null"
    />

    <div class="section-spacer hide-mobile"></div>
    <PageBuilder :content_blocks="dailySpecial.PageBuilder.contentBlocks" :type="'DailySpecial'" />
  </div>
</template>

<script>
import meta from '~/plugins/meta.js'
import { gql } from 'nuxt-graphql-request'
import { basics, image, featured_image, link, page_builder } from '~/gql/common'
import scrollTriggerHub from '~/mixins/ScrollTriggerHub'
const gql_content = `
  ${basics}
  ${featured_image} 
  seo {
    metaDesc
    title
    opengraphImage {
      sourceUrl
    }
  }
  ${page_builder('DailySpecial')}
  HeaderCTALink {
    linkType
    link {
      target
      title
      url
    }
  }
  categories {
    edges {
      node {
        name
        slug
      }
    }
  } 
`
export default {
  mixins: [scrollTriggerHub],
  data() {
    return {
      cats: String,
    }
  },
  async asyncData({ $graphql, route }) {
    const post_uri = route.params.slug

    const query = gql`
      query PostQuery ($uri: ID!) {
        dailySpecial(id: $uri, idType: URI) {
          ${gql_content}
          preview {
            node {
              ${gql_content}
            }
          } 
        }
      }
    `
    const variables = { uri: post_uri }
    let { dailySpecial } = await $graphql.default.request(query, variables)
    if (route.query && route.query.preview && dailySpecial.preview) {
      dailySpecial = dailySpecial.preview.node
    }
    return { dailySpecial }
  },
  head() {
    if (this.dailySpecial && this.dailySpecial.seo) {
      return {
        title: this.dailySpecial.seo.title,
        meta: meta(this.dailySpecial.seo),
      }
    }
  },
  mounted() {
    if (this.dailySpecial) {
      this.cats = ''
      for (let cat in this.dailySpecial.categories.edges) {
        this.cats += `${this.dailySpecial.categories.edges[cat].node.name}, `
      }
      this.cats = this.cats.substring(0, this.cats.length - 2)
    }
  },
}
</script>

<style lang="scss" scoped>
.daily-special-single-page {
  .daily-special-content {
    .inner {
      display: flex;
      flex-direction: column;

      .image {
        width: 100%;
        max-width: 810px;
        margin: 0 auto;
        padding: 0 10px;

        img {
          display: block;
          width: 100%;
          height: auto;
          border: 1px solid $black;
        }
      }

      blockquote {
        display: flex;
        width: 100%;
        max-width: 1035px;
        margin: 60px auto;
        padding: 0 10px;

        @include breakpoint(medium) {
          flex-direction: column;
        }

        &:before {
          content: '';
          position: relative;
          display: block;
          flex-shrink: 0;
          width: 244px;
          height: 197px;
          background-image: url('/images/quotes.png');
          background-size: 100%;
          background-repeat: no-repeat;

          @include breakpoint(medium) {
            left: -37px;
            width: 172px;
            height: 139px;
          }
        }

        p {
          @extend .h3;
          margin: 1.7em 0 0 70px;

          @include breakpoint(medium) {
            margin: 1.3em 0 0 0;
            text-align: center;
          }
        }
      }

      .regular-content {
        width: 100%;
        max-width: 810px;
        margin: 0 auto;
        padding: 0 10px;

        @include breakpoint(small) {
          text-align: center;
        }
      }
    }
  }
}
</style>

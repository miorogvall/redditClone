<template>
    <div>
        <div
        class="item-text"
        v-if="singlePost.data.selftext_html !== 'null' && singlePost.data.selftext.length > 0">
        <div v-html="singlePost.data.selftext_html"></div>
      </div>
      <div v-if="postType == 'image' && singlePost.data.url !== undefined">
        <imageContent :data="singlePost.data"/>
      </div>

      <div v-if="postType == 'link' && singlePost.data.preview.reddit_video_preview !== undefined && singlePost.data.preview.reddit_video_preview.fallback_url !== undefined">
        <linkContent :data="singlePost.data"/>
      </div>

      <div v-if="postType == 'rich:video' && singlePost.data.media.oembed.html">
        <mediaContent :data="singlePost.data"/>
      </div>

      <div v-if="postType == 'hosted:video'">
        <videoContent :data="singlePost.data"/>
      </div>

    </div>

</template>
<script>

import mediaContent from './content-types/mediaContent.vue'
import linkContent from './content-types/linkContent.vue'
import imageContent from './content-types/imageContent.vue'
import videoContent from './content-types/videoContent.vue'

export default {
  name: 'postContent',
  components: {
    mediaContent,
    linkContent,
    videoContent,
    imageContent
  },
  props: {
    singlePost: Object,
    postType: String
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  @import "../../scss/_variables.scss";

      .item-text {
            text-align: left;
            max-width: 660px;
            width: 90%;
            margin: auto;
            margin-top: 50px;
            display: block;
            word-break: break-word;
            color: $baby-blue;
            line-height: 23px;
            margin-top: 70px;
            margin-bottom: 70px;

            /deep/ p {
                margin-top: 10px;
            }
            /deep/ h1 {
                margin-top: 10px;
                margin-bottom: 20px;
            }

            /deep/ h2 {
                margin-top: 10px;
            }

            /deep/ h3 {
                margin-top: 10px;
            }

            /deep/ h4 {
                margin-top: 10px;
            }

            /deep/ h5 {
                margin-top: 10px;
            }

            /deep/ h6 {
                margin-top: 10px;
            }

            /deep/ a {
                margin-top: 10px;
            }

            /deep/ hr {
                margin-top: 18px;
                margin-bottom: 18px;
                display: block;
            }
          }

</style>

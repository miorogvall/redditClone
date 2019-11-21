<template>
    <div>
        <div class="link" v-if="dataPost.preview.reddit_video_preview !== undefined && dataPost.preview.reddit_video_preview.fallback_url !== undefined">
            <video
            class="video"
            controls
            autoplay="true"
            loop="loop"
            :src="dataPost.preview.reddit_video_preview.fallback_url">
            </video>
            
        </div>
        <div class="link" v-if="dataPost.preview.images[0].source.url !== undefined && dataPost.preview.reddit_video_preview == undefined">
            <img class="image" :src="dataPost.preview.images[0].source.url">
        </div>
    </div>
</template>
<script>

export default {
  name: 'postContent',
  props: {
    dataPost: Object
  }
}

</script>

      <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

    @import "../../../scss/_variables.scss";

    .link {
        color: $white;
        margin-top: 70px;
        margin-bottom: 70px;
    }

    .video {
        max-height: 400px;
    }

    .image {
        max-height: 500px;
    }

</style>

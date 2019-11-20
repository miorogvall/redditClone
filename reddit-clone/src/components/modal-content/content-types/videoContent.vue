<template>
    <div>
      <video class="video" controls autoplay="true" loop="loop">
          <source :src="data.media.reddit_video.fallback_url">
      </video>
    </div>
</template>
<script>

export default {
  name: 'videoContent',
  props: {
    data: Object
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  @import "../../../scss/_variables.scss";

  .video  {
          margin-top: 80px;
          height: 400px;
          outline: none;
          margin-top: 70px;
          margin-bottom: 70px;
      }

</style>

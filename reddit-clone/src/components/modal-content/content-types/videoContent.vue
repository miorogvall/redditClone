<template>
    <div>
      <video class="video" controls autoplay="true" loop="loop">
          <source :src="data.media.reddit_video.fallback_url">
      </video>
    </div>
</template>
      <script>
    
      import axios from 'axios'
      export default {
        name: 'videoContent',
        props: {
            data: Object
        },
        methods: {
        },
        computed: {
      }
    }
    
    </script>
    
      <!-- Add "scoped" attribute to limit CSS to this component only -->
      <style scoped lang="scss">
    
            $black-color: #272727;
            $white: #fdfdfd;
            $main-color: #ffe06d;
            $secondary-color: #20203c;
            $faded-red: #a23354;
            $baby-blue: #c8d8f5;

            .video  {
                    margin-top: 80px;
                    height: 400px;
                    outline: none;
                    margin-top: 70px;
                    margin-bottom: 70px;
                }
    
    </style>
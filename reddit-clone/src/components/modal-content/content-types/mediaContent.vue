<template>
        <div class="media" v-html="data.media.oembed.html">
    </div>
</template>
      <script>
    
      import axios from 'axios'
      export default {
        name: 'mediaContent',
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

            .media {
                    margin-top: 70px;
                    margin-bottom: 70px;
                }
    
    </style>
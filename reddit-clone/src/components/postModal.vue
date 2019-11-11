<template>
        <div>
            <div class="overlay is-closed" >
                <div class="wrapper" v-if="dataRecieved !== undefined">
                    <div class="item-title"><h1><span>{{dataRecieved.data[0].data.children[0].data.title}}</span></h1></div>
                  <div class="item-text" v-if="this.dataRecieved.data[0].data.children[0].data.selftext_html.length > 0 && this.dataRecieved.data[0].data.children[0].data.selftext_html !== 'null'" v-html="dataRecieved.data[0].data.children[0].data.selftext_html"></h1></div>
                    <div class="media" v-if="this.dataRecieved.data[0].data.children[0].data.post_hint == 'rich:video'" v-html="this.dataRecieved.data[0].data.children[0].data.media.oembed.html"></div>
                   <div v-if="this.dataRecieved.data[0].data.children[0].data.post_hint == 'image'">
                        <img class="image" v-bind:src="dataRecieved.data[0].data.children[0].data.url">
                    </div>
                    <div v-if="this.dataRecieved.data[0].data.children[0].data.post_hint == 'hosted:video'">
                        <video class="video" controls autoplay="true" loop="loop">
                            <source :src="dataRecieved.data[0].data.children[0].data.media.reddit_video.fallback_url">
                        </video>
                    </div>
                    <div v-if="this.dataRecieved.data[0].data.children[0].data.post_hint == 'link'" class="link" v-html="this.dataRecieved.data[0].data.children[0].data.media_embed.content"></div>
                    <div class="comments">
                        <div class="comments-header"></div>
                    </div>
                </div>
                <div class="close" v-on:click="closeModal">X</div>
            </div>
        </div>
     </template>
     
     
     <script>
        export default {
            name: 'postModal',
            props: {
                dataRecieved: Object,
            },
            created () {
            },
            methods: {
                closeModal: function(event) {
                    // provisional solution, not proud of it...
                    let overlay = document.getElementsByClassName('overlay')
                    let body = document.getElementById('body')
                    overlay['0'].classList.add('is-closed')
                    overlay['0'].classList.remove('is-open')
                    body.classList.add('modal-closed')
                    body.classList.remove('modal-open')
                    console.log(this.dataRecieved)
                    let video = document.querySelector('video.video')
                    let iframe = document.querySelector(".overlay iframe")
                    if (iframe) {
                        var iframeSrc = iframe.src;
                        iframe.src = iframeSrc;
                    }
                    if (video) {
                        video.pause();
                    }
                    console.log(this.dataRecieved)
                }
            },
}
     </script>
    
      <style scoped lang="scss">
          
        $black-color: #272727;
        $white: #fdfdfd;
        $main-color: #ffe06d;
        $secondary-color: #20203c;
        $faded-red: #a23354;
        $baby-blue: #c8d8f5;

          .overlay{
              position: fixed;
              left: 0;
              right: 0;
              top: 0;
              bottom: 0;
              background: $secondary-color;
              z-index: 99999999;
              overflow: auto;
              
              &.is-open {
                display: block;
              }
              &.is-closed {
                display: none;

              }

              .close {
                position: absolute;
                right: 0;
                top: 0;
                padding: 8px 18px;
                background: $baby-blue;
                color: $faded-red;
                font-family: "Montserrat";
                font-weight: bold;
                font-size: 30px;
                cursor: pointer;
                transition: all 0.15s ease;

                &:hover {
                    color: $baby-blue;
                    background-color: $faded-red;

                }


              }
          }

          .wrapper {

                .item-title {
                    width: 80%;
                    max-width: 1240px;
                    margin: auto;
                    overflow: hidden;
                    padding-left: 10px;
                    margin-top: 110px;
                    margin-bottom: 80px;

                    h1 {
                        span {
                            color: $secondary-color;
                            background-color: $main-color;
                            text-transform: capitalize;
                            font-style: italic;
                            font-size: 30px;
                            line-height: 2;
                            padding: 3px 0px;
                            display: inline;
                            box-shadow: 10px 0 0 $main-color, -10px 0 0 $main-color;
                        }
                    }
                }

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

                .media {
                    margin-top: 50px;
                }
                .image {
                    margin: auto;
                    margin-top: 80px;
                    max-width: 700px;
                    max-height: 400px;
                    width: auto;
                }
                .video  {
                    margin-top: 80px;
                    height: 400px;
                    outline: none;
                }

                .link {
                    color: $white;
                }
                .comments {
                    width: 100%;
                    min-height: 800px;
                    background: #141422;

                    .comments-header {
                        color: $faded-red;
                        font-family: 'overwatch-italic';
                        font-size: 30px;
                        width: 100%;
                        background: $baby-blue;
                        padding: 11px 20px;
                        margin-top: 50px;
                    }
                }
          }
      </style>

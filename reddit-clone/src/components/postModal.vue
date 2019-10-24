<template>
        <div>
            <div class="overlay is-closed" >
                <div class="wrapper" v-if="dataRecieved !== undefined">
                    <div class="item-title"><h1><span>{{dataRecieved.data.title}}</span></h1></div>
                    <div class="item-text" v-if="this.dataRecieved.data.selftext_html.length > 0 && this.dataRecieved.data.selftext_html !== 'null'" v-html="dataRecieved.data.selftext_html"></h1></div>
                    <div class="media" v-if="this.dataRecieved.data.post_hint == 'rich:video'" v-html="replaceURLParts"></div>
                   <div v-if="this.dataRecieved.data.post_hint == 'image'">
                        <img class="image" v-bind:src="dataRecieved.data.url">
                    </div>
                    <div v-if="this.dataRecieved.data.post_hint == 'hosted:video'">
                        <video class="video" controls autoplay="true">
                            <source :src="dataRecieved.data.media.reddit_video.fallback_url">
                        </video>
                    </div>
                    <div v-if="this.dataRecieved.data.post_hint == 'link'"class="link" v-html="replaceURLParts2"></div>
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
                    let overlay = document.getElementsByClassName('overlay')
                    let body = document.getElementById('body')
                    overlay['0'].classList.add('is-closed')
                    overlay['0'].classList.remove('is-open')
                    body.classList.add('modal-closed')
                    body.classList.remove('modal-open')
                    console.log(this.dataRecieved.data)
                }
            },
            computed: {
                replaceURLParts() {
                    let media = this.dataRecieved.data.media.oembed.html
                    let document = new DOMParser().parseFromString(media, "text/html");
                    let value = document.body.textContent

                    return value;
                },
                replaceURLParts2() {
                    let media = this.dataRecieved.data.media_embed.content
                    let document = new DOMParser().parseFromString(media, "text/html");
                    let value = document.body.textContent

                    return value;
                }
            }
}
     </script>
    
      <style scoped lang="scss">
          
        $black-color: #272727;
        $white: #fdfdfd;
        $main-color: #ffc5a1;
        $secondary-color: #37003c;
        $light-yellow: #feffa1;
        $dark-blue: #09003c;

          .overlay{
              position: fixed;
              left: 0;
              right: 0;
              top: 0;
              bottom: 0;
              background: $light-yellow;
              z-index: 1;
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
                background: $secondary-color;
                color: $main-color;
                font-family: "Montserrat";
                font-weight: bold;
                font-size: 30px;
                cursor: pointer;
                transition: all 0.15s ease;

                &:hover {
                    color: $secondary-color;
                    background-color: $main-color;

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
                    margin-top: 90px;

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
                    color: $black-color;
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
                    width: 90%;
                    margin: auto;
                    margin-top: 80px;
                    max-width: 700px;
                }
                .video  {
                    margin-top: 80px;
                    height: 400px;
                    outline: none;
                }
          }
      </style>

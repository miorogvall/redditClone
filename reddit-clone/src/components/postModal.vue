<template>
        <div>
            <div class="overlay is-closed" >
                <div class="wrapper" v-if="dataRecieved !== undefined">
                    <div class="item-title">
                        <h1><span>{{dataRecieved.data[0].data.children[0].data.title}}</span></h1>
                        <div class="upvotes">{{dataRecieved.data[0].data.children[0].data.ups}} upvotes</div>
                        <div class="link-section">
                            <a :href="dataRecieved.data[0].data.children[0].data.url" target="_blank" v-if="dataRecieved.data[0].data.children[0].data.permalink !== null">
                                <span><p class="link-text">view linked URL</p><i class="fas fa-external-link-square"></i></span>
                                
                            </a>
                            <a :href="baseUrl + dataRecieved.data[0].data.children[0].data.permalink" target="_blank">
                                <span><p class="link-text">view on reddit</p><i class="fab fa-reddit-alien"></i></span>
                            </a>
                        </div>
                    </div>
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
                    <div v-if="this.dataRecieved.data[0].data.children[0].data.post_hint == 'link'" class="link">
                        <video 
                        class="video" 
                        controls 
                        autoplay="true" 
                        loop="loop" 
                        v-if="this.dataRecieved.data[0].data.children[0].data.preview.reddit_video_preview !== undefined">
                            <source :src="this.dataRecieved.data[0].data.children[0].data.preview.reddit_video_preview.fallback_url">
                        </video>
                    </div>
                    <div class="comments">
                        <div class="comments-header">comments below</div>
                        <ul>
                            <li class="listing" v-for="comment in dataRecieved.data[1].data.children">
                                <div v-if="comment.kind !== 'more'" class="comment">
                                    <p class="author"><span class="author-name">{{comment.data.author}}</span> -  <span class="upvotes">{{comment.data.ups}} upvotes</span></p>
                                    <div class="text" v-html="comment.data.body_html"></div>
                                    <tree-menu
                                    :replyTree="comment.data.replies"
                                    v-if="comment.data.replies !== undefined"
                                    :depth="1"
                                  ></tree-menu>
                                </div>
                          </li>
                        </ul>
                    </div>
                </div>
                <div class="close" v-on:click="closeModal">X</div>
            </div>
        </div>
     </template>
     
     
     <script>
         import TreeMenu from './TreeMenu.vue'

        export default {
            name: 'postModal',
            components: {
                TreeMenu
            },
            props: {
                dataRecieved: Object,
            },
            data: function() {
                return {
                    baseUrl: 'https://www.reddit.com',
                };
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
                position: fixed;
                right: 10px;
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
                    margin: auto;
                    overflow: hidden;
                    background: repeating-linear-gradient( 45deg, #20203c, #20203c 10px, #141422 10px, #141422 29px );
                    padding: 88px 5%;

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
                            max-width: 981px;
                            margin: auto;
                        }
                    }

                    .link-section {
                        display: flex;
                        flex-direction: row;
                        justify-content: center;
                        margin-top: 40px;

                        a {
                            margin-left: 20px;
                            border-bottom: 0px;
                            display: flex;
                            justify-content: center;
                            align-items: center;
                            background: #c8d8f5;
                            padding: 6px 10px;

                            span {
                                display: flex;

                                p {
                                    margin-right: 7px;
                                    font-family: "overwatch"
                                }
                            }

                            i {
                                font-size: 22px;
                                font-family: "fontAwesome";

                                &:before {
                                    font-style: normal;
                                }
                            }
                        }
                    }

                    .upvotes {
                        color: #c8d8f5;
                        font-family: "overwatch";
                        margin-top: 22px;
                        display: inline-block;
                        font-size: 25px;
                        border-bottom: 2px solid #a23354;
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

                .media {
                    margin-top: 70px;
                    margin-bottom: 70px;
                }
                .image {
                    margin: auto;
                    margin-top: 80px;
                    max-width: 700px;
                    max-height: 400px;
                    width: auto;
                    margin-top: 70px;
                    margin-bottom: 70px;
                }
                .video  {
                    margin-top: 80px;
                    height: 400px;
                    outline: none;
                    margin-top: 70px;
                    margin-bottom: 70px;
                }

                .link {
                    color: $white;
                    margin-top: 70px;
                    margin-bottom: 70px;
                }
                .comments {
                    width: 100%;
                    min-height: 800px;
                    background: #141422;

                    .comments-header {
                        color: $faded-red;
                        font-family: 'overwatch';
                        font-size: 25px;
                        width: 100%;
                        background: $baby-blue;
                        padding: 11px 20px;
                        text-transform:uppercase;
                    }

                    ul {
                        margin-top: 50px;
                        padding-bottom: 150px;
                        margin-bottom: 0;
                    }

                    .listing {
                        width: 80%;
                        max-width: 800px;
                        margin: auto;
                        list-style-type: none;
                        text-align: left;
                        color: #ffe06d;
                        margin-bottom: 17px;

                        .comment {
                            .author {
                                font-style: italic;
                                color: #ffe06d;
                                border-bottom: 1px solid #a23354;
                                display: inline;
                                font-weight: 100;
                                font-size: 12px;
                            }

                            .text {
                                margin-top: 4px;
                                color: #c8d8f5;
                                font-size: 14px;
                                margin-bottom: 5px;
                            }
                        }
                    }
                }
          }
      </style>

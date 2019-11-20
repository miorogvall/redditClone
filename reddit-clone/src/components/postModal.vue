<template>
        <div>
            <div class="overlay is-closed">
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

                  <postContent :singlePost="this.dataRecieved.data[0].data.children[0]" :postType="this.dataRecieved.data[0].data.children[0].data.post_hint"/>

                    <div class="comments">
                        <div class="comments-header">comments below</div>
                        <ul>
                            <li class="listing" v-for="comment in dataRecieved.data[1].data.children">
                                <div v-if="comment.kind !== 'more'" class="comment">
                                    <p class="author"><span class="author-name">{{comment.data.author}}</span> | <span class="upvotes">{{comment.data.ups}} upvotes</span></p>
                                    <div class="text" v-html="comment.data.body_html"></div>
                                    <tree-comments
                                    :replyTree="comment.data.replies"
                                    v-if="comment.data.replies !== undefined"
                                    :depth="1"
                                  ></tree-comments>
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
import TreeComments from './TreeComments.vue'
import postContent from './modal-content/postContent.vue'

export default {
  name: 'postModal',
  components: {
    TreeComments,
    postContent
  },
  props: {
    dataRecieved: Object
  },
  data: function () {
    return {
      baseUrl: 'https://www.reddit.com'
    }
  },
  methods: {
    closeModal: function (event) {
      // provisional solution, not proud of it...
      let overlay = document.getElementsByClassName('overlay')
      let body = document.getElementById('body')
      overlay['0'].classList.add('is-closed')
      overlay['0'].classList.remove('is-open')
      body.classList.add('modal-closed')
      body.classList.remove('modal-open')
      let video = document.querySelector('video.video')
      let iframe = document.querySelector('.overlay iframe')
      if (iframe) {
        var iframeSrc = iframe.src
        iframe.src = iframeSrc
      }
      if (video) {
        video.pause()
      }
    }
  }
}
</script>

      <style scoped lang="scss">

          @import "../scss/_variables.scss";

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
                            border-left: 2px solid #2a2a44;
                            padding-left: 13px;

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

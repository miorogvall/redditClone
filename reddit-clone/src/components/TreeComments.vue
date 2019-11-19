<template>
        <div class="tree-comments" v-if="replyTree !== undefined && replyTree.data !== undefined">
            <div class="border" v-for="children in replyTree.data.children" v-bind:class="{ noBorder: children.kind == 'more' }">
                <div class="author-and-upvotes" v-if="children.kind !== 'more'">
                    {{children.data.author}} | {{children.data.ups}} upvotes
                    <div class="text" v-html="children.data.body_html"></div>
                    <div v-if="children.data.replies.data !== undefined">
                        <div class="border" v-if="children.data.replies.data !== undefined && children.data.replies.data.children[0].data.author !== undefined">
                            <div class="author-and-upvotes">
                                {{children.data.replies.data.children[0].data.author}} | {{children.data.replies.data.children[0].data.ups}} upvotes
                            </div>
                            <div class="text" v-html="children.data.replies.data.children[0].data.body">
                            </div>
                            <tree-comments
                            v-for="children in children.data.replies.data.children"
                            :replyTree="children.data.replies"
                            :depth="depth + 1"
                            >
                            </tree-comments>
                        </div>
                    </div>
                </div>
<!--                 <div v-if="children.kind == 'more' && children !== undefined" class="border">
                        <button v-if="replyTree.data.children[0].data.replies !== '' && replyTree.data.children[0].kind !== 'more'" v-on:click="moreComments(replyTree.data.children[0])" class="see-replies" v-show="!interactedWith">More replies <b>[+]</b></button>
                        <div v-if="interactedWith && replies" class="attach-comments border">
                            <div class="reply-fetched" v-for="fetchedReply in replies">
                                <div class="author-and-upvotes">
                                    {{fetchedReply.author}}
                                    <div class="text" v-html="fetchedReply.body"></div>
                                </div>
                            </div>
                        </div>
                    </div> -->
            </div>
    </div>
      </template>
      <script>

        import axios from 'axios'

        export default {
          props: ['replyTree', 'replies', 'depth'],
          name: 'tree-comments',
          methods: {
              moreComments: function(comment) { 
                  // using external proxy for get, see no danger since it removes the need for a proxy and backend server
                  // and its a simple get request. NEVER do this with sensitive data :)
                let url = `https://api.pushshift.io/reddit/comment/search?raw_json=1&parent_id=${comment.data.name}&limit=500`
                axios.get(url).then(response => {
                    this.interactedWith = !this.interactedWith
                    this.replies = response.data.data
                })
              }
          },
          data() {
            return {
                interactedWith: false,
            }
        }
      }
        
      </script>

<style scoped lang="scss">
          
        $black-color: #272727;
        $white: #fdfdfd;
        $main-color: #ffe06d;
        $secondary-color: #20203c;
        $faded-red: #a23354;
        $baby-blue: #c8d8f5;
        $thread-color: #2a2a44;

        .comment > .tree-comments {
            margin-left: 0px;
        }

        .text {
            margin-top: 4px;
            color: #c8d8f5;
            font-size: 14px;
            margin-bottom: 5px;
            font-weight: 400;
            font-style: normal;
        }

        .tree-comments {
           
           .border {
                border-left: 1.5px solid $thread-color;
                padding-left: 13px;
                margin-left: 10px;
           }
           
           .noBorder {
                border-left: none !important;
                padding-left: 0 !important;
                margin-left: 0 !important;
           }

            /deep/ .md {
                margin-bottom: 10px;
                color: $baby-blue;
            }

            .replies {
                margin-bottom: 10px;
                color: $baby-blue;
                color: #c8d8f5;
                font-size: 14px;
            }

            /deep/ .md p {
                color: #c8d8f5;
                font-size: 14px;
            }

            .author-and-upvotes {

                font-style: italic;
                color: $main-color;
                border-bottom: 1px solid $faded-red;
                display: inline;
                font-weight: 100;
                font-size: 12px;
                vertical-align: top;

            }

            .see-replies {
                -webkit-appearance: none;
                border: none;
                background: $baby-blue;
                color: $secondary-color;
                padding: 3px 8px;
                border-radius: 100px;
                text-transform: initial;
                font-family: inherit;
                font-size: 10px;
                font-weight: 600;
                cursor: pointer;
                transition: all 0.2s ease;
                margin-top: 6px;
                margin-bottom: 10px;
                outline: none;

                &:hover {
                    color: $secondary-color;
                    background: $main-color;

                }
            }

            .attach-comments {
                animation: fadeBG 1.7s linear;
            }

            @keyframes fadeBG {
                from {background: rgba(255,255,255,0.10)}
                to {background: transparent;}
                }

        }

</style>
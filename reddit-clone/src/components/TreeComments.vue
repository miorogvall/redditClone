<template>
        <div class="tree-comments" v-if="replyTree !== undefined && replyTree.data !== undefined">
            <div 
                class="author-and-upvotes" 
                v-if="replyTree.data !== undefined && replyTree.data.children[0].kind !== 'more'">
                {{replyTree.data.children[0].data.author}} | {{replyTree.data.children[0].data.ups}} upvotes
            </div>
            <div v-if="replyTree.data !== undefined && replyTree.data.children[0].kind == 'more'" v-on:click="moreComments(replyTree.data.children[0])">
                <button class="see-replies" v-on:click="interactedWith = !interactedWith" v-show="!interactedWith">More replies <b>[+]</b></button>
                <div v-if="this.replies !== undefined">
                    <div v-for="reply in this.replies">
                        <div class="tree-comments">
                            <div class="replies">
                                <div class="author-and-upvotes">
                                    {{reply.author}} | {{reply.score}} upvotes
                                </div>
                            <div v-html="reply.body"></div>
                            </div>
                        </div>
                    </div>
                </div>
        </div>
          <div v-if="replyTree.data !== undefined && replyTree.data !== undefined" v-html="replyTree.data.children[0].data.body_html"></div>
          <tree-comments
            v-for="reply in replyTree.data.children"
            :replyTree="reply.data.replies"
            :depth="depth + 1"
          >
          </tree-comments>
        </div>
      </template>
      <script>

        import axios from 'axios'

        export default {
          props: ['replyTree', 'depth'],
          name: 'tree-comments',
          methods: {
              moreComments: function(comment) {
                  // using external proxy for get, see no danger since it removes the need for a proxy and backend server
                  // and its a simple get, but yeah not ideal
                  let url = `https://api.pushshift.io/reddit/comment/search?raw_json=1&parent_id=${comment.data.parent_id}&limit=500`
                axios.get(url).then(response => {
                    console.log(response)
                    this.replies = response.data.data
                })

              }
          },
          data() {
            return {
                interactedWith: false,
                replies: []
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
        $thread-color: #2a2a44;

        .comment > .tree-comments {
            margin-left: 0px;
        }

        .tree-comments {
            border-left: 1.5px solid $thread-color;
            padding-left: 13px;
            margin-left: 10px;

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

                &:hover {
                    color: $secondary-color;
                    background: $main-color;

                }
            }

        }

</style>
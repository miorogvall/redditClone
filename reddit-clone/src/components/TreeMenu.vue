<template>
        <div class="tree-menu" v-if="replyTree !== undefined && replyTree.data !== undefined && replyTree.data.children[0].kind !== 'more'">
            <div 
                class="author-and-upvotes" 
                v-if="replyTree.data !== undefined && replyTree.data.children[0].kind !== 'more'">
                {{replyTree.data.children[0].data.author}} - {{replyTree.data.children[0].data.ups}} upvotes
            </div>
          <div v-if="replyTree.data !== undefined && replyTree.data !== undefined" v-html="replyTree.data.children[0].data.body_html"></div>
          <tree-menu
            v-for="reply in replyTree.data.children"
            :replyTree="reply.data.replies"
            :depth="depth + 1"
          >
          </tree-menu>
        </div>
      </template>
      <script>
        export default {
          props: ['replyTree', 'depth' ],
          name: 'tree-menu'
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

        .comment > .tree-menu {
            margin-left: 0px;
        }

        .tree-menu {
            border-left: 1.5px solid $thread-color;
            padding-left: 13px;
            margin-left: 10px;

            /deep/ .md {
                margin-bottom: 10px;
                color: $baby-blue;
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

        }

</style>
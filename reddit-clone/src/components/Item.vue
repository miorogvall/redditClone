<template>
    <div class="item" :data-id="id" @click="emitToParent" ref="chosenItem" v-model="childMessage">
        <div class="image-div">
            <img class="item-image" v-bind:src="this.thumbnail">
        </div>
        <div>
            <div class="item-title"><h1><span>{{this.title}}</span></h1>
            </div>
            <div class="item-upvotes"><h1><span>{{shortenUpvotesFormat}} updoots</span></h1>
            </div>
            <div class="item-info">
                <div class="item-author">posted by <span class="author">{{this.author}}</span> at <span class="author">{{convertFromUnix}}</span> with <span class="author">{{this.commentNumber}} comments</span></div>
            </div>
        </div>
    </div>
</template>
  <script>

  import axios from 'axios'
  export default {
    name: 'Item',
    props: {
      title: String,
      thumbnail: String,
      created: Number,
      commentNumber: Number,
      author:String,
      score:Number,
      permalink:String,
      id:String,
      parentData: Object,
      stringProp: String,
      title: String,
      childMessage: Object
    },
    methods: {
        emitToParent(event) {
            let item = this.$refs.chosenItem
            let postId = item.dataset.id
            console.log(postId)
            console.log('emitToParent')
            axios
            .get(`https://www.reddit.com/comments/${postId}/.json?raw_json=1`)
            .then(response => {
                this.$emit('childToParent', response)
                let overlay = document.getElementsByClassName('overlay')
                let body = document.getElementById('body')
                overlay['0'].classList.remove('is-closed')
                overlay['0'].classList.add('is-open')
                body.classList.add('modal-open')
                body.classList.remove('modal-closed')
                console.log(response)
            })
        }
    },
    computed: {
        convertFromUnix() {
            let options = {year: 'numeric', month: '2-digit', day: '2-digit', hour: 'numeric', minute: 'numeric' };
            let timestamp = new Date(this.created*1000).toLocaleDateString('SE-se', options).replace(/\//g, '-');
            return timestamp
        },
        shortenUpvotesFormat () {
            let upvotes = this.score
            let upvoteString = upvotes.toString()

            if(upvoteString.length < 4) {
                let format = upvotes
                return format
            } else if(upvoteString.length >= 4) {
                if(upvoteString.substring(3,4) === "0") {
                    let format = `${upvoteString.substring(0,2)}k`
                    return format
                } else {
                    let format = `${upvoteString.substring(0,2)}.${upvoteString.substring(3,4)}k`
                    return format
                }
            }
        }
    }
  }

</script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

    @import "../scss/_variables.scss";

    .item {
        width: 800px;
        margin: auto;
        display: flex;
        margin-bottom: 50px;
        margin-top: 40px;
        transition: all 0.1s ease;
        cursor: pointer;
        position: relative;

        &:hover {
            -webkit-transform: scale(0.93);
            -ms-transform: scale(0.93);
            transform: scale(0.93);
            opacity: 0.75;
        }

        .image-div {
            width: 250px;
            height: 170px;

            .item-image {
                width: 100%;
                height: 100%;
                object-fit: cover;
            }

        }

        .item-title {
            width: 550px;
            margin-top: 5px;
            margin-left: -25px;
            max-height: 86px;
            overflow: hidden;
            padding-left: 10px;

            h1 {
                text-align: left;

                span {
                    color: $secondary-color;
                    background-color: $main-color;
                    font-family: "Montserrat";
                    text-transform: capitalize;
                    font-style: italic;
                    font-size: 20px;
                    line-height: 2;
                    padding: 5px 0px;
                    display: inline;
                    box-shadow: 10px 0 0 $main-color, -10px 0 0 $main-color;
                }
            }
        }

        .item-upvotes {
            width: 550px;
            margin-top: -6px;
            margin-left: -25px;
            max-height: 39px;
            overflow: hidden;
            padding-left: 10px;

            h1 {
                text-align: left;

                span {
                    color: $baby-blue;
                    background-color: $faded-red;
                    font-family: "Montserrat";
                    text-transform: capitalize;
                    font-style: italic;
                    font-size: 11px;
                    line-height: 2;
                    padding: 5px 0px;
                    display: inline;
                    box-shadow: 10px 0 0 $faded-red, -10px 0 0 $faded-red;
                    font-weight: 600;
                }
            }
        }

        .item-author {
            text-align: left;
            margin-top: 10px;
            font-size: 12px;
            margin-left: 30px;
            font-weight: 600;
            color: $baby-blue;
            

            span {
                border-bottom: 2px solid $faded-red;
            }
        }

        .item-info {
            position: absolute;
            bottom: 0;
            left: 250px;
        }
    }
</style>

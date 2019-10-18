<template>
        <div>
            <div class="overlay" v-if="dataRecieved !== undefined" v-bind:class="isOpen">
                <div class="wrapper">
                    <div class="item-title"><h1><span>{{dataRecieved.data.title}}</span></h1></div>
                    <div class="item-text" v-if="dataRecieved.data.selftext !== ''" v-html="dataRecieved.data.selftext_html"></h1></div>
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
                isOpen: String
            },
            created () {
                console.log('hey')
                console.log(this.dataRecieved)
                console.log(this.isOpen)
                
            },
            methods: {
                closeModal: function(event) {
                    this.$emit('isOpen', 'is-closed')
                }
            }
}
     </script>
    
      <style scoped lang="scss">
          
            $black-color: #272727;
            $white: #fdfdfd;
            $main-color: #ffc5a1;
            $secondary-color: #37003c;

          .overlay{
              position: fixed;
              left: 0;
              right: 0;
              top: 0;
              bottom: 0;
              background: $white;
              z-index: 1;
              opacity: 0.98;
              overflow: scroll;
              
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
                    margin-top: 140px;

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
          }
      </style>

<template>
  <div id="app">
    <div id="previous" v-on:click="getData('before')" v-bind:class="{'active': this.count > 0,  'disabled': this.count == 0 }">
      <div class="flex-helper">
        <p class="pagination-text">prev</p>
      </div>
    </div>
    <div id="item-list">
      <div class="header">
        <div class="data-box">
          <label for="limit"># of entries</label>
          <div class="wrapper-icon">
            <span class="arrow-down"><i class="fas fa-chevron-down"></i></span>
            <select class="limit" @change="handleChange" name="limit">
              <option value="5">5</option>
              <option value="10">10</option>
              <option value="25">25</option>
            </select>
        </div>
      </div>
      <div class="data-box">
        <label for="subreddit">subreddit</label>
        <input class="subreddit" type="text" @keyup="handleChange" name="subreddit" placeholder="subreddit" id="subreddit-input">
      </div>
      </div>

      <ItemList :posts="posts"/>
    </div>
    <div id="next" v-on:click="getData('after')">
      <div class="flex-helper">
        <p class="pagination-text">next</p>
      </div>
    </div>
    <div id="loader"></div>
  </div>
</template>

<script>
  // @ is an alias to /src
  import ItemList from '@/components/ItemList.vue'
  import Item from '@/components/Item.vue'
  import axios from 'axios'

  export default {
    name: 'App',
    components: {
      ItemList
    },
    data() {
      return {
        posts: [],
        after: String,
        isLoading: Boolean,
        count: 0,
        before: '',
        after: '',
        limit: 5,
        subreddit: 'all',
      }
    },
    methods: {
      handleChange: function (e) {

        let input = document.querySelector('#subreddit-input')
        this.count = 0;
        //arrow function to preserve correct reference to 'this'
        if(e.target.classList.contains('limit')) {
          this.limit = e.target.options[e.target.options.selectedIndex].value
          let url = `https://www.reddit.com/r/${this.subreddit}/hot/.json?limit=${this.limit}&count=${this.count}&raw_json=1`
          axios.get(url).then(response => {
              this.posts = response.data.data.children
              this.after = response.data.data.after
              this.before = response.data.data.before
            })
        }
        if(e.target.classList.contains('subreddit')) {
          if(e.target.value != '') {
          this.subreddit = e.target.value
          } else {
            this.subreddit = 'all'
          }
          clearTimeout(this.timeout);
          this.timeout = setTimeout(() => {
              this.count = this.count = 0;
              let url = `https://www.reddit.com/r/${this.subreddit}/hot/.json?limit=${this.limit}&count=${this.count}&raw_json=1`
              axios.get(url).then(response => {
                  this.posts = response.data.data.children
                  this.after = response.data.data.after
                  this.before = response.data.data.before
                  window.scroll(0,0)
                })
          }, 560);
        }
        
      },
        getData: function(hash){
            if(hash == 'before') {
              console.log(this.count)
              console.log('CURRENT BEFORE', this.before)
              if(!this.count <= 0) {
                if (this.count <= 0) {
                this.count = 0
                console.log('zero')
              } else {
                this.count = this.count - parseInt(this.limit)
                console.log(this.count)
              }
              let url = `https://www.reddit.com/r/${this.subreddit}/hot/.json?limit=${this.limit}&count=${this.count}&before=${this.before}&raw_json=1`
              axios.get(url).then(response => {
                  this.posts = response.data.data.children
                  this.after = response.data.data.after
                  this.before = response.data.data.before
                })
              }
            } else {
               this.count = parseInt(this.count) + parseInt(this.limit);
              let url = `https://www.reddit.com/r/${this.subreddit}/hot/.json?limit=${this.limit}&count=${this.count}&after=${this.after}&raw_json=1`
              axios.get(url).then(response => {
                  this.posts = response.data.data.children
                  this.after = response.data.data.after
                  this.before = response.data.data.before
                })
            }
            window.scroll(0,0)
        }
      },
    created () {
      axios
        .get(`https://www.reddit.com/r/${this.subreddit}.json?&count=0&limit=${this.limit}&raw_json=1`)
        .then(response => {
          this.count = 0;
          this.posts = response.data.data.children
          this.after = response.data.data.after
          this.before = response.data.data.before
          console.log(this.before)
          console.log(this.after)
        }),

        axios.interceptors.request.use(config => {
          return config;
        }, function(error) {
          // Do something with request error
          return Promise.reject(error);
        });

        axios.interceptors.response.use(response => {
          return response;
        }, function(error) {
          return Promise.reject(error);
        });
      }
}
</script>

<style lang="scss">

  * {
    box-sizing: border-box;
  }

  $black-color: #272727;
  $white: #fdfdfd;
  $main-color: #ffe06d;
  $secondary-color: #20203c;
  $faded-red: #a23354;
  $baby-blue: #c8d8f5;

/* i love the overwatch font... */
  @font-face {
  font-family: overwatch;
  src: url(https://us.battle.net/forums/static/fonts/f014015d/f014015d.woff);
}

@font-face {
  font-family: overwatch-italic;
  src: url(https://us.battle.net/forums/static/fonts/bignoodletoo/bignoodletoo.woff);
}

    /* width */
  ::-webkit-scrollbar {
    width: 10px;
  }

  /* Track */
  ::-webkit-scrollbar-track {
    background: #141422;
  }
  
  /* Handle */
  ::-webkit-scrollbar-thumb {
    background: $baby-blue; 
    border-radius: 3px;
  }

  /* Handle on hover */
  ::-webkit-scrollbar-thumb:hover {
    background: $baby-blue; 
    opacity: 0.9;
  }

  #body {
    overflow: auto;

    &.modal-open {
      overflow: hidden;
    }
  }

  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    background: $baby-blue;
    padding: 15px 0px;
    position: fixed;
    width: 100%;
    z-index: 99999999;

    .wrapper-icon {
      position: relative;
    }

    .data-box {
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      width: 300px;
      text-align: left;

        input, label, select {
          display:block;
          outline: none;
        }

        label {
          color: $secondary-color;
          font-weight: bold;
          border: none;
          text-transform: uppercase;
          font-size: 15px;
          letter-spacing: 1px;
          background: transparent;
          font-family: "overwatch";
        }

        .arrow-down {
          color: $main-color;
          font-family: "fontAwesome";
          left: 38px;
          top: 15px;
          font-size: 12px;
          display: block;
          position: absolute;
          pointer-events: none;
        }
        select {
          background: $secondary-color;
          border: none;
          padding: 10px 14px;
          width: 60px;
          color: $main-color;
          font-size: 16px;
          font-weight: 600;
          outline: none;
          -webkit-appearance: none;
          cursor: pointer;
          transition: all 0.15 ease;
          margin-top: 4px;

        }
        input {
          background: transparent;
          border: none;
          font-weight: 600;
          color: #20203c;
          font-size: 19px;
          width: 100%;
          border-bottom: 2px solid #ffe06d;
          padding: 8px 11px;
          padding-left: 0;

          &::placeholder {
            color: inherit;
            font-size: 14px;
            color: #20203c52;
          }
        }
    }
  }

  a {
    color: $faded-red;
    text-decoration: none;
    border-bottom: 2px solid $baby-blue;
    transition: all 0.05s ease;
    line-height: 25px;
    padding: 3px;
    padding-bottom: 0;

    &:hover {
      color: $secondary-color;
      border-bottom: 2px solid $secondary-color;
      background: $main-color;
      padding: 3px;
    }
  }

html, body {
  min-height: 100vh;
  padding: 0;
  margin: 0;
  color: $black-color;
  font-family: "Montserrat";
  }

p, h1, h2, h3, h4, h5, h6 {
  padding:0;
  margin: 0;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  display: flex;
  flex-direction: row;
  background: $secondary-color;

  #previous {
    position: fixed;
    left: 0;

  }

  #next {
    position: fixed;
    right: 0;
  }

  #previous, #next {
    height: 100vh;
    padding: 0 20px;
    transition: background 0.15s ease;
    font-weight: 600;
    writing-mode: vertical-rl;
    text-orientation: upright;
    font-size: 20px;
    position: fixed;
    background: rgba(0, 0, 0, 0.2);
    color: $faded-red;
    transition: all 0.05s ease-in;
    will-change: opacity;
    text-transform: uppercase;
    z-index: 999999;
    font-family: "overwatch";

    &.disabled {
      background: #151515;
      color: #404040;
      cursor: not-allowed;
      transition: none;

      &:hover {
        cursor: not-allowed;
        opacity: 1;
        padding: 0 20px;
      }
    }

    &:hover {
      cursor: pointer;
      opacity: 0.75;
      padding: 0 28px;
    }
  }

  #loader.loading {
    position: fixed;
    width: 100%;
    height: 100%;
    background: red;
    opacity: 1;
  }

  #loader {
    pointer-events: none;
    opacity: 0;
  }

  #item-list {
    flex: 1 0 auto;
  }
  @keyframes gradientBG {
	0% {
		background-position: 0% 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0% 50%;
	}
}

}
</style>

<template>
  <div id="app">
    <div id="previous" v-on:click="getData('before')" v-bind:class="{'active': this.count > 0,  'disabled': this.count == 0 }">
      <div class="flex-helper">
        <p class="pagination-text">prev {{this.before}}</p>
      </div>
    </div>
    <div id="item-list">
      <div class="header">
        <select class="limit" @change="handleChange">
          <option value="5">5 entries</option>
          <option value="10 entries">10</option>
          <option value="25 entries">25</option>
      </select>
      <input class="subreddit" type="text" @change="handleChange">
      </div>

      <ItemList :posts="posts"/>
    </div>
    <div id="next" v-on:click="getData('after')">
      <div class="flex-helper">
        <p class="pagination-text">next {{this.after}}</p>
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
        firstPage: Boolean,
        isLoading: Boolean,
        count: 10,
        before: '',
        after: '',
        limit: 5,
        subreddit: 'all'
      }
    },
    methods: {
      handleChange: function (e) {
        console.log('changed')
        console.log(e.target.classList)
        if(e.target.classList.contains('limit')) {
         this.limit = e.target.options[e.target.options.selectedIndex].value
        }
        if(e.target.classList.contains('subreddit')) {
          if(e.target.value != '') {
          this.subreddit = e.target.value
          } else {
            this.subreddit = 'all'
          }
          console.log(e.target.value)
        }
        console.log(this.limit, this.subreddit)
               this.count = this.count = 0;
               console.log(this.after, 'after before post')
               console.log(this.before, 'before before post')
              let url = `https://www.reddit.com/r/${this.subreddit}/hot/.json?limit=${this.limit}&count=${this.count}&raw_json=1`
              axios.get(url).then(response => {
                  this.posts = response.data.data.children
                  this.after = response.data.data.after
                  this.before = response.data.data.before
                  console.log(url)
                  console.log('META BELOW')
                  console.log(this.count)
                })
      },
        getData: function(hash){
          console.log(hash)
            if(hash == 'before') {
              if (this.count == 10 && this.count == 0) {
                this.count = 0 
              } else {
                this.count = this.count -= 10;
              }
               console.log(this.after, 'after before post')
               console.log(this.before, 'before before post')
              console.log('CURRENT BEFORE', this.before)
              let url = `https://www.reddit.com/r/${this.subreddit}/hot/.json?limit=${this.limit}&count=${this.count}&before=${this.before}?raw_json=1`
              axios.get(url).then(response => {
                  this.posts = response.data.data.children
                  this.after = response.data.data.after
                  this.before = response.data.data.before
                   console.log(url)
                   console.log('META BELOW')
                  console.log(this.count)
                  
                })
            } else {
               this.count = this.count += 10;
               console.log(this.after, 'after before post')
               console.log(this.before, 'before before post')
              let url = `https://www.reddit.com/r/${this.subreddit}/hot/.json?limit=${this.limit}&count=${this.count}&after=${this.after}&raw_json=1`
              axios.get(url).then(response => {
                  this.posts = response.data.data.children
                  this.after = response.data.data.after
                  this.before = response.data.data.before
                  console.log(url)
                  console.log('META BELOW')
                  console.log(this.count)
                })
            }
            console.log('after', this.after)
            console.log('before', this.before)
        },
        transitionContent: function(elem) {
        }

      },
    created () {
      axios
        .get(`https://www.reddit.com/r/${this.subreddit}.json?limit=${this.limit}&raw_json=1`)
        .then(response => {
          this.count = 10;
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
  $main-color: #ffc5a1;
  $secondary-color: #37003c;
  $light-yellow: #feffa1;
  $dark-blue: #09003c;

    /* width */
  ::-webkit-scrollbar {
    width: 12px;
  }

  /* Track */
  ::-webkit-scrollbar-track {
    background: $secondary-color;
  }
  
  /* Handle */
  ::-webkit-scrollbar-thumb {
    background: $main-color; 
    border-radius: 5px;
  }

  /* Handle on hover */
  ::-webkit-scrollbar-thumb:hover {
    background: $main-color; 
    opacity: 0.9;
  }

  #body {
    overflow: auto;

    &.modal-open {
      overflow: hidden;
    }
  }

  a {
    color: $secondary-color;
    text-decoration: none;
    border-bottom: 2px solid $main-color;
    transition: all 0.05s ease;
    line-height: 25px;

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
  background: $white;

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
    font-size: 22px;
    font-style: italic;
    position: fixed;
    background: $light-yellow;
    color: $black-color;
    transition: all 0.05s ease-in;
    will-change: opacity;
    text-transform: uppercase;

    &.disabled {
      background: #e0e0e0;
      color: #bdbdbd;
      cursor: not-allowed;
      transition: none;

      &:hover {
        cursor: not-allowed;
        opacity: 1;
        padding: 0 12px;
      }
    }

    &:hover {
      cursor: pointer;
      opacity: 0.75;
      padding: 0 28px;
    }



    .pagination-icon {
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

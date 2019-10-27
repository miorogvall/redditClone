<template>
  <div id="app">
    <div id="previous" v-on:click="getData('before')" v-bind:class="{'active': this.count > 0,  'disabled': this.count == 0 }">
      <div class="flex-helper">
        <p class="pagination-text">prev</p>
      </div>
    </div>
    <div id="item-list">
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
        firstPage: Boolean,
        isLoading: Boolean,
        count: 0
      }
    },
    methods: {
        getData: function(hash){
            if(hash == 'before') {
              if (this.count == 10 && this.count == 0) {
                this.count = 0 
              } else {
                this.count = this.count -= 10;
              }
              let url = `https://www.reddit.com/r/GlobalOffensive/top/.json?limit=10&count=${this.count}&before=${this.before}?raw_json=1`
              axios
                .get(url)
                .then(response => {
                  this.posts = response.data.data.children
                  this.after = response.data.data.after
                  if(this.firstPage !== 0) {
                    this.firstPage--;
                  }
                  console.log(this.before)
                  console.log(this.after)
                  console.log(url)
                  
                })
            } else {
               this.count = this.count += 10;
              let url = `https://www.reddit.com/r/GlobalOffensive/top/.json?limit=10&count=${this.count}&after=${this.after}&raw_json=1`
              axios
                .get(url)
                .then(response => {
                  this.posts = response.data.data.children
                  this.after = response.data.data.after
                  this.firstPage++;
                  console.log(this.after)
                  console.log(url)
                  console.log(this.before)
                })
            }
        },
        transitionContent: function(elem) {
        }

      },
    created () {
      axios
        .get(`https://www.reddit.com/r/GlobalOffensive.json?limit=10&raw_json=1`)
        .then(response => {
          this.count = 0;
          this.posts = response.data.data.children
          this.after = response.data.data.after
          this.firstPage = 0;
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

<template>
  <main class="typeahead">
    <header>
      <div class="container header__text">
        <h1>Typeahead</h1>
        <h4>A Vue.js typeahead component for fetching data from Twitter</h4>
      </div>
    </header>
    <section class="container typeahead__section">
      <i class="fa fa-spinner fa-spin" v-if="loading"></i>
      <template v-else>
        <i class="fa fa-search" v-show="isEmpty"></i>
        <i class="fa fa-times" v-show="isDirty" @click="reset"></i>
      </template>

      <input type="text"
        class="typeahead__input"
        placeholder="Start typing to find a person"
        autocomplete="off"
        v-model="query"
        @keydown.down="down"
        @keydown.up="up"
        @keydown.enter="hit"
        @keydown.esc="reset"
        @blur="reset"
        @input="update"/>

      <ul class="typeahead__results" v-show="hasItems">
        <li v-for="(item, $item) in items" :class="'typeahead__block' + activeClass($item)" @mousedown="hit" @mousemove="setActive($item)">
          <div class="typeahead__content">
            <div class="typeahead__image">
              <img :src="item.profile_image_url_https" alt="" />
            </div>
            <div class="typeahead__content--name" v-text="item.name"></div>
            <div class="typeahead__content--screen-name" v-text="item.screen_name"></div>
            <div class="typeahead__content--description" v-text="item.description"></div>
          </div>
        </li>
      </ul>
      <div class="typeahead__no-results" v-show="isDirty && !items.length">
        <span>No results</span>
      </div>
    </section>
  </main>
</template>

<script>
import VueTypeahead from 'vue-typeahead'

export default {
  extends: VueTypeahead,
  data () {
    return {
      src: 'https://typeahead-js-twitter-api-proxy.herokuapp.com/demo/search',
      limit: 9
    }
  },
  methods: {
    onHit (item) {
      window.location.href = 'http://twitter.com/' + item.screen_name
    }
  }
}
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css?family=Karla');
@import '../assets/styles/styles.scss';

.typeahead {
  font-family: 'Karla', sans-serif;
  position: relative;

  header {
    background-image: linear-gradient(141deg,#009e6c 0,#00d1b2 71%,#00e7eb 100%);
    color: #fff;

    .header__text {
      padding: 5em 0;

      h1 {
        font-size: 3em;
      }
    }
  }

  &__section {
    padding: 4em 0;

    i {
      float: right;
      position: relative;
      top: 1.5em;
      right: 1.8em;
      opacity: 0.4;
    }

    .fa-times {
      cursor: pointer;
    }
  }

  &__input {
    border-radius: 1.4em;
    border: none;
    box-shadow: inset 0 0.06em 0.25em rgba(0,0,0,.4);
    font-size: 0.85em;
    padding: 0.75em 1.6em;
    transition: border-color ease-in-out .15s,box-shadow ease-in-out .15s;
    width: 100%;

    &:hover {
      border-color: rgba(0,0,0,.075);
      box-shadow: inset 0 0.06em 0.06em rgba(0,0,0,.075),0 0 8px grey;
      outline: 0;
    }

    &:focus {
      border-color: #4fc08d;
      box-shadow: inset 0 0.06em 0.06em rgba(0,0,0,.075),0 0 0.5em #4fc08d;
      outline: 0;
    }
  }

  &__results {
    display: flex;
    flex-wrap: wrap;
    padding: 0;
    min-width: 100%;
    list-style: none;
    z-index: 1000;

    li {
      cursor: pointer;
      display: flex;
      padding: 2em;
      position: relative;
      width: 100%;

      &:after {
        box-shadow: .25em .25em 1em rgba(0,0,0,0.15);
        background: #fff;
        content: "";
        height: 100%;
        left: -1em;
        opacity: 0;
        position: absolute;
        top: 0;
        transition: opacity 0.25s ease-out;
        width: 100%;
        z-index: -1;
      }

      &:hover:after {
        opacity: 1;
      }
    }

    @media all and (min-width: 40em) {
      li {
        width: 50%;
      }
    }
    @media all and (min-width: 60em) {
      li {
        width: 33.33%;
      }
    }
  }

  &__content {
    display: flex;
    flex-direction: column;
    padding: 1em;
    width: 100%;

    &--name {
      font-weight: 700;
      font-size: 1.125em;
      padding: .5em 0;
      word-wrap: break-word;
    }

    &--screen-name {
      padding: .5em 0;
      word-wrap: break-word;
    }

    &--description {
      font-style: italic;
      padding: .5em 0;
      word-wrap: break-word;
    }
  }

  &__image {
    align-items: center;
    border-radius: 50%;
    display: flex;
    height: 6em;
    justify-content: center;
    margin-bottom: 2em;
    overflow: hidden;
    width: 6em;

    img {
      width: 6em;
    }
  }

  &__no-results {
    text-align: center;
    padding: 6em;
  }
}
</style>

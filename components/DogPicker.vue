<template>
  <div class="component-DogPicker">
    <div class="left">
      <h2 v-if="showPlaceholder">
        Kuvia koirista! Valitse ketkä pääsevät jarkoon!
      </h2>
      <h2 v-if="!showPlaceholder">Pääseekö koira jatkoon?</h2>
      <div class="container">
        <div v-if="!showPlaceholder" class="image-wrapper">
          <img :src="imageUrl" alt="dog" />
        </div>
        <div v-if="!showPlaceholder" class="buttons-wrapper">
          <button class="green" @click="accpetDog">Kyllä</button>
          <button class="red" @click="declineDog">Ei</button>
        </div>
        <div v-if="showPlaceholder" class="placeholder-button">
          <button class="begin" @click="fetchImage">Aloita!</button>
        </div>
      </div>
    </div>
    <div class="right">
      <ul>
        <li v-for="dog in dogs" :key="dog">
          <a :href="dog" target="_blank">
            <img :src="dog" alt="dog" />
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import axios from 'axios'

const URL = 'https://dog.ceo/api/breeds/image/random'
export default {
  data: () => ({
    imageUrl: '',
    dogs: [],
  }),
  computed: {
    showPlaceholder() {
      return !this.imageUrl.length
    },
  },
  methods: {
    async fetchImage() {
      const r = await axios.get('https://dog.ceo/api/breeds/image/random')

      if (r.data) {
        this.imageUrl = r.data.message
      }
    },
    accpetDog() {
      this.dogs = [...this.dogs, this.imageUrl]
      this.fetchImage()
    },
    declineDog() {
      this.fetchImage()
    },
  },
}
</script>

<style lang="less" scoped>
.component-DogPicker {
  margin-top: 50px;
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin: auto;
  .left {
    h2 {
      text-align: center;
    }
    .container {
      width: 600px;
      height: 600px;
      margin: auto;
      .image-wrapper {
        text-align: center;
        img {
          border-radius: 5px;
          width: 600px;
          height: 600px;
          object-fit: cover;
          box-shadow: 0px 2px 10px 1px;
        }
      }
      .buttons-wrapper {
        margin-top: 20px;
        display: flex;
        flex-direction: row;
        justify-content: space-around;

        button {
          font-size: 20px;
          padding: 5px 20px;
          border: none;
          border-radius: 5px;
          box-shadow: 0px 2px 10px 1px;

          &.green {
            background-color: #69b870;
          }
          &.red {
            background-color: #c53f26;
          }
        }
      }
      .placeholder-button {
        display: flex;
        justify-content: center;
        .begin {
          font-size: 20px;
          padding: 5px 20px;
          border: none;
          border-radius: 5px;
          box-shadow: 0px 2px 10px 1px;
        }
      }
    }
  }
  .right {
    margin-top: 40px;
    width: 150px;
    height: 80vh;
    padding-right: 20px;
    overflow-y: auto;

    ul {
      list-style: none;

      li {
        margin: 0;
        text-align: center;
        img {
          width: 100px;
          height: 100px;
          object-fit: cover;
        }
      }
    }
  }
}
</style>

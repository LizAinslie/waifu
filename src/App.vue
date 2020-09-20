<template>
  <div style="width: 100%; display: flex; justify-content: center;">
    <div>
      <h1>Waifus!</h1>
      <action-bar style="display: block; width: 512px">
        <r-button @click="fetchWaifu" color="green">Fetch A Waifu</r-button>
        <r-button @click="aboutModalVisible = true" color="purple">
          About
        </r-button>
      </action-bar>

      <img
        style="display: block; width: 512px; height: 512px; background-color: #999"
        :src="image"
        alt="Waifu"
      />
    </div>
    <modal v-if="aboutModalVisible" @close="aboutModalVisible = false">
      <h3 slot="header">About</h3>
      <div slot="body">
        <p>
          This site uses artificial intelligence to generate anime characters!
          Here's how it's done:
        </p>
        <ol>
          <li>
            Images are continuously generated and uploaded to a S3 bucket by a
            Python program utilizing Tensorflow.
          </li>
          <li>
            A Node API then serves random images from the S3 bucket on request.
          </li>
          <li>
            This site makes requests to the Node API to fetch images and display
            them.
          </li>
        </ol>
      </div>
    </modal>
  </div>
</template>

<script>
import ActionBar from "@/components/ActionBar";
import Modal from "@/components/Modal";
import RButton from "@/components/Button";

export default {
  name: "App",
  components: {
    RButton,
    Modal,
    ActionBar
  },
  data() {
    return {
      image: "https://via.placeholder.com/512?text=Waifu",
      aboutModalVisible: false
    };
  },
  methods: {
    fetchWaifu() {
      fetch("https://api.waifu.raildev.tech/single")
        .then(res => res.json())
        .then(json => {
          this.image = json.url;
        });
    }
  }
};
</script>

<style lang="scss">
* {
  font-family: sans-serif;
}
</style>

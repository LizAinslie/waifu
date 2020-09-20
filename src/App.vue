<template>
  <div>
    <Button :on-click="fetchWaifu" color="green">Fetch A Waifu</Button>
    <img :src="image" alt="Waifu" />
  </div>
</template>

<script>
import Button from "@/components/Button";

export default {
  name: "App",
  components: {
    Button
  },
  data() {
    return {
      image: null
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

<style lang="scss"></style>

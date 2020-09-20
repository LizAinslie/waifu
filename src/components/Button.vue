<template>
  <button
    ref="btn"
    :class="{ btn: true, [color]: true }"
    @click="onButtonClick"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "RButton",
  props: {
    color: String
  },
  methods: {
    onButtonClick() {
      const scrollTop = document.body.scrollTop;
      const position = this.getPosition();
      const x = event.pageX - position.left;
      const y = event.pageY - position.top - scrollTop;

      const el = this.$refs["btn"];
      el.setAttribute("style", `--coord-x: ${x}px; --coord-y: ${y}px;`);
      el.classList.add("animate");
      setTimeout(() => {
        el.classList.remove("animate");
      }, 400);

      this.$emit("click");
    },
    getPosition() {
      let el = this.$refs["btn"];

      let x = 0,
        y = 0;
      while (el && !isNaN(el.offsetLeft) && !isNaN(el.offsetTop)) {
        x += el.offsetLeft - el.scrollLeft;
        y += el.offsetTop - el.scrollTop;
        el = el.offsetParent;
      }
      return { top: y, left: x };
    }
  }
};
</script>

<style scoped lang="scss">
$colors: (
  "red": (
    "bg": #e74c3c,
    "bottom": #cb4335,
    "hover": #ec7063,
    "clicked": #b03a2e
  ),
  "green": (
    "bg": #2ecc71,
    "bottom": #28b463,
    "hover": #58d68d,
    "clicked": #239b56
  ),
  "blue": (
    "bg": #3498db,
    "bottom": #2e86c1,
    "hover": #5dade2,
    "clicked": #2874a6
  ),
  "orange": (
    "bg": #e67e22,
    "bottom": #ca6f1e,
    "hover": #eb984e,
    "clicked": #af601a
  ),
  "purple": (
    "bg": #8e44ad,
    "bottom": #7d3c98,
    "hover": #a569bd,
    "clicked": #6c3483
  )
);

@keyframes btn-click {
  from {
    width: 30px;
    height: 30px;
    opacity: 1;
  }
  to {
    width: 150px;
    height: 150px;
    opacity: 0;
  }
}

.btn {
  position: relative;
  top: 0;
  border: none;
  padding: 12px 16px;
  background-color: #d0d0d0;
  color: #111;
  font-weight: bold;
  border-radius: 4px;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.16);
  border-bottom: 1px solid #aaa;
  outline: none;
  transition: background-color 0.2s, box-shadow 0.2s;
  overflow: hidden;
  cursor: pointer;
  display: inline-block;
  margin: 0.3rem;

  &.animate::after {
    position: absolute;
    content: " ";
    top: var(--coord-y);
    left: var(--coord-x);
    transform: translateX(-50%) translateY(-50%);
    background-color: rgba(0, 0, 0, 0.1);
    display: block;
    border-radius: 50%;
    animation: btn-click ease-out 0.5s normal;
  }

  &[disabled] {
    color: #999;
    background-color: #d0d0d0;
    border-bottom-color: #aaa;

    &:hover {
      top: 0;
      background-color: #d0d0d0;
      border-bottom-width: 1px;
      box-shadow: 0 2px 2px rgba(0, 0, 0, 0.16);
    }
  }

  &:hover {
    top: -1px;
    background-color: #dadada;
    border-bottom-width: 2px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.16);
  }

  &:active {
    top: 1px;
    background-color: #cdcdcd;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.16);
    border-bottom-width: 0;
  }

  @each $colorName, $colorVals in $colors {
    &.#{$colorName} {
      background-color: map-get($colorVals, "bg");
      color: #fff;
      border-bottom-color: map-get($colorVals, "bottom");

      &:hover {
        background-color: map-get($colorVals, "hover");
      }

      &:active {
        background-color: map-get($colorVals, "clicked");
      }
    }
  }
}
</style>

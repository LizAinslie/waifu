<template>
  <div class="action-bar">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "ActionBar"
};
</script>

<style scoped lang="scss">
.action-bar {
  margin: 0.3rem;
  padding: 0;
  border-radius: 4px;
  display: inline-block;

  & .btn {
    margin-top: 0;
    margin-bottom: 0;

    &:first-child {
      margin-left: 0;
    }

    &:last-child {
      margin-right: 0;
    }
  }
}
</style>

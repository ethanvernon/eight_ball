<template>
  <div>
    <p>{{ result }}</p>
  </div>
</template>

<script>
export default {
  name: "QueryResult",
  props: ["result"]
};
</script>

<style lang="scss" scoped>
div {
  height: 3rem;
}
</style>

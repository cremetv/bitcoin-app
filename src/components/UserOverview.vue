<template>
  <div>
    user overview for {{ username }} ...
  </div>
</template>

<script>
export default {
  name: 'UserOverview',
  props: {
    username: String
  }
}
</script>

<style scoped lang="less">

</style>

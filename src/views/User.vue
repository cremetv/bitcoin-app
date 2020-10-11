<template>
  <div class="user">
    <h1>This is the user page for {{ username }}</h1>
  </div>
</template>

<script>
export default {
  props: ['username']
}
</script>

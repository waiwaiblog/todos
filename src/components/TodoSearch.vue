<template>
  <span>
    <input
      class="new-todo" autocomplete="off"
      placeholder="Search" :value="value" @input="$emit('input', $event.target.value.trim())">
  </span>
</template>

<script>
export default {
  props: {
    value: {
      type: String
    }
  }
}

</script>
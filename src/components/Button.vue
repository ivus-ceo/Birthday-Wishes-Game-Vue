<template>
  <button class="button" @click="handleClick">
    <slot />
  </button>
</template>

<script setup>
  // import party from 'party-js'

  const handleClick = (event) => {
    // party.confetti(event.target)
  }
</script>

<style lang="scss" scoped>
  button.button {
    color: #fff;
    border: none;
    outline: none;
    cursor: pointer;
    position: relative;
    padding: 0.6rem 2rem;
    border-radius: 0.4rem;
    background-color: black;
    text-transform: uppercase;

    & > * {
      pointer-events: none;
    }
  }
</style>

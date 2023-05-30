<template>
  <main class="layout">
    <div class="layout__start" v-if="stage === 0">
      <p class="layout__welcome">
        Игра позволяет случайным образом связаться с Вселенной и выбрать для
        себя желания на день 🥳! После начала игры будет установлена связь между
        вами и Вселенной, затем положительная энергетика будет наполнять вашу
        жизнь и тех кто вам дорог 🎁!
      </p>

      <Button @click="handleClick">Начать игру</Button>
    </div>

    <div class="layout__game" v-if="stage === 1">
      <Wishes @end="handleEnd" />
    </div>

    <div class="layout__ending" v-if="stage === 2">
      <p>
        Вы случайным образом выбрали следующие желания, Вселенная дарит их вам
        на безвозмездном основании!
      </p>

      <div
        class="layout__wish"
        v-for="(wish, index) in chosenWishes"
        :key="index"
      >
        {{ wish }}
      </div>
    </div>
  </main>
</template>

<script setup>
  import { ref, watch } from 'vue'
  import Button from '@/components/Button.vue'
  import Wishes from '@/components/Wishes.vue'

  const stage = ref(0)
  const chosenWishes = ref(null)

  const handleClick = (event) => {
    stage.value++
  }

  const handleEnd = (value) => {
    chosenWishes.value = [...new Set(value)]
    stage.value++
  }
</script>

<style lang="scss">
  main.layout {
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;

    .layout__start {
      max-width: 20rem;
      text-align: justify;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }

    .layout__game {
    }

    .layout__ending {
      width: 100vw;
      height: 100vh;
      padding: 1rem;
    }

    .layout__wish {
    }
  }
</style>

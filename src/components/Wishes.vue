<template>
  <div class="wishes">
    <div id="wishes__timer"></div>
  </div>
</template>

<script setup>
  import { onMounted, ref, defineEmits } from 'vue'
  import party from 'party-js'
  import wishes from '@/assets/wishes.json'
  import emojis from '@/assets/emojis.json'

  const emit = defineEmits(['end'])

  const keys = Object.keys(wishes)
  const values = Object.values(wishes).flat(1)
  const choosen = ref([])
  let inter = null

  const timer = () => {
    let time = 16000
    const timer = document.querySelector('#wishes__timer')

    const interval = setInterval(() => {
      time -= 1000
      timer.innerHTML = time / 1000
      if (time <= 0) {
        clearInterval(interval)
        clearInterval(inter)

        setTimeout(() => {
          alert(
            'Поздравляю! Вселенная обрабатывает желания и скоро их выдаст тебе!'
          )

          emit('end', choosen.value)
        }, 1000)
      }
    }, 1000)
  }

  const startInterval = () => {
    appendWish(values[getRandomNumber(values.length)])
    timer()

    inter = setInterval(() => {
      appendWish(values[getRandomNumber(values.length)])
    }, getRandomNumberBetween(200, 700))
  }

  const getRandomNumber = (max) => {
    return Math.floor(Math.random() * max)
  }

  const getRandomNumberBetween = (min, max) => {
    return Math.floor(Math.random() * (max - min + 1) + min)
  }

  const appendWish = (wish) => {
    const span = document.createElement('span')
    span.setAttribute('data-wish', wish)
    span.innerHTML = emojis.party[getRandomNumber(emojis.party.length)]
    span.style.left = getRandomNumber(window.innerWidth) + 'px'
    span.classList.add('wish')
    span.addEventListener('click', handleClick)
    document.querySelector('.wishes').append(span)
    span.ontransitionend = handleTransitionEnd

    setTimeout(() => {
      const height = window.innerHeight
      span.style.bottom =
        getRandomNumberBetween(height / 3, height / 1.5) + 'px'
    }, 100)
  }

  const handleClick = (event) => {
    const wish = event.target

    choosen.value.push(wish.dataset.wish)

    party.confetti(wish, {
      count: party.variation.range(10, 15),
      gravity: 100,
    })

    setTimeout(() => {
      wish.remove()
    }, 100)
  }

  const handleTransitionEnd = (event) => {
    event.target.style.bottom = '-10rem'
    event.target.ontransitionend = () => {
      event.target.remove()
    }
  }

  onMounted(() => {
    startInterval()
  })
</script>

<style lang="scss">
  div.wishes {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    user-select: none;
    position: relative;

    #wishes__timer {
      top: 3rem;
      left: 50%;
      font-size: 4rem;
      position: absolute;
      transform: translateX(-50%);
    }

    span.wish {
      bottom: -10rem;
      font-size: 5rem;
      cursor: pointer;
      position: absolute;
      transition: 2s bottom;
      animation: 0.8s rotate infinite linear;
    }
  }

  @keyframes rotate {
    0% {
      rotate: 0;
    }

    100% {
      rotate: 360deg;
    }
  }
</style>

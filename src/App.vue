<template>
  <section class="flex items-center justify-center h-screen w-screen">
    <div class="container">
      <div class="cards-container">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-4">
          <div v-for="(card, index) in cards" :key="index">
            <div class="single-card-wrapper">
              <a href="#">
                <div
                  class="card-hover-container relative flex items-center justify-center rounded-lg border border-gray-700 overflow-hidden cursor-pointer min-h-[399px] aspect-square"
                  @mousemove="onHover($event, index)"
                  @touchmove="onHover($event, index)"
                >
                  <img :src="card.imgSrc" alt="" class="z-10" />
                  <div
                    class="absolute inset-0 bg-gradient-radial from-gray-900 via-indigo-500 to-pink-500 z-0"
                  ></div>
                  <div
                    class="card-bg-characters absolute inset-0 text-sm font-mono text-white opacity-0 transition-opacity duration-500"
                    :style="getCharacterStyle(index)"
                  >
                    {{ randomStrings[index] }}
                  </div>
                </div>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import './assets/main.css'

import { defineComponent, reactive } from 'vue'

interface Card {
  imgSrc: string
}

interface Position {
  x: string
  y: string
}

export default defineComponent({
  setup() {
    const cards: Card[] = reactive([
      {
        imgSrc:
          'https://www.yudiz.com/codepen/code-hover//github-mark-white.svg',
      },
      { imgSrc: 'https://www.yudiz.com/codepen/code-hover//code.svg' },
      { imgSrc: 'https://www.yudiz.com/codepen/code-hover//die.svg' },
    ])

    const randomStrings = reactive<string[]>(['', '', ''])
    const positions = reactive<Position[]>([
      { x: '0px', y: '0px' },
      { x: '0px', y: '0px' },
      { x: '0px', y: '0px' },
    ])

    const randomString = (length: number): string => {
      const chars =
        'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'
      return Array.from(
        { length },
        () => chars[Math.floor(Math.random() * chars.length)],
      ).join('')
    }

    const onHover = (event: MouseEvent | TouchEvent, index: number): void => {
      const target = event.currentTarget as HTMLElement
      const rect = target.getBoundingClientRect()
      const x =
        (event instanceof MouseEvent
          ? event.clientX
          : event.touches[0].clientX) - rect.left
      const y =
        (event instanceof MouseEvent
          ? event.clientY
          : event.touches[0].clientY) - rect.top

      positions[index] = { x: `${x}px`, y: `${y}px` }
      randomStrings[index] = randomString(2000)
    }

    const getCharacterStyle = (index: number): Record<string, string> => {
      return {
        '--x': positions[index].x,
        '--y': positions[index].y,
      }
    }

    return {
      cards,
      randomStrings,
      positions,
      randomString,
      onHover,
      getCharacterStyle,
    }
  },
})
</script>

<style></style>

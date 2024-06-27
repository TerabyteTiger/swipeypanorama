<!-- eslint-disable no-unused-vars -->
<script setup>
import { Cropper } from 'vue-advanced-cropper'
import 'vue-advanced-cropper/dist/style.css'
import { computed, ref } from 'vue'
import wait from 'waait'

// let cropperImg = ref('https://picsum.photos/500')
let uploadImg = ref('')
let pieces = ref(3)
let img1 = ref(null)
let img2 = ref(null)
let img3 = ref(null)
let img4 = ref(null)
let img5 = ref(null)
let img6 = ref(null)
let img7 = ref(null)
let img8 = ref(null)

// access to cropper elements
const cropper = ref(null)
const cropper2 = ref(null)
const cropperComputed = computed(() => cropper.value.cropper)
const cropper2Computed = computed(() => cropper2.value.cropper2)

// screen size
const screenWidth = ref(100)

function decrement() {
  pieces.value = pieces.value - 1
}
function increment() {
  pieces.value = pieces.value + 1
}

// function change({ coordinates, canvas }) {
//   console.log(coordinates, canvas)
// }

function handleUpload(event) {
  let file = event.target.files[0] || event.dataTransfer.files[0]
  uploadImg.value = URL.createObjectURL(file)
}

async function generateImages() {
  img1.value = null
  img2.value = null
  img3.value = null
  img4.value = null
  img5.value = null
  img6.value = null
  img7.value = null
  img8.value = null

  const { coordinates } = cropper.value.getResult()
  cropper2.value.setCoordinates(() => ({
    width: coordinates.width / pieces.value,
    height: coordinates.width / pieces.value,
    left: coordinates.left,
    top: coordinates.top
  }))
  await wait(500)
  cropper2.value.getResult().canvas.toBlob((blob) => {
    img1.value = URL.createObjectURL(blob)
  })

  cropper2.value.setCoordinates(() => ({
    width: coordinates.width / pieces.value,
    height: coordinates.width / pieces.value,
    left: coordinates.left + (1 * coordinates.width) / pieces.value,
    top: coordinates.top
  }))
  await wait(500)
  cropper2.value.getResult().canvas.toBlob((blob) => {
    img2.value = URL.createObjectURL(blob)
  })

  if (pieces.value >= 3) {
    cropper2.value.setCoordinates(() => ({
      width: coordinates.width / pieces.value,
      height: coordinates.width / pieces.value,
      left: coordinates.left + (2 * coordinates.width) / pieces.value,
      top: coordinates.top
    }))
    await wait(500)
    cropper2.value.getResult().canvas.toBlob((blob) => {
      img3.value = URL.createObjectURL(blob)
    })

    if (pieces.value >= 4) {
      cropper2.value.setCoordinates(() => ({
        width: coordinates.width / pieces.value,
        height: coordinates.width / pieces.value,
        left: coordinates.left + (3 * coordinates.width) / pieces.value,
        top: coordinates.top
      }))
      await wait(500)
      cropper2.value.getResult().canvas.toBlob((blob) => {
        img4.value = URL.createObjectURL(blob)
      })

      if (pieces.value >= 5) {
        cropper2.value.setCoordinates(() => ({
          width: coordinates.width / pieces.value,
          height: coordinates.width / pieces.value,
          left: coordinates.left + (4 * coordinates.width) / pieces.value,
          top: coordinates.top
        }))
        await wait(500)
        cropper2.value.getResult().canvas.toBlob((blob) => {
          img5.value = URL.createObjectURL(blob)
        })
        if (pieces.value >= 6) {
          cropper2.value.setCoordinates(() => ({
            width: coordinates.width / pieces.value,
            height: coordinates.width / pieces.value,
            left: coordinates.left + (5 * coordinates.width) / pieces.value,
            top: coordinates.top
          }))
          await wait(500)
          cropper2.value.getResult().canvas.toBlob((blob) => {
            img6.value = URL.createObjectURL(blob)
          })

          if (pieces.value >= 7) {
            cropper2.value.setCoordinates(() => ({
              width: coordinates.width / pieces.value,
              height: coordinates.width / pieces.value,
              left: coordinates.left + (6 * coordinates.width) / pieces.value,
              top: coordinates.top
            }))
            await wait(500)
            cropper2.value.getResult().canvas.toBlob((blob) => {
              img7.value = URL.createObjectURL(blob)
            })
            if (pieces.value >= 8) {
              cropper2.value.setCoordinates(() => ({
                width: coordinates.width / pieces.value,
                height: coordinates.width / pieces.value,
                left: coordinates.left + (7 * coordinates.width) / pieces.value,
                top: coordinates.top
              }))
              await wait(500)
              cropper2.value.getResult().canvas.toBlob((blob) => {
                img8.value = URL.createObjectURL(blob)
              })
            }
          }
        }
      }
    }
  }
}

function showAboutPopup() {
  document.getElementById('aboutDialog').showModal()
}

function hideAboutPopup() {
  document.getElementById('aboutDialog').close()
}
</script>

<template>
  <header class="flex justify-between">
    <h1>Swipey Panoramas</h1>
    <button @click="showAboutPopup()">Help?</button>
  </header>
  <dialog
    id="aboutDialog"
    class="w-11/12 h-11/12 sm:w-4/5 sm:h-4/5 rounded-xl p-4 backdrop:backdrop-blur-md max-w-prose"
  >
    <div class="flex flex-col justify-between items-center h-full">
      <div class="">
        <h2>Hello There 👋🏻</h2>
        <ol>
          <li>
            To begin, upload an image - this image won't be stored anywhere, everything happens on
            your device.
          </li>
          <li>
            Next, use the - and + buttons to select how many images you'd like chained together in
            your final output.
          </li>
          <li>
            Use the cropper window to zoom, pan, and move the selection stencil over the part of the
            image you'd like to create the images from
          </li>
          <li>Click "Generate" - in the background, the cropper will begin slicing the image.</li>
          <li>Finally, view your previews and tap each of the final images to download them.</li>
        </ol>
        <h2>Why did you make this?</h2>
        <p>
          After trying to find a quality free app to make a panoramic carousel and watching roughly
          43 minutes worth of mobile ads to finally get my images, I decided I could totally build
          something in the amount of time I was watching mobile ads. <br /><br />
          I was wrong - but this was a much more fun use of my time and now I'm passing a better
          experience on to you.
          <br /><br />
          If you like using this tool I'd love if you told your friends and
          <a href="https://www.instagram.com/t.vanblargan/"
            >tagged me in your posts you make with it</a
          >
          or you can check out <a href="https://terabytetiger.com">my blog</a>! <br /><br />
          <!-- While not necessary, if you'd like to provide financial support for Swipey Panoramas you
          can <a href="buymeacoffee.com/TerabyteTiger">buy me a coffee</a> -->
        </p>
      </div>

      <button class="sticky bottom-1 w-1/3" @click="hideAboutPopup()" autofocus>Close</button>
    </div>
  </dialog>
  <main class="pb-16">
    <div class="w-min mx-auto">
      <input type="file" name="fileUpload" id="fileUpload" @change="handleUpload($event)" />
    </div>

    <div class="flex p-8 justify-center space-around">
      <input type="button" value="-" @click="decrement()" :disabled="pieces <= 2" />
      <p class="p-4">{{ pieces }} images</p>
      <input type="button" value="+" @click="increment()" :disabled="pieces >= 8" />
    </div>

    <div class="flex w-screen justify-center">
      <Cropper
        :src="uploadImg"
        @change="change"
        :stencilProps="{ aspectRatio: pieces }"
        ref="cropper"
        class="w-1/2"
      ></Cropper>
      <Cropper
        :src="uploadImg"
        @change="change"
        :stencilProps="{ aspectRatio: 1 }"
        ref="cropper2"
        tabindex="-1"
        class="opacity-0 !absolute left-0 inline w-1/2"
        style="z-index: -1"
      ></Cropper>
    </div>
    <div class="text-center my-4">
      <button v-if="uploadImg != ''" @click="generateImages()">Generate</button>
    </div>
    <div v-if="img1 ? true : false">
      <h2>Combined Preview</h2>
      <div class="flex w-11/12 md:w-2/3 mx-auto justify-center">
        <div v-if="img1">
          <img :src="img1" alt="" />
        </div>
        <div v-if="img2">
          <img :src="img2" alt="" />
        </div>
        <div v-if="img3">
          <img :src="img3" alt="" />
        </div>
        <div v-if="img4">
          <img :src="img4" alt="" />
        </div>
        <div v-if="img5">
          <img :src="img5" alt="" />
        </div>
        <div v-if="img6">
          <img :src="img6" alt="" />
        </div>
        <div v-if="img7">
          <img :src="img7" alt="" />
        </div>
        <div v-if="img8">
          <img :src="img8" alt="" />
        </div>
      </div>
      <hr class="w-1/3 my-4 mx-auto" />
      <h2>Images - Click/Tap each to download</h2>
      <div class="flex w-11/12 md:w-2/3 mx-auto justify-center bg-cornflower_blue-100">
        <a :href="img1" download="PanoPix1.png"><img :src="img1" alt="" class="p-1" /></a>
        <a :href="img2" download="PanoPix2.png"><img :src="img2" alt="" class="p-1" /></a>
        <a v-if="pieces >= 3" :href="img3" download="PanoPix3.png"
          ><img :src="img3" alt="" class="p-1"
        /></a>
        <a v-if="pieces >= 4" :href="img4" download="PanoPix4.png"
          ><img :src="img4" alt="" class="p-1"
        /></a>
        <a v-if="pieces >= 5" :href="img5" download="PanoPix5.png"
          ><img :src="img5" alt="" class="p-1"
        /></a>
        <a v-if="pieces >= 6" :href="img6" download="PanoPix6.png"
          ><img :src="img6" alt="" class="p-1"
        /></a>
        <a v-if="pieces >= 7" :href="img7" download="PanoPix7.png"
          ><img :src="img7" alt="" class="p-1"
        /></a>
        <a v-if="pieces >= 8" :href="img8" download="PanoPix8.png"
          ><img :src="img8" alt="" class="p-1"
        /></a>
      </div>
    </div>
  </main>
</template>

<style>
header {
  @apply bg-cornflower_blue-700;
  @apply text-white;
  @apply font-bold;
  @apply p-4;
  @apply mb-4;
}
h1 {
  @apply text-4xl;
}

body {
  @apply bg-cornflower_blue-50;
}

h2 {
  @apply text-2xl;
  @apply font-bold;
  @apply mb-4;
  @apply text-center;
  @apply text-cornflower_blue-900;
}

input#fileUpload {
  font-size: 0;
}
/* 
input#fileUpload::file-selector-button {
  font-size: large !important;
  @apply border-0;
  @apply bg-cornflower_blue-700;
  @apply text-white;
  @apply rounded-xl;
  @apply mx-auto;
  @apply px-4;
  @apply py-2;

  @apply rounded-full;
  @apply bg-persian_blue-500;
  @apply text-xl;
  @apply py-2;
  @apply px-8;
  @apply hover:bg-persian_blue-400;
} */
input#fileUpload::file-selector-button,
button,
input[type='button'] {
  @apply border-0;
  @apply rounded-full;
  @apply bg-persian_blue-500;
  @apply text-xl;
  @apply py-2;
  @apply px-8;
  @apply hover:bg-persian_blue-400;
  @apply text-white;
}

a {
  @apply underline;
  @apply decoration-aquamarine-200;
  @apply underline-offset-4;
  @apply decoration-2;
}

ol li {
  @apply list-decimal;
  @apply pb-2;
}

dialog > * {
  @apply w-11/12;
  @apply mx-auto;
}
</style>

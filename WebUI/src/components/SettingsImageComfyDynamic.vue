<template>
  <div
    v-for="input in imageGeneration.comfyInputs"
    :key="`${input.nodeTitle}${input.nodeInput}`"
    class="flex flex-col gap-2 py-2"
  >
    <p>{{ languages[getTranslationLabel(input.label)] ??= input.label }}</p>

    <!--    Number    -->
    <slide-bar
      v-if="input.type === 'number'"
      v-model:current="input.current.value as number"
      :min="input.min"
      :max="input.max"
      :step="input.step"
    ></slide-bar>

    <!--    Image    -->
    <LoadImage
      :id="`${input.nodeTitle}.${input.nodeInput}`"
      v-if="input.type === 'image'"
      :image-url-ref="input.current as WritableComputedRef<string>"
    ></LoadImage>

    <!--    String    -->
    <Input
      v-if="input.type === 'string'"
      type="text"
      v-model="input.current.value as string"
    ></Input>

    <!--    Boolean    -->
    <button
      v-if="input.type === 'boolean'"
      class="v-checkbox-control flex-none w-5 h-5"
      :class="{ 'v-checkbox-checked': input.current.value }"
      @click="() => (input.current.value = !input.current.value)"
    ></button>
  </div>
</template>

<script setup lang="ts">
import { useImageGeneration } from '@/assets/js/store/imageGeneration'
import { Input } from '../components/ui/input'
import { LoadImage } from '../components/ui/loadImage'
import SlideBar from '../components/SlideBar.vue'

const imageGeneration = useImageGeneration()

function getTranslationLabel(label: string) {
  return (
    'SETTINGS_IMAGE_COMFY_' +
    label.replace(/ - /g, '_').replace(/-/g, '_').replace(/ /g, '_').toUpperCase()
  )
}
</script>

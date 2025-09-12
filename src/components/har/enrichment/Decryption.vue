<script setup>
import Code from "@/components/har/partials/Code.vue";
</script>

<script>
export default {
  components: {Code},
  props: ['decryption']
}
</script>

<template>
  <div class="my-2">
    <ul class="list-unstyled">
      <li class="">
        <span class="fw-bold text-unmuted">Algorithm: </span>
        <span class="font-monospace">{{ decryption.primitiveParameters.algorithm || '?' }}</span>
      </li>
      <li class="">
        <span class="fw-bold text-unmuted">Key: </span>
        <span class="font-monospace">{{ decryption.primitiveParameters.key || '?' }}</span>
      </li>
      <li class="">
        <span class="fw-bold text-unmuted">IV: </span>
        <span class="font-monospace">{{ decryption.primitiveParameters.iv || '?' }}</span>
      </li>
      <li class="" v-if="decryption.sizeDiffEncrypted || decryption.sizeDiffDecrypted">
        <span class="fw-bold text-unmuted">Sizes differences:</span>
        <ul>
          <li v-if="decryption.sizeDiffEncrypted">
            <span>matching cipher text - original cipher text</span><span class="font-monospace"> = {{ decryption.sizeDiffEncrypted }} bytes</span>
          </li>
          <li v-if="decryption.sizeDiffDecrypted">
            <span>matching plain text - original cipher text</span><span class="font-monospace"> = {{ decryption.sizeDiffDecrypted }} bytes</span>
          </li>
        </ul>
      </li>
    </ul>
    <div v-if="decryption.originalBase64Content">
      <Code title="Original data (base64)" :toggleable=false :content="decryption.originalBase64Content"></Code>
    </div>
  </div>
</template>

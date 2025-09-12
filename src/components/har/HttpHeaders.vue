<script setup>
import CopyBtn from "@/components/har/generic/CopyBtn.vue";
</script>

<script>
export default {
  components: {CopyBtn},
  props: ['headers']
}
</script>

<template>

  <ul class="list-unstyled">
    <li class="" v-for="h in headers">
      <CopyBtn>
        <span class="fw-bold">{{ h.name }}</span>:
        <span class="text-break">{{ h.value }}</span>
      </CopyBtn>
    </li>
  </ul>

</template>

<style scoped>

</style>

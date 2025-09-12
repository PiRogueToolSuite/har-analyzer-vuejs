<script setup>
import CopyBtn from "@/components/har/generic/CopyBtn.vue";
</script>

<script>
export default {
  components: {CopyBtn},
  props: ['entry']
}
</script>

<template>

  <ul class="list-unstyled">
    <li class="" v-for="st in entry.headers">
      <CopyBtn>
        <span class="fw-bold">{{ st.name }}</span>:
        <span class="text-break">{{ st.value }}</span>
      </CopyBtn>
    </li>
  </ul>

</template>

<style scoped>

</style>

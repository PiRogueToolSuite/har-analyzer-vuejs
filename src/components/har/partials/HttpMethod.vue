<script setup>
import {Tag} from "primevue";
</script>

<script>
export default {
  props: ['method', 'status']
}
</script>

<template>
  <Tag class="me-1" icon="pi pi-times" :value="method" severity="danger" v-if="status>=400"/>
  <Tag class="me-1" icon="pi pi-check" :value="method" severity="success" v-else/>
</template>
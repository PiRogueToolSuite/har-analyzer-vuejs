<script setup>
import {Tag} from "primevue";
</script>

<script>
export default {
  props: ['entry'],
  computed: {
    headersSize() {
      return this.entry.request.headersSize + (this.entry.response?.headersSize ?? 0);
    },
    requestBodySize() {
      return this.entry.request._content?.size ?? this.entry.request.postData?._size ?? this.entry.request.bodySize ?? 0;
    },
    responseBodySize() {
      return this.entry.response?.content?.size ?? this.entry.response?.bodySize ?? 0;
    },
  },
}
</script>

<template>
<Tag class="me-1" icon="pi pi-cloud-download"
     :value="$humanFileSize(headersSize + requestBodySize + responseBodySize)"
     severity="secondary" />
</template>

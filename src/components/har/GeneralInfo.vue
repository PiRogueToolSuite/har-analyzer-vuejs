<script setup>
import CopyBtn from "@/components/har/generic/CopyBtn.vue";
</script>

<script>
export default {
  props: ['entry'],
  computed: {
    isAborted() {
      return !this.entry.response?.status;
    },
  }
}
</script>

<template>
  <ul class="list-unstyled">
    <li>
      <span class="text-unmuted fw-bold">URL</span>:
      <CopyBtn>{{ entry.request.url }}</CopyBtn>
    </li>
    <li>
      <span class="text-unmuted fw-bold">Method</span>:
      <CopyBtn>{{ entry.request.method }}</CopyBtn>
    </li>
    <li>
      <span class="text-unmuted fw-bold">Status</span>:
      <span class="text-danger" v-if="isAborted">Aborted</span>
      <CopyBtn v-else>{{ entry.response.status }}</CopyBtn>
    </li>
    <li>
      <span class="text-unmuted fw-bold">Timestamp</span>:
      <CopyBtn>{{ entry.startedDateTime }}</CopyBtn>
    </li>
    <li>
      <span class="text-unmuted fw-bold">Duration</span>:
      <CopyBtn>{{ entry.time.toFixed(2) }}ms</CopyBtn>
    </li>
    <li>
      <span class="text-unmuted fw-bold">Remote IP</span>:
      <CopyBtn v-if="entry.serverIPAddress">{{ entry.serverIPAddress }}</CopyBtn>
      <span class="text-danger" v-else>unavailable</span>
    </li>
    <li v-if="entry._communityId">
      <span class="text-unmuted fw-bold">Community ID</span>:
      <CopyBtn><span class="font-monospace">{{ entry._communityId }}</span></CopyBtn>
    </li>
    <li v-if="entry._sha1Id">
      <span class="text-unmuted fw-bold">SHA1</span>:
      <CopyBtn><span class="font-monospace">{{ entry._sha1Id }}</span></CopyBtn>
    </li>
  </ul>
</template>

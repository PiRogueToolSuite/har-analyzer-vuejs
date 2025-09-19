<script>
export default {
  props: ['communication', 'stacktrace']
}
</script>

<template>
<span v-if="stacktrace?.process">
  <span class="pi pi-wave-pulse me-2 text-primary" ></span>
  <span class="font-monospace">{{ stacktrace.process }}</span>
  <span class="pi pi-code mx-4" ></span>
</span>
<span class="pi pi-mobile me-2 text-primary" ></span>
<span class="font-monospace">{{ communication.src.ip }}</span>
<span class="pi pi-code mx-4" ></span>
<span class="pi pi-server me-2 text-primary" ></span>
<span class="font-monospace">
{{ communication.dst.host }}:{{ communication.dst.port }}
({{ communication.dst.ip }})
</span>
</template>

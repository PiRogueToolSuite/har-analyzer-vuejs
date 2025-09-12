<script setup>
import Code from "@/components/har/partials/Code.vue";
</script>

<script>
export default {
  props: ['entry', 'body'],
  computed: {
    eitherBody: function() {
      return this.entry._content ?? this.body;
    },
    notPrintableBody: function() {
      return !!this.entry._content || !!this.body?.encoding;
    },
    bodyOptionallyIndentedIfJson: function () {
      const reqOrRespBody = this.body?.text; // only for printable body
      try {
        return JSON.stringify(JSON.parse(reqOrRespBody), null, 2);
      } catch {
        return reqOrRespBody;
      }
    }
  }
}
</script>

<template>
  <div>
    <div class="" v-if="notPrintableBody">
      <span class="pi pi-exclamation-triangle text-orange-400"></span>
      <span class="text-unmuted"> The original data is not printable</span>
    </div>
    <ul class="list-unstyled my-2">
      <li>
        <span class="font-bold text-unmuted">Mimetype: </span>
        <span class="font-monospace">{{ eitherBody.mimeType || '?' }}</span>
      </li>
      <li v-if="notPrintableBody">
        <span class="font-bold text-unmuted">Encoding: </span>
        <span class="font-monospace">{{ eitherBody.encoding }}</span>
      </li>
    </ul>
    <Code title="Data" :toggleable=false :content="eitherBody.text" v-if="notPrintableBody"></Code>
    <Code title="Data" :toggleable=false :content="bodyOptionallyIndentedIfJson" v-else></Code>
  </div>
</template>

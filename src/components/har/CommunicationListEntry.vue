<script setup>
import {Tag} from "primevue";
</script>

<script>
export default {
  props: ['entry', 'timeRange'],
  computed: {
    hostname() {
      const url = new URL(this.entry.request.url);
      return url.hostname;
    },
    isAborted() {
      return !this.entry.response?.status;
    },
    isDataUrl() {
      return this.entry.request?.url?.startsWith('data:');
    },
    headersSize() {
      return Math.max(0, this.entry.request.headersSize ?? 0) + Math.max(0, this.entry.response?.headersSize ?? 0);
    },
    requestBodySize() {
      return this.entry.request._content?.size ?? this.entry.request.postData?._size ?? this.entry.request.bodySize ?? 0;
    },
    responseBodySize() {
      return Math.max(0, this.entry.response?.content?.size ?? this.entry.response?.bodySize ?? 0);
    },
    processName() {
        const reqProcess = this.entry.request._stacktrace?.process;
        const respProcess = this.entry.response?._stacktrace?.process;
        if(!reqProcess) return respProcess;
        if(!respProcess) return reqProcess;
        if(reqProcess != respProcess) {
            console.log(
                `Ambiguous process name for entry ID ${this.entry._sha1Id} (${reqProcess} != ${respProcess})`
            );
        }
        return reqProcess;
    }
  }
}
</script>

<template>
  <div style="white-space: nowrap; overflow-x: hidden;">
    <div class="d-flex flex-row">
      <div class="p-0" style="vertical-align: sub;" v-if="entry._ogreCrumbs">
        <img src="/src/assets/ogre_logo_CC_Lima_Studio.svg" height="20"/>
      </div>
      <div class="p-0 mx-1">
        <Tag class="" :value="entry.request.method" severity="danger" v-if="isAborted || entry.response.status>=400" />
        <Tag class="" :value="entry.request.method" severity="success" v-else />
      </div>
      <div class="p-1">
        <span class="pi pi-download text-muted" v-if="isDataUrl"></span>
        <span class="pi pi-cloud-download text-unmuted" v-else></span>
        <span class="ms-1 text-danger" v-if="isAborted">
          Aborted
        </span>
        <span class="ms-1 text-unmuted" v-else>
          {{ $humanFileSize(headersSize + requestBodySize + responseBodySize) }}
        </span>
      </div>
      <div class="p-1 text-primary" v-if="isDataUrl">
        <span class="pi pi-file"></span>
        local content
      </div>
      <div class="p-1 text-primary" v-else>
        <span class="pi pi-globe"></span>
        {{ hostname }}
      </div>
    </div>
    <div class="d-flex flex-row ">
      <div class="p-1 text-unmuted font-monospace" v-if="processName">
        <span class="pi pi-wave-pulse" ></span>
        <span class="mx-1 text-sm">{{ processName }}</span>
      </div>
      <div class="p-1 text-unmuted font-monospace" v-else-if="isDataUrl">
        <span class="pi pi-code" ></span>
        <span class="mx-1 text-sm">local script</span>
      </div>
      <div class="p-1 text-unmuted font-monospace" v-else-if="entry.serverIPAddress">
        <span class="pi pi-server" ></span>
        <span class="mx-1 text-sm">{{ entry.serverIPAddress }}</span>
      </div>
    </div>
  </div>
</template>

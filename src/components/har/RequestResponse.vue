<script setup>
import HttpBody from "@/components/har/HttpBody.vue";
import Stacktrace from "@/components/har/enrichment/Stacktrace.vue";
import HttpHeaders from "@/components/har/HttpHeaders.vue";
import Decryption from "@/components/har/enrichment/Decryption.vue";
import {Tabs, Tab, TabList, TabPanels, TabPanel, Badge} from "primevue";
</script>

<script>
export default {
  props: ['entry', 'body'],
  methods: {}
}
</script>

<template>
  <Tabs value="0">
    <TabList>
      <Tab value="0">
        <span class="pi pi-list"></span>
        Headers
        <Badge :value="$humanFileSize(entry.headersSize)" severity="secondary" size="small"></Badge>
      </Tab>
      <Tab value="1" v-if="entry.bodySize>0 || body?.size > 0">
        <span class="pi pi-file"></span>
        Body
        <Badge :value="$humanFileSize(body?._size || body?.size)" severity="secondary" size="small"></Badge>
      </Tab>
      <Tab value="2" v-if="entry._stacktrace">
        <span class="pi pi-sitemap"></span>
        Stacktrace
      </Tab>
      <Tab value="3" v-if="entry._decryption">
        <span class="pi pi-lock-open"></span>
        Decryption
      </Tab>
    </TabList>
    <TabPanels>
      <TabPanel value="0">
        <HttpHeaders :entry="entry"/>
      </TabPanel>
      <TabPanel value="1" v-if="entry.bodySize>0 || body?.size > 0">
        <HttpBody :entry="entry" :body="body"/>
      </TabPanel>
      <TabPanel value="2" v-if="entry._stacktrace">
        <Stacktrace :entry="entry"/>
      </TabPanel>
      <TabPanel value="3" v-if="entry._decryption">
        <Decryption :entry="entry"/>
      </TabPanel>
    </TabPanels>
  </Tabs>
</template>
<script setup>
import CommunicationListEntry from "@/components/har/CommunicationListEntry.vue";
import RequestResponse from "@/components/har/RequestResponse.vue";
import GeneralInfo from "@/components/har/GeneralInfo.vue";
import HttpMethod from "@/components/har/partials/HttpMethod.vue";
import HttpStatus from "@/components/har/partials/HttpStatus.vue";
import TotalSize from "@/components/har/partials/TotalSize.vue";
import CommunicationLine from "@/components/har/partials/CommunicationLine.vue";
import SecureHttp from "@/components/har/partials/SecureHttp.vue";
import Detection from "@/components/har/enrichment/Detection.vue";
import {Tabs, Tab, TabPanels, TabPanel, TabList, Splitter, SplitterPanel, Tag, Listbox} from "primevue";
</script>

<script>
const loadDataSource = async (url) => {
  const response = await fetch(url);
  const json = await response.json();
  return json.log.entries;
};

export default {
  props: ['dataSrc'],
  expose: ['setEntries'],
  data() {
    return {
      selectedEntry: null,
      entries: [],
    }
  },
  mounted() {
    if (this.dataSrc) {
      const url = this.dataSrc;
      loadDataSource(url).then(this.setEntries);
    }
  },
  methods: {
    setEntries(entriesArray) {
      this.selectedEntry = null;
      this.entries.splice(0, this.entries.length, ...entriesArray);
    },
  },
  computed: {
    isDataUrl() {
      return this.selectedEntry?.request?.url?.startsWith('data:');
    },
  }
}
</script>

<template>
  <div class="har-analyzer p-2 items-center justify-center h-100">
    <Splitter class="h-100">
      <SplitterPanel class="flex p-0 h-100" :size="33" :minSize="25">
        <Listbox v-model="selectedEntry" :options="entries" optionLabel="request" scrollHeight="100%" class="h-100 no-border" :striped=true>
          <template #option="slotProps">
            <CommunicationListEntry :entry="slotProps.option"></CommunicationListEntry>
          </template>
        </Listbox>
      </SplitterPanel>
      <SplitterPanel class="flex p-4 overflow-auto" :size="75" v-model="selectedEntry">
        <div class="h-100" v-if="selectedEntry">
          <div class="">
            <div class="text-wrap text-primary">
              <div v-if="isDataUrl">
                <span class="pi pi-file" style="font-size: 1rem"></span>
                <span class="mx-2 font-monospace">local content</span>
              </div>
              <div v-else>
                <span class="pi pi-globe" style="font-size: 1rem"></span>
                <span class="mx-2 font-monospace">{{ selectedEntry.request.url }}</span>
              </div>
            </div>
            <div class="text-wrap my-2">
              <HttpMethod v-model:method="selectedEntry.request.method" v-model:status="selectedEntry.response.status"/>
              <HttpStatus v-model:status="selectedEntry.response.status"/>
              <SecureHttp :request="selectedEntry.request"/>
              <TotalSize v-model:entry="selectedEntry"/>
              <Tag class="me-1" icon="pi pi-stopwatch" :value="selectedEntry.time.toFixed(1)+' ms'" severity="secondary"/>
            </div>
            <div class="mt-3 mb-0 " v-if="selectedEntry.request._communication">
              <CommunicationLine v-model:entry="selectedEntry.request"/>
            </div>
          </div>
          <Tabs value="1" class="">
            <TabList>
              <Tab value="0">General</Tab>
              <Tab value="1">Request</Tab>
              <Tab value="2">Response</Tab>
              <Tab value="3" v-if="selectedEntry._ogreCrumbs">Data identification</Tab>
            </TabList>
            <TabPanels class="mx-0 p-0">
              <TabPanel value="0">
                <div class="m-3">
                  <GeneralInfo v-model:entry="selectedEntry"/>
                </div>
              </TabPanel>
              <TabPanel value="1">
                <div class="m-0">
                  <RequestResponse
                    v-model:entry="selectedEntry.request"
                    v-model:body="selectedEntry.request.postData" class=""></RequestResponse>
                </div>
              </TabPanel>
              <TabPanel value="2">
                <div class="m-0">
                  <RequestResponse
                    v-model:entry="selectedEntry.response"
                    v-model:body="selectedEntry.response.content" class=""></RequestResponse>
                </div>
              </TabPanel>
              <TabPanel value="3" v-if="selectedEntry._ogreCrumbs">
                <div class="m-0">
                  <Detection
                    v-model:ogre="selectedEntry._ogreCrumbs"
                    class=""></Detection>
                </div>
              </TabPanel>
            </TabPanels>
          </Tabs>
        </div>
        <div v-else>
          <span class="pi pi-arrow-left text-muted"></span>
          <span class="ms-2 text-muted">Please select an entry to explore transaction details.</span>
        </div>
      </SplitterPanel>
    </Splitter>
  </div>
</template>

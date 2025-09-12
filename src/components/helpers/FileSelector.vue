<script setup>
import {useTemplateRef} from "vue";
import HarAnalyzer from "@/components/HarAnalyzer.vue";
import {FileUpload} from "primevue";

const harAnalyzer = useTemplateRef('harAnalyzer');

function readHarEntries(file) {
  return new Promise(function(resolve, reject){
    const reader = new FileReader();
    reader.onload = function () {
      const har = JSON.parse(this.result).log;
      console.debug(`loaded ${har.entries.length} entries from ${file.name}`, har.creator);
      resolve(har.entries);
    };
    reader.onerror = function(err) {
      reject(err);
    }
    reader.readAsText(file);
  });
}

function onHarSelection(event) {
  Promise.all(event.files.map(f => readHarEntries(f))).then(arrHarEntries => {
    const allHarEntries = arrHarEntries.flat();
    console.log(`loaded ${allHarEntries.length} entries in total`, allHarEntries.map(e => e._sha1Id));
    harAnalyzer.value.setEntries(allHarEntries);
  }).catch(console.error);
}
</script>

<template>
  <div class="flex justify-end px-3">
    <FileUpload mode="basic" accept=".har" @select="onHarSelection" customUpload multiple auto
                severity="secondary" class="p-button-outlined inline-flex h-8 text-sm "/>
  </div>

  <HarAnalyzer ref='harAnalyzer'></HarAnalyzer>
</template>

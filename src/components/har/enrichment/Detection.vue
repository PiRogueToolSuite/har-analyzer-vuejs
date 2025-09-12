<script setup>
import {DataTable, Column} from "primevue";
</script>

<script>
export default {
  props: ['ogre'],
  data() {
    return {
      expandedRows: {}
    };
  },
  methods: {
    sortKeys: function (obj) {
      return Object.keys(obj).sort().reduce((result, key) => {
        result[key] = obj[key];
        return result;
      }, {});
    },
  },
  computed: {
    detections: function () {
      let data = [];
      const sortedObj = this.sortKeys(this.ogre.detected_data);
      for (const [key, values] of Object.entries(sortedObj)) {
        let exampleValue = values[0].value;
        let hasMultipleValues = values.length > 1 && values.slice(1).some((v) => v.value != exampleValue);
        data.push({
          type: key,
          count: values.length,
          example: exampleValue + (hasMultipleValues ? ' ; …': ''),
          details: values
        })
      }
      return data;
    }
  }
}
</script>

<template>
  <div class="my-2">
    <DataTable v-model:expandedRows="expandedRows" :value="detections" dataKey="type" size="small">
      <Column expander style="width: 1rem"/>
      <Column field="type" header="" sortable>
        <template #header>
          <span class="text-unmuted fw-bold">Data category</span>
        </template>
        <template #body="slotProps">
          <span class="text-primary">{{ slotProps.data.type }}</span>
        </template>
      </Column>
      <Column field="count" header="" sortable>
        <template #header>
          <span class="text-unmuted fw-bold">Count</span>
        </template>
      </Column>
      <Column field="example" header="">
        <template #header>
          <span class="text-unmuted fw-bold">Value</span>
        </template>
        <template #body="slotProps">
          <span class="font-monospace text-unmuted">{{ slotProps.data.example }}</span>
        </template>
      </Column>
      <template #expansion="slotProps">
        <div class="m-0 p-0 text-sm border-0">
          <DataTable :value="slotProps.data.details" size="small" class="">
            <Column field="part" header="" class="col-2">
              <template #header>
                <span class="fw-bold text-unmuted">Part</span>
              </template>
              <template #body="slotProps">
                <span class="text-unmuted">{{ slotProps.data.part }}</span>
              </template>
            </Column>
            <Column field="section" header="" class="col-2">
              <template #header>
                <span class="fw-bold text-unmuted">Section</span>
              </template>
              <template #body="slotProps">
                <span class="text-unmuted">{{ slotProps.data.section }}</span>
              </template>
            </Column>
            <Column field="pointer" header="" class="col-4">
              <template #header>
                <span class="fw-bold text-unmuted">Path</span>
              </template>
              <template #body="slotProps">
                <samp class="text-unmuted">{{ slotProps.data.pointer }}</samp>
              </template>
            </Column>
            <Column field="value" header="" class="col-4">
              <template #header>
                <span class="fw-bold text-unmuted">Value</span>
              </template>
              <template #body="slotProps">
                <samp class="text-unmuted">{{ slotProps.data.value }}</samp>
              </template>
            </Column>
          </DataTable>
        </div>
      </template>
    </DataTable>
  </div>
</template>

<script setup>
import {ButtonGroup, Card} from "primevue";
import CopyBtn from "@/components/har/generic/CopyBtn.vue";
</script>

<script>
export default {
  components: {ButtonGroup, Card},
  props: ['content', 'title'],
  methods: {}
}
</script>

<template>
  <div v-if="content">
    <Card>
      <template #header>
        <div class="pt-2 pb-0 border-bottom d-flex justify-content-between">
          <div class="pt-2">
            <span class="fw-bold">{{ title }}</span>
          </div>
          <div class="p-0 m-0">
            <ButtonGroup>
              <CopyBtn :content="content" text="Copy"></CopyBtn>
            </ButtonGroup>
          </div>
        </div>
      </template>
      <template #content>
        <pre class="mb-0 border border-primary text-unmuted rounded p-2"
             style="margin-left: -16px; width: calc(100% + 32px); text-wrap: wrap;"><code>{{ content }}</code></pre>
      </template>
    </Card>
  </div>
</template>

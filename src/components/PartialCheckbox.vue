<!--
 Copyright 2021 Google LLC

 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
-->

<template>
  <v-btn block type="button" class="my-5" @click="revertAllSelection">
    {{ selectAllText[selectAll] }}
  </v-btn>
  <v-row class="mb-5">
    <v-col v-for="label in labels" :key="label" cols="12" md="4">
      <input
        type="checkbox"
        :value="label"
        :checked="modelValue.get(label)"
        @change="updateSelected($event.target.value)"
      />
      <label v-if="label"> {{ label }} </label>
    </v-col>
  </v-row>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    labels: {
      type: Array,
      default: new Array()
    },
    modelValue: {
      type: Map,
      default: new Map()
    }
  },
  data() {
    return {
      selectAll: 1,
      selectAllText: ['Select All', 'Unselect All']
    }
  },
  mounted() {
    // Set the default value to be true once mounted
    for (let key of this.labels) {
      this.modelValue.set(key, true)
    }
  },
  methods: {
    updateSelected(newSelect: string) {
      this.modelValue.set(newSelect, !this.modelValue.get(newSelect))
      this.$emit('update:modelValue', this.modelValue)
    },
    revertAllSelection() {
      this.selectAll = 1 - this.selectAll
      for (let key of this.modelValue.keys()) {
        this.modelValue.set(key, Boolean(this.selectAll))
      }
    }
  }
})
</script>

<style scoped>
ul > li {
  display: inline-block;
  list-style-type: none;
  margin-left: 5%;
  margin-right: 5%;
  top: 0px;
  height: 50px;
}
</style>

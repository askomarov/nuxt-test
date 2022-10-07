<script setup>
import { TabGroup, TabList, Tab, TabPanels, TabPanel } from "@headlessui/vue";
</script>
<template>
  <div class="team-page container">
    <h2>Team page</h2>
    <TabGroup>
      <TabList>
        <Tab
          as="template"
          v-slot="{ selected }"
        >
          <button
            class="tab-btn"
            :class="{
              active: selected,
            }"
          >
            Edie
          </button>
        </Tab>
        <Tab
          as="template"
          v-slot="{ selected }"
        >
          <button
            class="tab-btn"
            :class="{
              active: selected,
            }"
          >
            John
          </button>
        </Tab>
        <Tab
          as="template"
          v-slot="{ selected }"
        >
          <button
            class="tab-btn"
            :class="{
              active: selected,
            }"
          >
            Mary
          </button>
        </Tab>
      </TabList>
      <TabPanels>
        <TabPanel>Content 1</TabPanel>
        <TabPanel>Content 2</TabPanel>
        <TabPanel>Content 3</TabPanel>
      </TabPanels>
    </TabGroup>
  </div>
</template>

<style lang="scss" scoped>
.team-page {
  [role="tablist"] {
    border: 1px solid #000;
    padding: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
.tab-btn {
  @apply px-4 py-1 rounded-md;

  &.active {
    @apply bg-blue-400;
  }
}
</style>

<template>
  <div>
    <h1>Basic CRUD APP With VUE.JS</h1>
    <div class="names-manager-wrapper">
      <input class="search-bar" v-model="search" placeholder="search" />
      <div class="main-row">
        <select v-model="selected" size="10" class="name-database">
          <option
            v-for="(name, i) in showNames"
            :key="name"
            :value="i"
            class="name-row"
          >
            {{ name }}
          </option>
        </select>
        <div class="input-fields-wrapper">
          <div class="field-group">
            <div class="first-field">
              <label for="firstname-input">Firstname</label>
              <input id="firstname-input" v-model="ninput.first" />
            </div>
            <div class="last-field">
              <label for="lastname-input">Lastname</label>
              <input id="lastname-input" v-model="ninput.last" />
            </div>
          </div>
          <div class="btn-group">
            <button @click="createName">Create</button>
            <button @click="updateName">Update</button>
            <button @click="deleteName">Delete</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch } from "vue";
const search = ref<string>("");

const ninput = ref<{
  first: string;
  last: string;
}>({
  first: "",
  last: "",
});

// names database
const names = ref<string[]>([]);

const selected = ref<number | undefined>(undefined);

const showNames = computed(() =>
  names.value.filter((n) => n.startsWith(search.value))
);

watch([selected], () => {
  if (selected.value === undefined) {
    resetInput();
    return;
  }
  [ninput.value.last, ninput.value.first] =
    names.value[selected.value].split(", ");
});

function createName() {
  const fullname = formatFullname(ninput.value.first, ninput.value.last);
  if (!hasName(fullname)) {
    names.value.push(fullname);
    resetInput();
  }
}

function updateName() {
  console.log(selected.value);
  if (selected.value !== undefined) {
    names.value = names.value.map((n, i) => {
      if (i === selected.value) {
        return formatFullname(ninput.value.first, ninput.value.last);
      }
      return n;
    });
  }
}

function deleteName() {
  if (selected.value !== undefined) {
    names.value.splice(selected.value, 1);
    resetInput();
    unselectName();
  }
}

function unselectName() {
  selected.value = undefined;
}

function hasName(name: string): boolean {
  return names.value.includes(name);
}

function resetInput() {
  ninput.value = { last: "", first: "" };
}

function formatFullname(first: string, last: string): string {
  return `${last}, ${first}`;
}
</script>

<style scoped>
.names-manager-wrapper {
  font-size: 1rem;
  display: flex;
  flex-direction: column;
  align-items: start;
}
.search-bar {
  padding: 0.3rem;
  width: 285px;
  margin-bottom: 12px;
}
.main-row {
  display: flex;
}
.name-database {
  width: 300px;
}
.input-fields-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.input-fields-wrapper input {
  padding: 0.3rem;
  margin-left: 10px;
}
.field-group {
  display: flex;
  flex-direction: column;
  gap: 24px;
}
.btn-group {
  display: flex;
  gap: 4px;
}
.name-row {
  width: 100%;
}
</style>

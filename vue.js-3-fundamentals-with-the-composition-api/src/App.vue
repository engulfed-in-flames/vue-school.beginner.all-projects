<script setup lang="ts">
import { ref, computed } from 'vue';

interface ShoppingItem {
  id: number;
  label: string;
  purchased: boolean;
  highPriority: boolean;
}

interface ShoppingItemList extends Array<ShoppingItem> {}

// `reactive`: You can not override the entire reactive variable state in the instance.
// In most cases, you can use `ref` instead of `reactive`.
// If you don't want to use `.value`, then you can use `reactive` instead of `ref`.
const header = ref('Shopping List App');
const editing = ref(true);
const items = ref<ShoppingItemList>([
  { id: 1, label: '10 party hats', purchased: true, highPriority: false },
  { id: 2, label: '2 board games', purchased: true, highPriority: false },
  { id: 3, label: '20 cups', purchased: false, highPriority: true },
]);

const newItem = ref('');
const newItemHasHighPriority = ref(false);

const charactersCount = computed(() => newItem.value.length);
const reversedItems = computed(() => [...items.value].reverse());

const saveItem = () => {
  const _newItem: ShoppingItem = {
    id: items.value.length + 1,
    label: newItem.value,
    purchased: false,
    highPriority: newItemHasHighPriority.value,
  };
  items.value.push(_newItem);

  newItem.value = '';
  newItemHasHighPriority.value = false;
};

const doEdit = (value: boolean) => {
  editing.value = value;
};

const togglePurchased = (item: ShoppingItem) => {
  item.purchased = !item.purchased;
};
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="!editing" @click="doEdit(true)" class="btn btn-primary">
      Add Item
    </button>
    <button v-else @click="doEdit(false)" class="btn">Cancel</button>
  </div>
  <form v-if="editing" @submit.prevent="saveItem" class="add-item-form">
    <input
      v-model="newItem"
      @keyup.enter="saveItem"
      type="text"
      name="newItem"
      placeholder="Add an item"
    />
    <label>
      <input
        type="checkbox"
        name="newItemHasHighPriority"
        v-model="newItemHasHighPriority"
      />
      High Priority
    </label>
    <button class="btn btn-primary" :disabled="newItem.length < 5">
      Save Item
    </button>
  </form>
  <p class="counter">{{ charactersCount }} / 200</p>
  <ul v-if="items.length">
    <li
      v-for="(item, index) in reversedItems"
      :key="item.id"
      :class="{ strikeout: item.purchased, priority: item.highPriority }"
      @click="togglePurchased(item)"
    >
      {{ item.label }}
    </li>
  </ul>
  <p v-else>Nothing to see here</p>
</template>

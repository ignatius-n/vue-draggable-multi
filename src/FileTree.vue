<template>
  <draggable
    ref="drag"
    class="list-group"
    tag="ul"
    v-model="files"
    v-bind="dragOptions"
    multi-drag
    selected-class="multi-drag"
    @update="onUpdate"
    @add="onAdd"
    @remove="onRemove"
    @start="onDragStart"
    @end="onDragEnd"
  >
    <transition-group type="transition" :name="'flip-list'">
      <template v-for="file in files">
        <li
          class="list-group-item"
          :class="{ selected: selected[file.id] }"
          :key="file.id"
          @click="toggleSelect(file.id)"
        >
          <div class="name-area">{{ file.name }}</div>
        </li>
      </template>
    </transition-group>
  </draggable>
</template>

<script>
import draggable from "vuedraggable";

export default {
  components: {
    draggable,
  },
  props: {
    value: Array,
  },
  data() {
    return {
      selected: {},
      dragOptions: {
        animation: 150,
        group: "description",
        ghostClass: "ghost",
        disabled: false,
      },
      isDragging: false,
    };
  },
  computed: {
    files: {
      get() {
        return this.value;
      },
      set(files) {
        this.$emit("input", files);
      },
    },
  },
  methods: {
    onDragStart(e) {
      this.isDragging = true;
      e.item._selected_items = this.files.filter((f) => this.selected[f.id]);
    },
    onDragEnd() {
      this.isDragging = false;
      this.selected = {};
    },
    onUpdate(e) {
      if (e.item._selected_items.length) {
        const ids = e.item._selected_items.map(({ id }) => id);
        const files = this.files.filter((f) => !ids.includes(f.id));
        files.splice(e.newIndex, 0, ...e.item._selected_items);
        this.files = files;
      }
    },
    onRemove(e) {
      if (e.item._selected_items.length) {
        const ids = e.item._selected_items.map(({ id }) => id);
        this.files = this.files.filter((f) => !ids.includes(f.id));
      }
    },
    onAdd(e) {
      if (e.item._selected_items.length) {
        this.files.splice(e.newIndex, 1, ...e.item._selected_items);
      }
    },
    toggleSelect(id) {
      this.$set(this.selected, id, !this.selected[id]);
    },
  },
};
</script>

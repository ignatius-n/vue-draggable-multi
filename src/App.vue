<template>
  <div>
    <file-tree v-model="oddFiles" />
    <file-tree v-model="evenFiles" />
    <div class="row">
      <div class="col">
        <pre>{{ oddFiles }}</pre>
      </div>
      <div class="col">
        <pre>{{ evenFiles }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import FileTree from "./FileTree";

export default {
  components: {
    FileTree,
  },
  data() {
    return {
      oddFiles: [],
      evenFiles: [],
    };
  },
  created() {
    this.oddFiles = [1, 3, 5, 7, 9].map((id) => ({
      id,
      name: `File ${id}`,
    }));
    this.evenFiles = [2, 4, 6, 8, 10].map((id) => ({
      id,
      name: `File ${id}`,
    }));
  },
};
</script>

<style lang="scss">
.category-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 8px;
  border-style: inset;
}

.el-button-group .el-button {
  width: 30px;
  height: 30px;
}

.flip-list-move {
  transition: transform 0.5s;
}

.no-move {
  transition: transform 0s;
}

.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}

.list-group {
  min-height: 5px;
  background: #b4b3ff;
  padding: 4px;
}

.list-group-item {
  cursor: move;
  display: flex;
  border: 1px solid #efefef;
  padding: 3px;
  margin: 1px;

  &.selected {
    background-color: #f9c7c8;
    border: solid red 1px;
  }

  &.multi-drag {
    background-color: #f9c7c8;
    border: solid red 1px;
  }

  .name-area {
    flex: 1 1 auto;
  }

  .icon-area {
    flex: 0 0 auto;

    i {
      margin: 0px 10px;
      cursor: pointer;
    }
  }
}

.btn-default {
  color: #007bff;
  font-size: 14px;

  &:disabled {
    color: #999;
    cursor: not-allowed;
  }
}
</style>

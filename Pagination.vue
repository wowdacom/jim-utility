<template>
  <div>
    <h1>This is in HTTP Vue Loader</h1>
    <ul>
      <li>會遇到載入其他npm模組的問題</li>
      <li>
        <a
          href="https://medium.com/codememo/http-vue-loader-%E4%BD%BF%E7%94%A8-sass-%E7%9A%84%E6%96%B9%E6%B3%95-88fb34b153f5"
          >會遇到scss loader的問題</a
        >
      </li>
    </ul>
  </div>

  <!-- <c-box class="pagination-wrapper">
    <c-list display="flex" justify-content="center" align-items="center" py="5" class="pagination">
      <c-list-item class="pagination-item page-controller">
        <button
          type="button"
          @click="onClickFirstPage"
          :disabled="isInFirstPage"
          aria-label="Go to first page"
        >
          第一頁
        </button>
      </c-list-item>

      <c-list-item class="pagination-item page-controller">
        <button
          type="button"
          @click="onClickPreviousPage"
          :disabled="isInFirstPage"
          aria-label="Go to previous page"
        >
          上一頁
        </button>
      </c-list-item>

      <c-list-item
        :key="index"
        v-for="(page, index) in pages"
        class="pagination-item page-number"
      >
        <button
          type="button"
          @click="onClickPage(page.name)"
          :disabled="page.isDisabled"
          :class="{ 'current-page': isPageActive(page.name) }"
          :aria-label="`Go to page number ${page.name}`"
        >
          {{ page.name }}
        </button>
      </c-list-item>

      <c-list-item class="pagination-item page-controller">
        <button
          type="button"
          @click="onClickNextPage"
          :disabled="isInLastPage"
          aria-label="Go to next page"
        >
          下一頁
        </button>
      </c-list-item>

      <c-list-item class="pagination-item page-controller">
        <button
          type="button"
          @click="onClickLastPage"
          :disabled="isInLastPage"
          aria-label="Go to last page"
        >
          最後一頁
        </button>
      </c-list-item>
    </c-list>
  </c-box> -->

  <!-- <style lang="scss">
.pagination {
}

.pagination-item {
  margin: 5px;
  button {
    outline: none;
    &:active {
      background-color: #5a0cda;
      color: #ffffff;
      border-radius: 5px;
    }
    &.current-page {
      background-color: #5a0cda;
      width: 100%;
      color: #ffffff;
    }
  }
}

.pagination-item.page-controller {
  margin: 5px;
  button {
    padding: 5px 13px;
  }
}

.pagination-item.page-number {
  border: solid 1px gray;
  border-radius: 5px;
  text-align: center;
  width: 50%;
  max-width: 30px;
  position: relative;
  &:after {
    content: "";
    display: block;
    position: relative;
    padding-top: 100%;
  }
  button {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    width: 99%;
    height: 99%;
    text-align: center;
    z-index: 1;
    border: none;
  }
}
</style> -->
</template>

<script>
module.exports = {
  props: {
    maxVisibleButtons: {
      type: Number,
      required: false,
      default: 3,
    },
    totalPages: {
      type: Number,
      required: true,
    },
    total: {
      type: Number,
      required: true,
    },
    perPage: {
      type: Number,
      required: true,
    },
    currentPage: {
      type: Number,
      required: true,
    },
  },
  computed: {
    startPage() {
      if (this.currentPage <= (this.maxVisibleButtons + 1) / 2) {
        return 1;
      }

      if (
        this.currentPage >
        this.totalPages - (this.maxVisibleButtons + 1) / 2
      ) {
        return this.totalPages - this.maxVisibleButtons + 1;
      }

      return this.currentPage - (this.maxVisibleButtons + 1) / 2 + 1;
    },
    endPage() {
      return Math.min(
        this.startPage + this.maxVisibleButtons - 1,
        this.totalPages
      );
    },
    pages() {
      const range = [];

      for (let i = this.startPage; i <= this.endPage; i += 1) {
        range.push({
          name: i,
          isDisabled: i === this.currentPage,
        });
      }

      return range;
    },
    isInFirstPage() {
      return this.currentPage === 1;
    },
    isInLastPage() {
      return this.currentPage === this.totalPages;
    },
  },
  methods: {
    onClickFirstPage() {
      this.$emit("pagechanged", 1);
    },
    onClickPreviousPage() {
      this.$emit("pagechanged", this.currentPage - 1);
    },
    onClickPage(page) {
      console.log("hello");
      this.$emit("pagechanged", page);
    },
    onClickNextPage() {
      this.$emit("pagechanged", this.currentPage + 1);
    },
    onClickLastPage() {
      this.$emit("pagechanged", this.totalPages);
    },
    isPageActive(page) {
      return this.currentPage === page;
    },
  },
};
</script>


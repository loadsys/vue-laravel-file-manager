<template>
    <div class="fm-content d-flex flex-column">
        <div class="fm-search-wrapper">
            <div class="row align-items-center m-1">
                <label class="col-3" for="fm-search">Search:</label>
                <div class="col-9">
                    <input class="form-control fm-search" v-model="query" @input="debounceSearch" />
                </div>
            </div>
        </div>
        <disk-list v-bind:manager="manager" />
        <breadcrumb v-bind:manager="manager" />
        <div class="fm-content-body">
            <table-view v-if="viewType === 'table'" v-bind:manager="manager"/>
            <grid-view v-else v-bind:manager="manager"/>
        </div>
    </div>
</template>

<script>
// Components
import DiskList from './DiskList.vue';
import Breadcrumb from './Breadcrumb.vue';
import TableView from './TableView.vue';
import GridView from './GridView.vue';
import managerHelper from './mixins/manager';

export default {
  name: 'Manager',
  mixins: [managerHelper],
  components: {
    DiskList,
    Breadcrumb,
    TableView,
    GridView,
  },
  props: {
    manager: { type: String, required: true },
  },
  computed: {
    /**
     * view type - grid or table
     * @returns {default.computed.viewType|(function())|string}
     */
    viewType() {
      return this.$store.state.fm[this.manager].viewType;
    },
  },
  watch: {
    selectedDirectory() {
      this.query = null;
    },
  },
  data() {
    return {
      query: null,
      debounce: null,
    };
  },
  methods: {
    debounceSearch() {
      clearTimeout(this.debounce);
      this.debounce = setTimeout(() => {
        this.selectDirectory(this.selectedDirectory, this.query);
      }, 600);
    },
  },
};
</script>

<style lang="scss">
    .fm-content {
        height: 100%;
        padding-left: 1rem;

        .fm-content-body {
            overflow: auto;
        }
    }

    .fm-search-wrapper {
      margin-left: auto;
    }
</style>

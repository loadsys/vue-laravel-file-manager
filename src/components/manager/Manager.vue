<template>
    <div class="fm-content d-flex flex-column">
        <div class="ms-auto">
            <div class="row align-items-center m-1">
                <label class="col-3" for="fm-search">Search:</label>
                <div class="col-9">
                    <input class="form-control" id="fm-search" @input="debounceSearch" />
                </div>
            </div>
        </div>
        <disk-list v-bind:manager="manager" />
        <bread-crumb v-bind:manager="manager" />
        <div class="fm-content-body">
            <table-view v-if="viewType === 'table'" v-bind:manager="manager" />
            <grid-view v-else v-bind:manager="manager" />
        </div>
    </div>
</template>

<script>
// Components
import DiskList from './DiskList.vue';
import BreadCrumb from './BreadCrumb.vue';
import TableView from './TableView.vue';
import GridView from './GridView.vue';
import managerHelper from './mixins/manager';

export default {
    name: 'Manager',
    mixins: [managerHelper],
    components: {
        DiskList,
        BreadCrumb,
        TableView,
        GridView,
    },
    props: {
        manager: { type: String, required: true },
    },
    data() {
        return {
            debounce: null,
        };
    },
    computed: {
        /**
         * view type - grid or table
         * @returns {any}
         */
        viewType() {
            return this.$store.state.fm[this.manager].viewType;
        },
    },
    methods: {
        debounceSearch(event) {
            clearTimeout(this.debounce);
            this.debounce = setTimeout(() => {
                this.selectDirectory(this.selectedDirectory, event.target.value);
            }, 600);
        },
    },
};
</script>

<style lang="scss">
.fm-content {
    padding-left: 1rem;

    .fm-content-body {
        overflow: auto;
    }
}
</style>

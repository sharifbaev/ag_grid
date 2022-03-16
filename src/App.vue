<template>
  <div>
    <button @click="getSelectedRows()">Get Selected Rows</button>
    <ag-grid-vue style="width: 500px; height: 500px;"
        class="ag-theme-alpine"
        :columnDefs="columnDefs"
        :rowData="rowData"
        :autoGroupColumnDef="autoGroupColumnDef"
        @grid-ready="onGridReady">
    </ag-grid-vue>
  </div>
</template>

<script>
import { AgGridVue } from "ag-grid-vue";

export default {
  name: "App",
  data() {
    return {
      columnDefs: null,
      rowData: null,
      autoGroupColumnDef: {
        headerName: 'Model',
        field: 'model',
        cellRenderer: 'agGroupCellRenderer',
        cellRendererParams: {
          checkbox: true
        }
      }
    };
  },
  components: {
    AgGridVue,
  },
  beforeMount() {
     this.columnDefs = [
       { field: 'make', sortable: true, filter: true, checkboxSelection: true, rowGroup: true },
       { field: 'model', sortable: true, filter: true },
       { field: 'price', sortable: true, filter: true }
     ];

     fetch('https://www.ag-grid.com/example-assets/row-data.json')
             .then(result => result.json())
             .then(rowData => this.rowData = rowData);
   },
   methods: {
     onGridReady(params) {
         this.gridApi = params.api;
         this.columnApi = params.columnApi;
     },
     getSelectedRows() {
         const selectedNodes = this.gridApi.getSelectedNodes();
         const selectedData = selectedNodes.map( node => node.data );
         const selectedDataStringPresentation = selectedData.map( node => `${node.make} ${node.model}`).join(', ');
         alert(`Selected nodes: ${selectedDataStringPresentation}`);
     }
  },
};
</script>
<style lang="scss">
  @import "~ag-grid-community/src/styles/ag-grid.scss";
  @import "~ag-grid-community/src/styles/ag-theme-alpine/sass/ag-theme-alpine-mixin.scss";

  .ag-theme-alpine {
      @include ag-theme-alpine((
          odd-row-background-color: #ACE
      ));
  }
</style>
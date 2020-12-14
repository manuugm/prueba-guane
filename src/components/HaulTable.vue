<template>
  <div class="haul-table">
    <div class="haul-table__buttons">
      <b-button
        class="haul-table__button"
        variant="danger"
        @click="deleteSelected"
        >Delete</b-button
      >
      <b-button variant="success" @click="addNewRow">Add new row</b-button>
    </div>
    <b-table striped outlined hover :fields="fields" :items="tableRows">
      <template #head(checkbox)>
        <b-form-checkbox
          id="checkbox-head"
          name="checkbox-head"
          value="selected-all"
          v-model="toggleAll"
          @change="toggleAllRows"
        ></b-form-checkbox>
      </template>

      <template #cell(checkbox)="data">
        <b-form-checkbox
          :id="`checkbox-${data.index}`"
          :name="`checkbox-${data.index}`"
          :value="data.index"
          v-model="selected"
        ></b-form-checkbox>
      </template>
    </b-table>
  </div>
</template> 

<script>
export default {
  name: "HaulTable",
  props: ["rows"],
  data() {
    return {
      selected: [],
      fields: ["checkbox", "hu_count", "dimensions", "weight"],
      tableRows: []
    };
  },
  watch: {
    rows() {
      this.tableRows = this.rows.slice(0);
    }
  },
  computed: {
    toggleAll: {
      get() {
        return this.selected.length === this.tableRows.length ? "selected-all" : "";
      },
      set(newValue) {
        return newValue;
      },
    },
  },
  methods: {
    toggleAllRows() {
      const isSelectedAll = this.selected.length === this.tableRows.length;

      if (!isSelectedAll) {
        this.tableRows.forEach((item, index) => {
          const isSelected = this.selected.indexOf(index) > -1;

          if (!isSelected) {
            this.selected.push(index);
          }
        });
      } else {
        this.selected = [];
      }
    },
    deleteSelected() {
      const newItems = this.tableRows.filter((item, index) => {
        if (!this.selected.includes(index)) {
          return item;
        }
      });

      this.tableRows = newItems;
      this.selected = [];
    },
    addNewRow() {
      const newRow = {
        hu_count: "2 Pallet",
        dimensions: "30.0 X 48.0 X 20.0 Inch",
        weight: "7.0 lb",
      };

      this.tableRows.push(newRow);
    },
  },
};
</script>

<style scoped lang="scss">
.haul-table {
  background-color: #ffffff;
  border-radius: 20px;
  overflow: hidden;

  .table {
    margin-bottom: 0;
  }

  &__buttons {
    background-color: #1d1d32;
    text-align: left;
    padding: 10px 10px;
  }
  &__button {
    margin-right: 10px;
  }
}
</style>

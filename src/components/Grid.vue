<template>
  <grid
    :cols="cols"
    :rows="rows"
    :auto-width="autoWidth"
    :pagination="pagination"
    :language="language"
    :height="'340px'"
  ></grid>
</template>


<script>
import Grid from "gridjs-vue";

export default {
  props: ["jsonDataset"],
  components: {
    Grid,
  },
  data() {
    return {
      dropColumns: [
        "href",
        "app_edrpou",
        "cpv_category",
        "edrpou",
        "buyer_code",
        "cpv_description",
        "obj_edrpou_code",
      ],
      cutColumns: ["object_name", "goods_name", "subject"],
      columnsTranslate: {
        application_id: "Реєстраційний номер",
        reg_date: "Дата",
        doc_type: "Тип документу",
        org_name: "Ким виданий",
        object_name: "Назва об'єкту",
        contractor_name: "Підрядник",
        seller_name: "Підрядник",
        goods_name: "Назва робіт/послуг",
        volume_uah: "Вартість, грн",
        prozorro_number: "Ідентифікатор",
        date: "Дата",
        id: "Ідентифікатор",
        documentDate: "Дата",
        amount: "Вартість, грн",
        contractors: "Підрядник",
        subject: "Назва робіт/послуг",
      },
      // autoWidth: false,
      autoWidth: true,
      pagination: {
        enabled: false,
        limit: 4,
        summary: true,
      },
      language: {
        pagination: {
          previous: "Попередня",
          to: "-",
          of: "із",
          next: "Наступна",
          showing: "Показано",
          results: () => "результатів",
        },
        noRecordsFound: "За вашим запитом нічого не знайдено",
      },
    };
  },
  methods: {
    getCols() {
      let raw_cols = Object.keys(this.jsonDataset[0]);

      let filteredCols = raw_cols.filter(
        (col) => !this.dropColumns.includes(col)
      );

      let displayCols = filteredCols.map((col) => {
        return {
          id: col,
          name: this.columnsTranslate[col],
          attributes: (cell) => {
            return {
              "data-cell-content": cell,
              onclick: (event) => this.rowClick(event),
            };
          },
        };
      });

      return displayCols;
    },
    rowClick(event) {
      let idx = Array.prototype.indexOf.call(
        event.target.parentNode.parentNode.querySelectorAll("tr"),
        event.target.parentNode
      );

      let url = this.jsonDataset[idx]["href"];

      window.open(url);
    },
  },
  computed: {
    cols() {
      return this.getCols();
    },
    rows() {
      return this.jsonDataset.map((item) => {
        this.cutColumns.forEach((col) => {
          if (item[col]) {
            item[col] = item[col].split(" ").slice(0, 7).join(" ");
          }
        });

        let filteredRows = Object.fromEntries(
          Object.entries(item).filter(
            (pair) => !this.dropColumns.includes(pair[0])
          )
        );
        let raw_rows = Object.values(filteredRows);

        return raw_rows;
      });
    },
  },
};
</script>


<style lang="scss">
.gridjs-wrapper:last-of-type {
  border-radius: 0px;
  box-shadow: none;
  .gridjs-table {
    font-size: 12px;
    border-radius: 0px;
    table-layout: auto;
    // max-height: 200px;
    // height: 200px;
    th {
      background-color: #e5eef2;
    }
    // .gridjs-tbody {
    //   height: 200px;
    // }
    th.gridjs-th {
      background-color: #e5eef2;
      border: 2px solid transparent;
      color: rgb(0, 0, 0);
    }
    td.gridjs-td {
      border: 2px solid #e5eef2;
      cursor: pointer;
    }
  }
}
</style>
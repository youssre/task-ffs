<template>
  <h2 class="title">Task ffs</h2>
  <table>
    <tbody>
      <tr>
        <th>Date</th>
        <th>WeekDay</th>
        <th>In</th>
        <th>Out</th>
        <th>Lunch</th>
        <th>status</th>
        <th>+/-</th>
        <th>plan</th>
      </tr>
    </tbody>

    <Row
      @changed="updated"
      v-for="item in data"
      :dataSelect="item"
      :key="item"
    />
  </table>
  <p class="total">{{ "total plan : " + planTotal }} </p>
</template>

<script>
import Row from "@/components/Row.vue";
export default {
  data() {
    return {
      footer: 0,
      ArrayUpdated: [],
      planTotal: 0,
      overTotal : 0,
      data: [
        {
          date: "01 / 04",
          day: "sat",
          selectedStatus: "",
          selectedIn: "",
          selectedLunch: "",
          selectedOut: "",
          id: Math.random().toString(36).substring(7),
        },
        {
          date: "02 / 04",
          day: "sun",
          selectedStatus: "",
          selectedIn: "",
          selectedLunch: "",
          selectedOut: "",
          id: Math.random().toString(36).substring(7),
        },
        {
          date: "03 / 04",
          day: "mon",
          selectedStatus: "",
          selectedIn: "",
          selectedLunch: "",
          selectedOut: "",
          id: Math.random().toString(36).substring(7),
        },
      ],
    };
  },
  components: {
    Row,
  },
  methods: {
    updated(footer) {
      this.ArrayUpdated = [
        ...this.ArrayUpdated.filter((item) => item.id !== footer.id),
        footer,
      ];
      this.footer = this.ArrayUpdated;
      this.planTotal = this.ArrayUpdated.reduce((acc, item) => {
        if (
          item.selectedOut == 0 ||
          item.selectedIn == 0
        ) {
          return acc;
        } else {
          let x =
            item.selectedOut > item.selectedIn
              ? item.selectedOut - item.selectedIn 
              : (item.selectedIn % 12) - item.selectedOut + 12   

          console.log(x)
          return acc + x;
        }
      }, 0);
    },
  },
};
</script>


<style >
table,
td,
th {
  border: 1px solid #ddd;
  text-align: left;
}

table {
  border-collapse: collapse;
  width: 100%;
}

th,
td {
  padding: 15px;
}
.title{
  text-align: center;
  padding: 20px 0;
}
.total{
  text-align: center;
  padding: 20px;
}
</style>
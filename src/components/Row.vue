<template>
  <tr>
    <td>{{row.date}}</td>
    <td>{{ row.day }}</td>
    <td>
      <select v-model="row.selectedIn" :disabled="row.selectedStatus === 0">
        <option value="" selected disabled>0:00</option>

        <option v-for="i in 24" :key="i" :value="i">
          {{ i + ":" + "00" }}
        </option>
      </select>
    </td>
    <td>
      <select v-model="row.selectedOut" :disabled="row.selectedStatus === 0">
        <option value="" selected disabled>0:00</option>

        <option v-for="i in 24" :key="i" :value="i">
          {{ i + ":" + "00" }}
        </option>
      </select>
    </td>
    <td>
      <select v-model="row.selectedLunch" :disabled="row.selectedStatus === 0">
        <option value="" selected disabled>chosse</option>

        <option value="30">00:15</option>
        <option value="15">00:30</option>
        <option value="45">00:45</option>
        <option value="1">1:00</option>
      </select>
    </td>
    <td>
      <select v-model="row.selectedStatus">
        <option value="" selected disabled>choose</option>

        <option v-for="item in status" :key="item.v" :value="item.v">
          {{ item.name }}
        </option>
      </select>
    </td>
    <td>
      {{
        row.selectedStatus == 0
          ? row.selectedStatus
          : row.selectedStatus == -1
          ? -1
          : over
      }}
    </td>
    <td>
      {{
        row.selectedStatus == 0
          ? row.selectedStatus
          : row.selectedStatus == -1
          ? -1
          : plan
      }}
    </td>
  </tr>
</template>

<script>
export default {
    name: "RowComp",
  props: ["dataSelect"],
  data() {
    return {
      //days : ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
      row: {},
      footer: 0,
      list: [
        {
          label: "Date",
          value: "date",
        },
      ],
      status: [
        {
          name: "Work from home",
          v: 1,
        },

        {
          name: "Public holiday",
          v: 0,
        },
        {
          name: "Compensation time off",
          v: -1,
        },
      ],
      plan : '',
      over: ''
    };
  },
  watch: {
    row: {
      handler(newValue) {
        // calculate plan
        this.plan  =   newValue.selectedOut > newValue.selectedIn
            ? newValue.selectedOut - newValue.selectedIn 
            : (newValue.selectedIn % 12) - newValue.selectedOut + 12  ;
        
        // calculate over
        if(this.plan > 8){
          this.over = this.plan - 8;
        }else if(this. plan== 8){
          this.over = 0
        }else{
          this.over = this.plan - 8
        }


        if (newValue.selectedOut > newValue.selectedIn) {
          this.$emit("changed", newValue);
        } else {

          this.$emit("changed",newValue);
        }
      },
      deep: true,
    },
    dataSelect: {
      handler(newValue) {
        this.row = { ...newValue };
      },
      deep: true,
      immediate: true,
    },
  },
};
</script>
<template>
  <div class="d-minus">
    {{ formattedTime }}
  </div>
</template>

<script>
import moment from "moment";
import 'moment-precise-range-plugin';

export default {
  name: 'DMinus',
  props: {
    time: Number
  },
  computed: {
    formattedTime() {
      const diff = moment.preciseDiff(moment(this.time), moment(), true);
      const sign = diff.firstDateWasLater ? "-" : "+";
      let letter = null;
      let value = null;
      if (diff.years) {
        letter = "y";
        value = diff.years
      } else if (diff.months) {
        letter = "m";
        value = diff.months
      } else if (diff.days) {
        letter = "d";
        value = diff.days
      } else if (diff.hours) {
        letter = "h";
        value = diff.hours
      } else {
        letter = "o";
        value = 0;
      }
      return `${letter}${sign}${value}`
    }
  }
}
</script>

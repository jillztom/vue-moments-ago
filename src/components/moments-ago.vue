<template>
  <span v-if="date" class="vue-moments-ago" :style="elementStyle" :class="elementClass"
    >{{ prefix }} {{ humanFormatted }} {{ suffix }}</span
  >
</template>

<script>
import Vue from "vue";
import moment from "moment";
Vue.prototype.moment = moment;

export default {
  data() {
    return {
      language: {
        en: {
          few: "a few",
          postfix: "s"
        },
        kr: {
          few: "몇",
          postfix: ""
        },
        jp: {
          few: "何",
          postfix: ""
        },
        he: {
          few: "כמה",
          postfix: "",
        },
        fr: {
          few: "il ya",
          postfix: "",
        }
      },
      epochs: {
        en: ["year", "month", "day", "hour", "minute"],
        kr: ["년", "달", "일", "시간", "분"],
        jp: ["年", "月", "日", "時", "分"],
        he: ["שנה", "חודש", "יום", "שעה", "דקה"],
        fr: ["an", "mois", "jour", "heure", "minute"],
      },
      year: 31536000,
      month: 2592000,
      day: 86400,
      hour: 3600,
      minute: 60,
      humanReadable: "",
      humanDifference: 0,
      humanWord: "moment"
    };
  },

  props: {
    prefix: {
      type: String,
      default: "posted"
    },
    suffix: {
      type: String,
      default: "ago"
    },
    date: {
      type: String,
      required: true
    },
    lang: {
      type: String,
      default: "en"
    },
    elementClass: {
      type: String | Object,
      default: ""
    },
    elementStyle: {
      type: String | Object,
      default: ""
    }
  },

  mounted() {
    this.$nextTick(() => {
      this.getSeconds(this.date);
    });
  },

  computed: {
    humanFormatted() {
      if (this.humanDifference === 0) {
        return (
          this.language[this.lang].few +
          " " +
          this.humanWord +
          this.language[this.lang].postfix
        );
      } else if (this.humanDifference > 1) {
        return (
          this.humanDifference +
          " " +
          this.humanWord +
          this.language[this.lang].postfix
        );
      } else {
        return this.humanDifference + " " + this.humanWord;
      }
    }
  },

  methods: {
    getSeconds(time) {
      let seconds = moment().diff(moment(time), "seconds");
      this.humanReadable = this.getDuration(seconds);
      if (this.humanReadable) {
        this.humanDifference = this.humanReadable.interval;
        this.humanWord = this.humanReadable.humanEpoch;
      }
    },
    getDuration(seconds) {
      let epoch, interval;
      let humanEpoch;
      for (let i = 0; i < this.epochs[this.lang].length; i++) {
        epoch = this.epochs.en[i];
        humanEpoch = this.epochs[this.lang][i];
        interval = Math.floor(seconds / this[epoch]);
        if (interval >= 1) {
          return { interval: interval, humanEpoch: humanEpoch };
        }
      }
    }
  }
};
</script>

<style scoped>
.vue-moments-ago {
  font-size: 12px;
}
</style>

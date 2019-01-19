<template>
  <span
    v-if="date"
    class="vue-moments-ago"
  >{{ prefix }} {{ humanDifference | plural(humanWord, lang) }} {{suffix}}</span>
</template>

<script>
import Vue from 'vue';
import moment from 'moment';
Vue.prototype.moment = moment;

export default {
  data() {
    return {
      epochs: ['year', 'month', 'day', 'hour', 'minute'],
      epochs_nl: ['jaar', 'maand', 'dag', 'uur', 'minuut'],
      epochs_de: ['jahr', 'monat', 'tag', 'stunde', 'minute'],
      epochs_kr: ['년', '달', '일', '시간', '분'],
      epochs_jp: ['年', '月', '日', '時', '分'],
      year: 31536000,
      month: 2592000,
      day: 86400,
      hour: 3600,
      minute: 60,
      humanReadable: '',
      humanDifference: 0,
      humanWord: 'moment'
    };
  },

  props: {
    prefix: {
      type: String,
      default: 'posted'
    },
    suffix: {
      type: String,
      default: 'ago'
    },
    date: {
      type: String,
      required: true
    },
    lang: {
      type: String,
      default: 'en'
    }
  },

  mounted() {
    setInterval(() => {
      this.getSeconds(this.date);
    }, 1000);
  },

  filters: {
    plural(value, name, lang) {
      let plural;
      if (value == 0) {
        let names = '';
        if (lang == 'kr') {
          return '몇' + name;
        } else if (lang == 'jp') {
          return '何' + name;
        } else if (lang == 'nl') {
          if (name == 'moment') {
            names = 'seconden';
          }
          return 'enkele ' + names;
        } else if (lang == 'de') {
          if (name == 'moment') {
            names = 'sekunden';
          }
          return 'einige ' + names;
        } else {
          return 'a few ' + name + 's';
        }
      } else if (value > 1) {
        let names = '';
        if (lang == 'en') {
          return value + ' ' + name + 's';
        } else if (lang == 'nl') {
          var names = '';
          if (name == 'jaar') {
            names = 'jaar';
          }
          if (name == 'maand') {
            names = 'maanden';
          }
          if (name == 'dag') {
            names = 'dagen';
          }
          if (name == 'uur') {
            names = 'uur';
          }
          if (name == 'minuut') {
            names = 'minuten';
          }
          if (name == 'moment') {
            names = 'seconden';
          }
          return value + ' ' + names;
        } else if (lang == 'de') {
          if (name == 'jahr') {
            names = 'jahr';
          }
          if (name == 'monat') {
            names = 'monaten';
          }
          if (name == 'tag') {
            names = 'tagen';
          }
          if (name == 'stunde') {
            names = 'stunden';
          }
          if (name == 'minute') {
            names = 'minuten';
          }
          if (name == 'moment') {
            names = 'sekunden';
          }
          return value + ' ' + names;
        } else {
          return value + ' ' + name + '';
        }
      } else {
        return value + ' ' + name;
      }
    }
  },

  methods: {
    getSeconds(time) {
      let seconds = moment().diff(moment(time), 'seconds');
      this.humanReadable = this.getDuration(seconds);
      if (this.humanReadable) {
        this.humanDifference = this.humanReadable.interval;
        this.humanWord = this.humanReadable.humanEpoch;
      }
    },
    getDuration(seconds) {
      let epoch, interval;
      let humanEpoch;
      for (let i = 0; i < this.epochs.length; i++) {
        epoch = this.epochs[i];
        if (this.lang == 'kr') {
          humanEpoch = this.epochs_kr[i];
        } else if (this.lang == 'jp') {
          humanEpoch = this.epochs_jp[i];
        } else if (this.lang == 'nl') {
          humanEpoch = this.epochs_nl[i];
        } else if (this.lang == 'de') {
          humanEpoch = this.epochs_de[i];
        } else {
          humanEpoch = this.epochs[i];
        }
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

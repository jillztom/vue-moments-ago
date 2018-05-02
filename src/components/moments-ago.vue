<template>
  <div v-if="date">
    <span class="vue-moments-ago">{{ prefix }} {{ humanDifference | plural(humanWord) }} {{suffix}}</span>
  </div>
</template>

<script>
export default {
  data () {
   return {
    epochs: ['year', 'month', 'day', 'hour', 'minute'],
    year: 31536000,
    month: 2592000,
    day: 86400,
    hour: 3600,
    minute: 60,
    humanReadable: '',
    humanDifference: 0,
    humanWord: 'moment',
   }
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
    }
  },
  
  mounted(){
    setInterval(()=>{
      this.getSeconds(this.date);
    }, 1000);
  },
  
  filters: {
    plural(value, name){
      if(value===0){
        return 'a few '+name+'s'
      }
      else if(value > 1){
        return value+' '+name+'s'
      }
      else{
        return  value+' '+name;
      }
    }
  },

  methods: {
    getSeconds(time){
      let seconds = moment().diff(moment(time), 'seconds');
      this.humanReadable =  this.getDuration(seconds);
      if(this.humanReadable){
        this.humanDifference = this.humanReadable.interval;
        this.humanWord = this.humanReadable.epoch;
      }
    },
    getDuration(seconds){
      let epoch, interval;
      for (let i = 0; i < this.epochs.length; i++) {
        epoch = this.epochs[i];
        interval = Math.floor(seconds / this[epoch]);
        if (interval >= 1) {
          return {interval: interval, epoch: epoch};
        }
      }
    }
  }
}

</script>

<style scoped>
  .vue-moments-ago{
    font-size: 12px;
  }
</style>
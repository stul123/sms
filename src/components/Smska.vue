<script>

export default {
  methods: {
   
  },
  props: {
    sms: {
      type: Object,
      required: true
    }
  }
} 
</script>

<template>
    <div v-show="sms != [] && sms.ime != ''" className="smska" >
        <div className="top">
          <p className="name">{{ sms.ime }}</p>
          <p className="date">{{ sms.date }}</p>
        </div>
        <div className="sms">
          <img v-if="sms.smska.slice(0,3) == 'api'" :src="'https://smski.site/' + sms.smska" alt="">
          <audio v-else-if="sms.smska.slice(0,3) == 'gsa'" controls :src="'https://smski.site/' + sms.smska.slice(2)"></audio>
          <p v-else class="text">{{ sms.smska }}</p>
        </div>
    </div>
</template>

<style scoped> 
  .smska {
    padding: 15px;
    border-radius: 15px;
    background: #232222;
    margin-bottom: 10px;
  }

  p {
    margin: 0;
  }

  .sms img {
    width: 200px;
  }

  .name {
    font-size: 19px;
    font-weight: 600;
  }

  .date {
    font-size: 12px;
  }

  .top {
    display: flex;
    width: 100%;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 5px;
  }

  audio {
    width: 100%;
    height: 30px;
  }
</style>

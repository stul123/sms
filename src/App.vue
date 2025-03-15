<script>
import Smska from './components/Smska.vue';
import axios from 'axios';

export default {
  components: {
    Smska
  },
  data() {
    return {
      smski: [],
      newSms: {
        date: '',
        ime: localStorage.name || 'Anon' ,
        smska: '',
      },
      date: new Date(),
      showName: null
    }
  },
  computed: {
    smsDate() {
      return `${this.date.getHours().toString().padStart(2, '0')}:${this.date.getMinutes().toString().padStart(2, '0')} ${this.date.getDate().toString().padStart(2, '0')}.${(this.date.getMonth() + 1).toString().padStart(2, '0')}.${this.date.getFullYear()}`;
    },
  },
  methods: {
    getSmski() {
      axios.get(`https://smski.site/api/getsms.php`)
        .then(res => {
          this.smski = res.data
        }) 
    },
    async addSms() {
      if (this.newSms.smska.length > 0) {
        this.newSms.date = this.smsDate;
        let formBody = `smska=${encodeURIComponent(this.newSms.smska)}&name=${encodeURIComponent(this.newSms.ime)}&date=${encodeURIComponent(this.newSms.date)}`;
        localStorage.name = this.newSms.ime
        let saveSmska = this.newSms.smska
        this.smski.push(this.newSms)
        try {
          await axios.post('https://smski.site/api/addsms.php', formBody)
            .then(res => {
              if (res.data == '') {
                this.newSms.smska = ''
                this.smski[this.smski.length-1].smska = saveSmska
              }
            }) 
        } catch (error) {
          console.error('Ошибка при отправке:', error);
        }
      } else {
        this.error = 'Надо смску зполнить';
      }
    },
    hideName() {
      this.showName = true
      console.log(this.showName)
    },
    saveName() {
      localStorage.name = this.newSms.ime || "Anon";
    }
  },
  mounted() {
    this.getSmski();  
  },
}
</script>

<template>
  <div className="wrapper">
    <h2>smski + vue</h2>   
    <div className="smski_block">
      <Smska v-for="sms in smski.slice().reverse()" :sms="sms"/>
    </div>
    <div v-if="showName != true" className="ime_block">
      <input @change="saveName()" type="text" v-model="newSms.ime" className="ime" placeholder="Имя в нике">
      <button @click="showName = true"><svg viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M12.81 4.36l-1.77 1.78a4 4 0 0 0-4.9 4.9l-2.76 2.75C2.06 12.79.96 11.49.2 10a11 11 0 0 1 12.6-5.64zm3.8 1.85c1.33 1 2.43 2.3 3.2 3.79a11 11 0 0 1-12.62 5.64l1.77-1.78a4 4 0 0 0 4.9-4.9l2.76-2.75zm-.25-3.99l1.42 1.42L3.64 17.78l-1.42-1.42L16.36 2.22z"/></svg></button>
    </div>
    <form @submit.prevent="addSms()">
      <input type="text" v-model="newSms.smska" placeholder="Смска...">
      <button type="submit">Запостить</button>
    </form>
  </div>
</template>

<style scoped>
  .ime_block {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
    gap: 10px;
  }

  .ime_block button {
    padding: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    outline: none;
  }

  .ime_block button svg {
    width: 28px;
    fill: #fff;
  }

  .ime {
    padding: 7.5px 5px;
    width: 100%;
    font-size: 20px;
    border-radius: 8px;
    border: 1px solid #000;
    outline: none;
  }
  .error {
    color: rgb(150, 3, 3);
  }
  .wrapper {
    background: #370254;
    padding: 30px;
    width: 50vw;
    height: auto;
    max-height: 60vh;
    border-radius: 10px;
  }

  .smski_block {
    max-height: 40vh;
    overflow: auto;
    margin-bottom: 10px;
  }
  
  .wrapper h2,
  .wrapper p { 
    text-align: center;
  }

  

  form {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  form input {
    padding: 7.5px 5px;
    width: 100%;
    font-size: 20px;
    border-radius: 8px;
    border: 1px solid #000;
    outline: none;
  }

  h2 {
    line-height: 100%;
    margin: 0 0 20px 0;
  }
</style>

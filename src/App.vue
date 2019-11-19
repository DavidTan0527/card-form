<template>
  <div id="app">
    <div class="card-form">
      <div class="card">
        <img :src="require(`./assets/card-templates/${card_type}.png`)" class="card-image">
        <div class="card-display-number" :class="{
          'type-3': card_type == 3,
          'type-6': card_type == 6
        }">
          {{ card_display_number }}
        </div>
        <div class="card-display-expire" :class="{
          'type-3': card_type == 3,
          'type-6': card_type == 6
        }">
          <div class="expire-title">
            VALID THRU
          </div>
          {{ card_display_expire }}
        </div>
        <div class="card-display-name" :class="{
          'type-3': card_type == 3,
          'type-6': card_type == 6
        }">
          {{ card_display_name || 'HOLDER\'S NAME' }}
        </div>
      </div>
      <div class="card-form-inputs">
        <div class="form-section text-section">
          <input id="card-number" class="form-input card-number" name="card_number" v-model="card_number">
          <label class="form-label" for="card-number"
          :class="card_number ? 'active': ''">Card No.</label>
        </div>
        <div class="form-section text-section">
          <input id="card-holder" class="form-input card-holder" name="card_holder" v-model="card_holder">
          <label class="form-label" for="card-holder"
          :class="card_holder ? 'active': ''">Holder Name</label>
        </div>
        <div class="form-section select-section">
          <div class="form-select">
            <select id="card-month" class="form-input card-month" name="card_number" v-model="expire_month">
              <option :value="null" disabled selected>Month</option>
              <option :value="1" class="month">Jan</option>
              <option :value="2" class="month">Feb</option>
              <option :value="3" class="month">Mar</option>
              <option :value="4" class="month">Apr</option>
              <option :value="5" class="month">May</option>
              <option :value="6" class="month">Jun</option>
              <option :value="7" class="month">Jul</option>
              <option :value="8" class="month">Aug</option>
              <option :value="9" class="month">Sep</option>
              <option :value="10" class="month">Oct</option>
              <option :value="11" class="month">Nov</option>
              <option :value="12" class="month">Dec</option>
            </select>
            <select id="card-month" class="form-input card-month" name="card_number" v-model="expire_year">
              <option :value="null" disabled selected>Year</option>
              <option :value="current_year+ind-1" v-for="ind in 11" :key="ind">
                {{current_year + ind - 1}}
              </option>
            </select>
          </div>
          <label class="form-label" for="card-month">Expiry Date</label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data: () => ({
    card_number: null,
    expire_month: null,
    expire_year: null,
    cvc: null,
    card_holder: '',
    current_year: null
  }),
  mounted() {
    this.current_year = (new Date).getFullYear();
  },
  computed: {
    card_type() {
      if (this.card_number) return parseInt(this.card_number.toString()[0])
      return 5;
    },
    card_display_number() {
      let str = (this.card_number || '').toString();
      let out = ''; let acc = 0;
      for (let i = 0; i < str.length; i++) {
        if (acc != 0 && acc%4 == 0) out = out + ' ';
        out = out + str[i];
        acc++;
      }
      for (let i = 0; i < 16-str.length; i++) {
        if (acc != 0 && acc%4 == 0) out = out + ' ';
        out = out + 'X';
        acc++;
      }
      return out;
    },
    card_display_expire() {
      let month = 'MM';
      let year = 'YY';
      if (this.expire_month) {
        month = this.expire_month;
        if (this.expire_month.toString().length < 2) month = '0'+month;
      }
      if (this.expire_year) {
        year = this.expire_year.toString().substr(this.expire_year.toString().length-2, 2);
      }
      return  `${month}/${year}`
    },
    card_display_name() {
      return this.card_holder.toUpperCase();
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>

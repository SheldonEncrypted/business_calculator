<template>
  <div class="home">
    
    <div class="container">
      <div class="title">Tax Calculator</div>
      <div class="toggles">
        <RadioButton 
          title="Self-Employed" 
          groupName="employment"
          value="self-employeed"
          @input="employmentStatus=$event"
        />
        <RadioButton 
          title="Employeed" 
          groupName="employment" 
          value="employeed"
          @input="employmentStatus=$event"
          checked
        />
      </div>

      <div class="content">
        <div class="inputs">
          <div class="inputs_row">
            <span> 
              <label for=""> {{ employmentStatus != 'employeed' ? 'Gross Profit :' : 'Gross Salary :' }} </label>  
            </span>
            <span>
              $<input type="text" id="salary" v-model="salary">
            </span>
          </div>

          <div class="inputs_row" v-if="employmentStatus != 'employeed'">
            <span>
              <label for="expense">Total Expense :</label>
            </span>
            <span>
              $<input type="text" id="expense" v-model="expense">
            </span>
          </div>

          <div class="inputs_row">
            <span>Total Tax: </span>
            <span>{{toMoney(totalTax)}}</span>
          </div>

          <div class="inputs_row" v-show="employmentStatus == 'employeed'">
            <span>Taxed Salary : </span>
            <span>{{toMoney(salary - totalTax)}}</span>
          </div>
        </div>

        <div class="cards">
          <div class="cards_row">
            <Card
              title="PAYE"
              :salary="salary"
              :percent="25/100"
              @calculated="payeTotal = $event"
            />
            
            <Card
              title="NHT"
              :salary="salary"
              :percent="nhtPercent"
              @calculated="nhtTotal = $event"
            />
          </div>

          <div class="cards_row">
            <Card
              title="NIS"
              :salary="salary"
              :percent="nisPercent"
              @calculated="nisTotal = $event"
            />

            
            <Card
              title="EdTax"
              :salary="salary"
              :percent="edTaxPercent"
              :nisTotal="nisTotal"
              :isEdTax="true"
              @calculated="edTaxTotal = $event"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Card from '../components/Card.vue'
import RadioButton from '../components/RadioButton.vue'

export default {
  name: 'Home',

  components: { Card, RadioButton },

  data(){
    return {
      employmentStatus: 'employeed',
      salary: 0,
      expense: 0,

      nisTotal: 0,
      nhtTotal: 0,
      payeTotal: 0,
      edTaxTotal: 0,
    }
  },

  computed: {
    totalTax() {
      const ic = this.salary < 125000 ? 0 : this.payeTotal
      return this.nisTotal + this.nhtTotal + this.edTaxTotal + ic;
    },

    nisPercent(){ return this.employmentStatus == 'employeed' ? (3.5/100) : (5/100) },
    nhtPercent() { return this.employmentStatus == 'employeed' ? (2/100) : (3/100) },
    edTaxPercent() { return this.employmentStatus == 'employeed' ? (2.25/100) : (3.75/100)},
  },

  methods: {
    toMoney(value) {
      const formatter = new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD'
      })
      return formatter.format(Number(value) ? value : 0);
    }
  },
}
</script>

<style scoped>
  .home {
    font-family: Poppins, sans-serif;
    width: 100wh;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .container {
    background: #FFFFFF;
    box-shadow: 0px 0px 10px rgba(77, 65, 8, 0.2);
    border-radius: 2px;
  }

  .title {
    font-size: 37.9px;
    text-align: center;
    padding: 16px 0px;
  }

  .toggles {
     display: flex;
     gap: 24px;
     justify-content: center;
     width: 100%;
    padding-bottom: 64px;
  }


  .content {
    display: flex;
    flex-direction: row;
    gap: 64px;
    padding: 0 32px;
    padding-bottom: 64px;
  }

  .cards {
    display: flex;
    flex-direction: column;
    gap: 64px;
  }

  .cards_row {
    display: flex;
    gap: 50px;
    padding-right: 16px;
  }

  .inputs {
    display: flex;
    flex-direction: column;
    gap: 24px;
    
    font-size: 21.3px;

    height: 160px;
  }

  .inputs_row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    column-gap: 32px;
    text-align: right;
  }
  
  .inputs_row > span > input {
    width:200px;
    font-size: 21.3px;
    text-align: right;

    border: none;
    border-bottom: 1px solid #ccc;
    outline: none;
  }
</style>

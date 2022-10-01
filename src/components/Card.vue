<template>
  <div class="card">
    <div class="card__body">
      <div class="title"> {{title}} </div>

      <div class="row">
        <span>Salary: </span> <span> {{toMoney(salary)}} </span>
      </div>

      <template v-if="isEdTax">
        <div class="row" :class=" isEdTax && salary < threshold ? 'total' : '' ">
          <span>Less Nis: </span> <span> {{toMoney(nisTotal)}} </span>
        </div>
      </template>

      <template v-if="salary > threshold">
        <div class="row total">
          <span>Less Threshold: </span> <span> {{toMoney(threshold)}} </span>
        </div>
      </template>

      <div class="row" v-if="salary > threshold || isEdTax">
        <span></span> <span> {{toMoney(totalDeductions)}} </span>
      </div>

      <div class="row total">
        <span>Percentage: </span> <span> {{+(percent*100).toFixed(2).toString()}}% </span>
      </div>

      <div class="row">
        <span>Total: </span><span> {{toMoney(totalTax)}} </span>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'Card',

  props: {
    title: {
      type: String,
      default: ''
    },

    salary: {
      type: Number,
      default: 0
    },

    percent: {
      type: Number,
      default: 0,
    },

    threshold: {
      type: Number,
      default: 125000
    },

    isEdTax: {
      type: Boolean,
      default: false,
    },

    nisTotal: {
      type:  Number,
      default: 0
    }
  },

  computed: {
    totalDeductions() {
      const hasThreshold = this.salary > this.threshold;
      return hasThreshold ? (this.salary - this.threshold - this.nisTotal) : (this.salary - this.nisTotal)
    },  

    totalTax() {
      return  this.totalDeductions * this.percent
    }
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

  watch: {
    totalTax(newVal, oldVal) {
      if (newVal != oldVal) {
        this.$emit('calculated', this.totalTax)
      }
    }
  }
}
</script>

<style scoped>
  .card {
    font-family: Poppins, sans-serif;
    font-size: 16px;

    background-color: #F2CC0C;
    color: #4D4626;
    width: 280px;
    height: 270px;
    margin-right: calc(335px - 284px);
  }

  .card__body {
    position: relative;
    top: 10px;
    left: 10px;

    display: flex;
    flex-direction: column;
    gap: 16px;

    background-color: #F2EAC2;
    width: 300px;
    height: 300px;
    padding: 24px;

    box-shadow: 3px 5px 5px 2px rgba(77, 65, 8, .2);
  }

  .title {
    width: 100%;
    font-weight: 700;
    text-align: center;
  }

  .row {
    display: flex;
    justify-content: space-between;
  }

  .total {
    border-bottom: 1px solid black;
  }
</style>
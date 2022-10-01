<template>
  <div>
    <label :for="title" class="button">
      <input 
        type="radio" 
        :id="title" 
        :name="groupName" 
        :value="value"
        :checked="checked"
        @change="$emit('input', $event.target.value)"
      >
      {{title}} 
    </label>
  </div>
</template>

<script>
export default {
  name: 'RadioButton',
  
  props: {
    title: {
      type: String,
      default: ''
    },

    value: {
      type: String,
      default: '',
    },

    groupName: {
      type: String,
      default: ''
    },

    checked: {
      type: Boolean,
      default: false
    }
  },
}
</script>

<style scoped>
  .button {
    display: flex;
    gap: 8px;
    justify-content: center;
    align-items: center;
    color: #4D4626;
    border: 3px solid #4D4626;

    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
  } 

  input[type="radio"] {
    appearance: none;
    position: relative;
    border: 3px solid #4D4626;
    width: 1.5em;
    height: 1.5em;
    border-radius: 50%;
    cursor: pointer;
  }

  input[type="radio"]::before {
    content: '';
    position: absolute;

    width: .9em;
    height: .9em;

    border-radius: 50%;
    background-color: #4D4626;

    transform: scale(0);
  }

  input[type="radio"]:checked::before {
    transform: scale(1);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  
  .button:has( input[type="radio"]:checked ) {
    background-color: #F2EAC2;
  }
</style>
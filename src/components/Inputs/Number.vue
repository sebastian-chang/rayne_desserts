<template>
  <div :class="`rayne-${this.$store.state.theme}`">
    <div class="group">
      <input
        type="number"
        :id="name"
        class="form_field"
        :class="errors[name] ? 'invalid' : ''"
        :placeholder="inputPlaceholder"
        min="0"
        v-model="inputText"
        @input="$emit('update:modelValue', $event.target.value)"
        @keyup="keyupValidate"
        @keypress="numberValidation($event)"
        @blur="validateInput"
        autocomplete="off"
        required
      />
      <span class="highlight"></span>
      <span class="bar"></span>
      <label>{{ inputLabel }}</label>
    </div>
    <div class="error_label" v-if="errors[name]">
      {{ errors[name] }}
    </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import userFormValidation from '../../modules/userFormValidation'
export default {
  props: ['name', 'required', 'label'],
  setup (props) {
    const inputText = ref('')
    const inputLabel = ref(props.label)
    const inputPlaceholder = ref('100')
    const touched = ref(false)

    const numberRegex = /^[0-9\s]*$/;

    const { errors, validateNameField } = userFormValidation()

    const keyupValidate = () => {
      if (touched.value) {
        validateInput()
      }
    }

    const validateInput = () => {
      if (!touched.value) {
        touched.value = true
      }
      validateNameField(props.name, inputText.value)
    }

    const numberValidation = event => {
      if (numberRegex.test(event.key)) {
        return true
      }
      else {
        event.preventDefault()
        return false
      }
    }

    return { inputText, inputLabel, inputPlaceholder, validateInput, errors, numberValidation, keyupValidate }
  }
}
</script>

<style>
</style>

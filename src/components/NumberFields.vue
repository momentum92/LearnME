<template>
  <v-container fill-height fluid>
    <v-row align="center" justify="center">
      <v-col cols="2" sm="2" :key="index" v-for="(number, index) in numbers">
        <v-text-field class="centered-input"
                      ref="inputField"
                      :rules=rules.validate(number.value)
                      :placeholder="getPlaceholder(number)"
                      oninput="if(this.value.length > 1) this.value = this.value[this.value.length-1]"
                      @input="moveToNextField(number.value, index)"/>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'NumberFields',
  props: {
    numbers: {
      required: true,
      type: Array
    },
    reveal: {
      required: true,
      type: Boolean
    }
  },
  data: () => ({
    rules: {
      validate(val) {
        return [
          v => !!v || 'Required',
          v => Number(v) === Number(val) || 'Incorrect value'
        ];
      },
    }
  }),
  methods: {
    moveToNextField(correctVal, index) {
      const nextIndex = index + 1;
      const value = this.$refs.inputField[index].internalValue

      if (value && Number(value) === correctVal && nextIndex < this.numbers.length) {
        this.$refs.inputField[nextIndex].focus()
      }
    },
    getPlaceholder(number) {
      return (!number.hidden || this.reveal) ? `${number.value}` : '?';
    },
  },
}
</script>

<style scoped>
.centered-input >>> input {
  text-align: center
}
</style>

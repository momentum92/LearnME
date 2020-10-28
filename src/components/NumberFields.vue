<template>
    <v-row align="center" justify="center">
        <template v-for="(number, index) in numbers">
          <v-col cols="12" sm="1" md="1" lg="1" :key="index">
            <v-text-field class="centered-input"
                          :rules=rules.validate(number.value)
                          :placeholder="getPlaceholder(number)"
                          oninput="if(this.value.length > 1) this.value = this.value[this.value.length-1]"/>
          </v-col>
        </template>
    </v-row>
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

<template>
  <v-form ref="form" v-model="valid">
    <v-container>
      <v-row align="center" justify="center">
        Round: {{ round }}
      </v-row>
      <v-row v-if="setNumbers" align="center" justify="center">
        <template>
          <v-col cols="12">
            <v-text-field ref="setField" class="centered-input" v-model="numbersField" type="number" />
          </v-col>
        </template>
      </v-row>
      <NumberFields v-else :numbers="numbers" :reveal="reveal" />
      <v-row align="center" justify="center">
        <v-btn elevation="2" medium outlined raised @click="reset">
          {{ (setNumbers) ? 'SET' : 'RESET' }}
        </v-btn>
        <v-btn v-if="!setNumbers" elevation="2" medium outlined raised @click="toggleReveal">
          REVEAL
        </v-btn>
        <v-btn v-if="!setNumbers" elevation="2" medium outlined raised @click="validate">
          SUBMIT
        </v-btn>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
  import NumberFields from "./NumberFields";

  export default {
    name: 'Layout',
    components: {
      NumberFields,
    },
    data: () => ({
      valid: false,
      round: 1,
      setNumbers: true,
      numbersField: '123',
      numbers: [
        { value: 1, hidden: false },
        { value: 2, hidden: false },
        { value: 3, hidden: false }
      ],
      reveal: false,
    }),
    methods: {
      validate () {
        this.$refs.form.validate();
        if (this.valid === true) {
          this.valid = false;
          this.$refs.form.reset()
          this.$refs.form.resetValidation()
          this.reveal = false;
          this.round++;
          this.setVisibility();
        }
      },
      setVisibility() {
        this.resetVisibility();
        let maxNum = this.numbers.length - Math.floor(this.numbers.length / this.round);
        let length = this.numbers.length;
        let selected = new Array(length);

        while (maxNum > 0) {
          const pos = Math.floor(Math.random() * length);
          this.numbers[pos in selected ? selected[pos] : pos].hidden = true;
          selected[pos] = --length in selected ? selected[length] : length;
          maxNum--;
        }
      },
      resetVisibility() {
        this.numbers.map(item => {
          item.hidden = false;
        });
      },
      toggleReveal() {
        this.$refs.form.resetValidation()
        this.reveal = !this.reveal;
      },
      reset() {
        this.setNumbers = !this.setNumbers;

        if (!this.setNumbers) {
          this.numbers = [...(this.numbersField
              .split('')
              .map(number => {
                return {
                  value: Number(number),
                  hidden: false
                }
              }))];
        } else {
          this.numbersField = this.numbers.map(number => number.value).join('').toString();
        }

        this.round = 1;
        this.valid = false;
        this.$refs.form.reset()
        this.$refs.form.resetValidation()
        this.reveal = false;
        this.setVisibility();
      }
    },
    beforeMount() {
      this.valid = false;
      this.setVisibility();
    }
  }
</script>

<style scoped>
  .centered-input >>> input {
    text-align: center
  }
</style>

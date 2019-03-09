<template>
  <v-text-field
    type="tel"
    :value="formattedValue"
    @input="change"
    v-money="{precision, decimal, thousands, prefix}"
    class="v-money"
    :suffix="suffix"
  />
</template>
<script>
// import Vue from "vue";
import money from "./directive";
import defaults from "./options";
import { format, unformat } from "./utils";
// import vuetify from "vuetify";

// Vue.use(vuetify);

export default {
  name: "Money",
  props: {
    suffix: null,
    label: null,
    value: {
      required: true,
      type: [Number, String],
      default: 0
    },
    masked: {
      type: Boolean,
      default: false
    },
    precision: {
      type: Number,
      default: () => defaults.precision
    },
    decimal: {
      type: String,
      default: () => defaults.decimal
    },
    thousands: {
      type: String,
      default: () => defaults.thousands
    },
    prefix: {
      type: String,
      default: () => defaults.prefix
    }
  },

  directives: { money },

  data() {
    return {
      formattedValue: ""
    };
  },

  watch: {
    value: {
      immediate: true,
      handler(newValue, oldValue) {
        var formatted = format(newValue, this.$props);
        if (formatted !== this.formattedValue) {
          this.formattedValue = formatted;
        }
      }
    }
  },

  methods: {
    change(value) {
      this.$emit(
        "input",
        this.masked ? value : unformat(value, this.precision)
      );
    }
  }
};
</script>



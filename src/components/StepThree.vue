<template>
  <div>
    <div class="input-group">
      <label for="message">Message</label>
      <textarea
        id="message"
        v-model="formData.message"
        placeholder="Message"
        autofocus="true"
      ></textarea>
      <p v-if="validationErrors.message" class="error">
        {{ validationErrors.message }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    /**
     * { name, age, email, message }
     */
    modelValue: {
      type: Object,
      required: true,
    },
    /**
     * { name, age, email, message }
     */
    validationErrors: {
      type: Object,
      required: true,
    },
  },
  computed: {
    formData: {
      get() {
        return this.modelValue;
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
};
</script>

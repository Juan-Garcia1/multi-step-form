<template>
  <div>
    <div class="input-group">
      <label for="email">Email</label>
      <input
        id="email"
        type="email"
        v-model="formData.email"
        placeholder="Email"
        autofocus
      />
      <p v-if="validationErrors.email" class="error">
        {{ validationErrors.email }}
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

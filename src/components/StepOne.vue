<template>
  <div>
    <div class="input-group">
      <label for="name">Name</label>
      <input
        id="name"
        type="text"
        v-model="formData.name"
        placeholder="John Smith"
        autofocus
        @input="filterNameInput"
      />
      <p v-if="validationErrors.name" class="error">
        {{ validationErrors.name }}
      </p>
    </div>
    <div class="input-group">
      <label for="age">Age</label>
      <input
        id="age"
        type="number"
        v-model="formData.age"
        placeholder="45"
        min="18"
        max="120"
      />
      <p v-if="validationErrors.age" class="error">
        {{ validationErrors.age }}
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
  methods: {
    filterNameInput() {
      // Regular expression to allow letters, hyphens, apostrophes, and spaces
      const validPattern = /^[A-Za-z'-\s]*$/;
      // Filter out invalid characters
      this.formData.name = this.formData.name
        .split("")
        .filter((char) => validPattern.test(char))
        .join("");
    },
  },
};
</script>

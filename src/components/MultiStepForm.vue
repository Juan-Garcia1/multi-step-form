<template>
  <form @submit.prevent="nextStep">
    <ul class="step-list">
      <li
        v-for="(step, index) in stepTitles"
        :key="index"
        :class="{ active: currentStep === index }"
      >
        {{ index + 1 }}. {{ step.title }}
      </li>
    </ul>

    <!-- Step Content -->
    <h2>{{ stepTitles[currentStep].title }}</h2>
    <component
      :is="steps[currentStep]"
      v-model="formData"
      v-model:validationErrors="validationErrors"
    />

    <!-- Navigation Buttons -->
    <div class="step-list__navigation">
      <button type="button" @click="prevStep" v-if="currentStep !== 0"
        >Previous</button
      >
      <button type="submit">{{ isLastStep ? "Submit" : "Next" }}</button>
    </div>
  </form>
</template>

<script>
import StepOne from "./StepOne.vue";
import StepTwo from "./StepTwo.vue";
import StepThree from "./StepThree.vue";

export default {
  components: {
    StepOne,
    StepTwo,
    StepThree,
  },
  data() {
    return {
      currentStep: 0,
      formData: {
        name: "",
        age: "",
        email: "",
        message: "",
      },
      validationErrors: {},
      steps: ["StepOne", "StepTwo", "StepThree"],
      stepTitles: [
        { title: "Personal Information" },
        { title: "Contact Information" },
        { title: "Message" },
      ],
    };
  },
  computed: {
    isLastStep() {
      return this.currentStep === this.steps.length - 1;
    },
  },
  methods: {
    nextStep() {
      if (this.validateStep()) {
        if (this.isLastStep) {
          this.handleSubmit();
        } else {
          this.currentStep++;
        }
      }
    },
    prevStep() {
      if (this.currentStep > 0) {
        this.currentStep--;
        this.validationErrors = {};
      }
    },
    validateStep() {
      this.validationErrors = {}; // Clear previous errors
      const step = this.currentStep;

      if (step === 0) {
        if (!this.formData.name) {
          this.validationErrors.name = "Name is required";
        }
        if (!this.formData.age) {
          this.validationErrors.age = "Age is required";
        }
      }

      if (step === 1) {
        if (!this.formData.email) {
          this.validationErrors.email = "Email is required";
        }
      }

      if (step === 2) {
        if (!this.formData.message) {
          this.validationErrors.message = "Message is required.";
        }
      }

      return Object.keys(this.validationErrors).length === 0;
    },
    handleSubmit() {
      console.log("Form Data:", this.formData);
      alert("Form submitted successfully!");
    },
  },
};
</script>

<style>
.step-list {
  list-style-type: none;
  padding: 0;
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.step-list li {
  padding: 10px 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
}

.step-list li.active {
  background-color: #4caf50;
  color: white;
  font-weight: bold;
}

.input-group:not(:last-child) {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 8px;
  font-weight: 700;
}

input,
textarea {
  width: 100%;
  padding: 12px 20px 12px 12px;
  border: solid 1px lightgray;
  border-radius: 5px;
  font-size: 16px;
  box-sizing: border-box;
}

.error {
  color: red;
  margin-top: 10px;
}

.step-list__navigation {
  margin-top: 15px;
  display: flex;
  justify-content: center;
  column-gap: 10px;
}
</style>

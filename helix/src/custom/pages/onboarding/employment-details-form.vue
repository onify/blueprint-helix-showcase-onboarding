<template>
  <v-form>
    <v-col lg="6">
      <h-page-title>Employment details</h-page-title>
      <h-page-description class="text-subtitle-1 pb-5">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      </h-page-description>
    </v-col>
    <v-row>
      <v-col>
        <v-text-field v-model="formData.jobTitle" label="Job title" :rules="[hValidate.required]"></v-text-field>
      </v-col>
      <v-col></v-col>
    </v-row>
    <v-row>
      <v-col>
        <div>Employment type</div>
        <v-radio-group v-model="formData.permanentEmployment" :rules="[hValidate.is_not([null])]">
          <v-radio label="Permanent employment" value="permanent"></v-radio>
          <v-radio label="Fixed-term employment" value="fixed"></v-radio>
        </v-radio-group>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-text-field v-model="formData.employmentStartDate" type="date" label="Start date" :rules="[hValidate.required]"></v-text-field>
      </v-col>
      <v-col>
        <v-text-field
          v-if="formData.permanentEmployment === 'fixed'"
          v-model="formData.employmentEndDate"
          type="date"
          :min="formData?.employmentStartDate"
          label="End date"
          :rules="[hValidate.required]"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <div>Working hours</div>
        <v-radio-group v-model="formData.fullTime" :rules="[hValidate.is_not([null])]">
          <v-radio label="Full-time" :value="true"></v-radio>
          <v-radio label="Part-time" :value="false"></v-radio>
        </v-radio-group>
      </v-col>
      <v-col>
        <v-text-field
          v-if="formData.fullTime === false"
          v-model="formData.workHours"
          label="Work hours per week"
          :rules="[hValidate.required, hValidate.between([5, 40])]"
          type="number"
        ></v-text-field>
      </v-col>
    </v-row>
  </v-form>
</template>

<script setup>
const props = defineProps({
  formData: Object,
});
</script>

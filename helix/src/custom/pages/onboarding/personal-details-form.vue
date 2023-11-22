<template>
  <v-form>
    <v-col lg="6">
      <h-page-title>Personal details</h-page-title>
      <h-page-description class="text-subtitle-1 pb-5">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      </h-page-description>
    </v-col>
    <v-row>
      <v-col>
        <v-text-field v-model="formData.firstName" label="First name" :rules="[hValidate.required]"></v-text-field>
      </v-col>
      <v-col>
        <v-text-field v-model="formData.lastName" label="Last name" :rules="[hValidate.required]"></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-text-field
          v-model="formData.email"
          persistent-hint
          hint="Personal email address"
          label="Email address"
          :rules="[hValidate.required, hValidate.email]"
        ></v-text-field>
      </v-col>
      <v-col>
        <v-text-field
          v-model="formData.confirmEmail"
          persistent-hint
          hint="Confirm your personal email address"
          label="Confirm email address"
          :rules="[hValidate.required, hValidate.email, hValidate.confirmed({ target: formData.email })]"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-text-field
          v-model="formData.personalNumber"
          label="Personal number"
          :rules="[hValidate.required, hValidate.regex(['^(19|20)?[0-9]{6}[- ]?[0-9]{4}$'])]"
        ></v-text-field>
      </v-col>
      <v-col>
        <v-text-field
          v-model="formData.mobileNumber"
          persistent-hint
          hint="Personal mobile phone number"
          label="Mobile phone"
          :rules="[hValidate.required, hValidate.regex(['^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$'])]"
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

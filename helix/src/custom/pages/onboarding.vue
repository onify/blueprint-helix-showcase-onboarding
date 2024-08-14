<template>
  <h-form-container page-title="Onboarding Form">
    <v-tabs v-if="tab !== 3" v-model="tab">
      <v-tab :value="0">Personal details</v-tab>
      <v-tab :value="1" :disabled="!isPersonalDetailsFormValid">Employment details</v-tab>
      <v-tab :value="2" :disabled="!isPersonalDetailsFormValid || !isEmploymentDetailsFormValid">Organization details</v-tab>
    </v-tabs>

    <v-window v-model="tab">
      <v-form ref="vFormRef">
        <v-window-item :value="0">
          <v-container fluid>
            <personal-details-form v-model="isPersonalDetailsFormValid" :form-data="formData"></personal-details-form>
            <v-btn class="mt-5" color="primary" :disabled="!isPersonalDetailsFormValid" @click="nextTab()">Next</v-btn>
          </v-container>
        </v-window-item>
        <v-window-item :value="1">
          <v-container fluid>
            <employment-details-form v-model="isEmploymentDetailsFormValid" :form-data="formData"></employment-details-form>
            <v-btn class="mt-5 mr-2" variant="text" @click="previousTab()">Back</v-btn>
            <v-btn class="mt-5" color="primary" :disabled="!isEmploymentDetailsFormValid" @click="nextTab()">Next</v-btn>
          </v-container>
        </v-window-item>
        <v-window-item :value="2">
          <v-container fluid>
            <organization-details-form :form-data="formData"></organization-details-form>
            <v-btn class="mt-5 mr-2" variant="text" @click="previousTab()">Back</v-btn>
            <v-btn class="mt-5" color="primary" :loading="isSubmitting" @click="submit()">Start onboarding</v-btn>
          </v-container>
        </v-window-item>
        <v-window-item :value="3">
          <v-container fluid>
            <div class="text-h4 pb-2">Thank you!</div>
            <div class="text-subtitle-1 pb-5">
              Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
            </div>
            <v-btn class="mt-5 mr-2" color="primary" @click="startNewOnboarding()">Start new onboarding</v-btn>
            <v-btn class="mt-5" color="secondary" href="/">Homepage</v-btn>
          </v-container>
        </v-window-item>
      </v-form>
    </v-window>
  </h-form-container>
</template>

<script setup>
import { ref } from 'vue';
const { hHttpRequest } = useHCommon();
const { oUser } = useOCommon();

import PersonalDetailsForm from './onboarding/personal-details-form.vue';
import EmploymentDetailsForm from './onboarding/employment-details-form.vue';
import OrganizationDetailsForm from './onboarding/organization-details-form.vue';

const tab = ref(0);
const vFormRef = ref();
const formData = reactive({});

const isPersonalDetailsFormValid = ref(false);
const isEmploymentDetailsFormValid = ref(false);
const isSubmitting = ref(false);

function nextTab() {
  tab.value++;
}

function previousTab() {
  tab.value--;
}

function startNewOnboarding() {
  location.reload();
}

function showThankYouPage() {
  tab.value = 3;
}

async function submit() {
  const { valid } = await vFormRef.value.validate();

  if (valid) {
    isSubmitting.value = true;
    const response = await hHttpRequest({
      url: 'my/processes/start/employee-onboarding',
      method: 'post',
      payload: {
        title: `Employee onboarding by ${oUser.value.name}`,
        description: `${formData.firstName} ${formData.lastName}`,
        input: formData
      }
    }).response();
    isSubmitting.value = false;
    if (response?.status === HTTPStatusCode.CREATED) {
      showThankYouPage();
    }
  }
}
</script>

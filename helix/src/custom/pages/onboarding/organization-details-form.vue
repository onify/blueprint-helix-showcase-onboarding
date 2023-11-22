<template>
  <div>
    <v-col lg="6">
      <h-page-title class="text-h4 pb-2">Organization details</h-page-title>
      <h-page-description class="text-subtitle-1 pb-5">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      </h-page-description>
    </v-col>
    <v-row>
      <v-col>
        <h-autocomplete
          v-model:search="managerSearchParams.term"
          v-model="formData.manager"
          :items="managerData?.records"
          :loading="isManagerData"
          label="Manager"
          placeholder="Search"
          item-title="name"
          clearable
          :rules="[hValidate.required]"
        ></h-autocomplete>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <h-autocomplete
          v-model="formData.department"
          clearable
          placeholder="Search"
          label="Department"
          :items="departments?.records"
          item-title="name"
          :rules="[hValidate.required]"
        ></h-autocomplete>
      </v-col>
      <v-col>
        <h-autocomplete
          v-model="formData.country"
          clearable
          placeholder="Select country"
          label="Location"
          :items="['Sweden', 'Norway', 'Denmark', 'Finland']"
          :rules="[hValidate.required]"
        ></h-autocomplete>
      </v-col>
    </v-row>
  </div>
</template>

<script setup>
const { hHttpRequest } = useHCommon();

const props = defineProps({
  formData: Object,
});

const { data: departments } = hHttpRequest({
  url: '/my/options/tags/department',
  params: {
    term: '*',
    sort: 'name',
    sortby: 'asc',
    pagesize: 100,
  },
});

const {
  params: managerSearchParams,
  data: managerData,
  loading: isManagerData,
} = hHttpRequest({
  url: '/my/items/employees',
  method: 'get',
  params: {
    pagesize: 30,
    sort: 'name.lower,asc',
    term: (value) => value || '*',
  },
  isLive: true,
});
</script>

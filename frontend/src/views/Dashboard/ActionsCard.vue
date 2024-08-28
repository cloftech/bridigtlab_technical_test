<script lang="ts">
import { defineComponent, ref } from 'vue';
import { api } from '@/api';
import { useModal } from '@/composables/useModal';
import { useToast } from '@/composables/useToast';
import Modal from './Modal.vue';

export default defineComponent({
  components: {
    Modal,
  },
  setup() {
    const modal = useModal<boolean>();
    const toast = useToast();
    const showModal = ref<boolean>(false);

    // const formData = ref({
    //   applicantName: '',
    //   applicantEmail: '',
    //   applicantMobilePhoneNumber: '',
    //   applicantAddress: '',
    //   annualIncomeBeforeTax: 0,
    //   incomingAddress: '',
    //   incomingDeposit: 0,
    //   incomingPrice: 0,
    //   incomingStampDuty: 0,
    //   loanAmount: 0,
    //   loanDuration: 0,
    //   monthlyExpenses: 0,
    //   outgoingAddress: '',
    //   outgoingMortgage: 0,
    //   outgoingValuation: 0,
    //   savingsContribution: 0,
    // });

    const submitApplication = async () => {
      const response = await api.applications.post(formData.value);
      if (response.success) {
        toast.success('Application Saved Successfully.');
      } else {
        toast.error('Error occurred while saving application');
        // formData.value = {
        //   applicantName: '',
        //   applicantEmail: '',
        //   applicantMobilePhoneNumber: '',
        //   applicantAddress: '',
        //   annualIncomeBeforeTax: 0,
        //   incomingAddress: '',
        //   incomingDeposit: 0,
        //   incomingPrice: 0,
        //   incomingStampDuty: 0,
        //   loanAmount: 0,
        //   loanDuration: 0,
        //   monthlyExpenses: 0,
        //   outgoingAddress: '',
        //   outgoingMortgage: 0,
        //   outgoingValuation: 0,
        //   savingsContribution: 0,
        // };
      }
    };

    const openModal = () => {
      showModal.value = true;
    };

    return {
      showModal,
      formData,
      submitApplication,
      openModal,
    };
  },
});
</script>

<template>
  <div class="action-section">
    <BCard align-title="center" align-footer="center" align-content="center">
      <template #title>Submit loan application</template>
      <BSvgIcon name="dashboard-loan" />
      <template #footer>
        <BButton
          variant="primary"
          label="Submit application"
          icon-pos="right"
          icon="pi pi-chevron-right"
          @click="openModal"
        />
        <Modal v-if="showModal" @close="showModal = false" />
      </template>
    </BCard>
  </div>
</template>

<style lang="scss" scoped>
.action-section {
  display: flex;
  flex-flow: row wrap;
  gap: 1rem;
  align-items: stretch;
  container-type: inline-size;

  > * {
    flex: 1 1 100%;
  }

  @container (min-width: 900px) {
    > * {
      flex: 1 1 calc((100% - 2rem) / 3);
    }
  }
}

.b-card {
  height: 100%;
}

.b-icon {
  width: 5rem;
  height: 5rem;
}
</style>

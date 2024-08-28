<template>
  <div class="modal">
    <div class="modal-content">
      <span class="close" @click="$emit('close')">&times;</span>
      <form @submit.prevent="submitApplication">
        <div class="grid mt-6">
          <div class="col-12">
            <InputText id="applicant_name" v-model="loanApplication.applicant_name" required placeholder="Name" />
          </div>
          <div class="col-12">
            <InputText
              id="applicant_email"
              v-model="loanApplication.applicant_email"
              type="email"
              placeholder="Email"
              required
            />
          </div>
          <div class="col-12">
            <InputText
              id="applicant_mobile_phone_number"
              v-model="loanApplication.applicant_mobile_phone_number"
              placeholder="Mobile No."
              required
            />
          </div>
          <div class="col-12">
            <InputText
              id="applicant_address"
              v-model="loanApplication.applicant_address"
              required
              placeholder="Address"
            />
          </div>
          <div class="col-12">
            <InputText
              id="annual_income_before_tax"
              v-model="loanApplication.annual_income_before_tax"
              type="number"
              placeholder="Annual Income Before Tax"
              required
            />
          </div>
          <div class="col-12">
            <InputText
              id="incoming_address"
              v-model="loanApplication.incoming_address"
              required
              placeholder="Incoming Address"
            />
          </div>
          <div class="col-12">
            <InputText
              id="incoming_deposit"
              v-model="loanApplication.incoming_deposit"
              type="number"
              required
              placeholder="Incoming Deposit"
            />
          </div>
          <div class="col-12">
            <InputText
              id="incoming_price"
              v-model="loanApplication.incoming_price"
              type="number"
              required
              placeholder="Incoming Price"
            />
          </div>
          <div class="col-12">
            <InputText
              id="incoming_stamp_duty"
              v-model="loanApplication.incoming_stamp_duty"
              type="number"
              required
              placeholder="Incoming Stamp Duty"
            />
          </div>
          <div class="col-12">
            <InputText
              id="loan_amount"
              v-model="loanApplication.loan_amount"
              type="number"
              required
              placeholder="Loan Amount"
            />
          </div>
          <div class="col-12">
            <InputText
              id="loan_duration"
              v-model="loanApplication.loan_duration"
              type="number"
              required
              placeholder="Loan Duration"
            />
          </div>
          <div class="col-12">
            <InputText
              id="monthly_expenses"
              v-model="loanApplication.monthly_expenses"
              type="number"
              required
              placeholder="Monthly Expenses"
            />
          </div>
          <div class="col-12">
            <InputText
              id="outgoing_address"
              v-model="loanApplication.outgoing_address"
              required
              placeholder="Outgoing Address"
            />
          </div>
          <div class="col-12">
            <InputText
              id="outgoing_mortgage"
              v-model="loanApplication.outgoing_mortgage"
              type="number"
              required
              placeholder="Outgoing Mortgage"
            />
          </div>
          <div class="col-12">
            <InputText
              id="outgoing_valuation"
              v-model="loanApplication.outgoing_valuation"
              type="number"
              required
              placeholder="Outgoing Valuation"
            />
          </div>
          <div class="col-12">
            <InputText
              id="savings_contribution"
              v-model="loanApplication.savings_contribution"
              type="number"
              required
              placeholder="Savings Contribution"
            />
          </div>
        </div>

        <Button type="submit" label="Submit" class="mt-5 w-full" />
      </form>
      <div v-if="result">
        <p>{{ result }}</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';

interface LoanApplication {
  applicant_name: string;
  applicant_email: string;
  applicant_mobile_phone_number: string;
  applicant_address: string;
  annual_income_before_tax: string;
  incoming_address: string;
  incoming_deposit: string;
  incoming_price: string;
  incoming_stamp_duty: string;
  loan_amount: string;
  loan_duration: string;
  monthly_expenses: string;
  outgoing_address: string;
  outgoing_mortgage: string;
  outgoing_valuation: string;
  savings_contribution: string;
}

export default defineComponent({
  components: {
    InputText,
    Button,
  },
  setup() {
    const loanApplication = ref<LoanApplication>({
      applicant_name: '',
      applicant_email: '',
      applicant_mobile_phone_number: '',
      applicant_address: '',
      annual_income_before_tax: '',
      incoming_address: '',
      incoming_deposit: '',
      incoming_price: '',
      incoming_stamp_duty: '',
      loan_amount: '',
      loan_duration: '',
      monthly_expenses: '',
      outgoing_address: '',
      outgoing_mortgage: '',
      outgoing_valuation: '',
      savings_contribution: '',
    });
    const result = ref<string | null>(null);

    const submitApplication = async () => {
      try {
        const response = await fetch('brokers/applications/create-applications', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(loanApplication.value),
        });

        const data = await response.json();
        result.value = data.message;
      } catch (error) {
        console.error('Error submitting application:', error);
      }
    };

    return {
      loanApplication,
      result,
      submitApplication,
    };
  },
});
</script>

<style scoped>
.modal {
  display: block;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 5% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 50%;
  border-radius: 0.5rem;
  box-shadow: 0 1px 10px rgba(0, 0, 0, 0.1);
}
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

.mt-4 {
  margin-top: 1rem;
}

.mt-5 {
  margin-top: 1.25rem;
}

.w-full {
  width: 100%;
}
</style>

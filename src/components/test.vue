<template>
  <div class="min-h-screen bg-gray-50 text-black py-8">
    <div
      class="max-w-3xl mx-auto bg-white rounded-lg shadow-lg overflow-hidden"
    >
      <!-- Header -->
      <div class="bg-[#00078c] text-white py-4 px-6">
        <h1 class="text-2xl font-bold text-center">Vehicle Renewal Service</h1>
      </div>

      <!-- Progress Steps -->
      <div class="flex justify-between px-8 py-4 border-b">
        <div v-for="step in 4" :key="step" class="flex flex-col items-center">
          <div
            :class="[
              'w-8 h-8 rounded-full flex items-center justify-center font-bold',
              currentStep === step
                ? 'bg-[#00078c] text-white'
                : currentStep > step
                ? 'bg-green-500 text-white'
                : 'bg-gray-200 text-gray-600',
            ]"
          >
            {{ step }}
          </div>
          <span
            :class="[
              'text-sm mt-1',
              currentStep === step
                ? 'text-[#00078c] font-bold'
                : 'text-gray-500',
            ]"
          >
            {{ stepTitles[step - 1] }}
          </span>
        </div>
      </div>

      <!-- Step Content -->
      <div class="p-6">
        <!-- Step 1: Vehicle Information Entry -->
        <div v-if="currentStep === 1">
          <h2 class="text-xl font-bold text-[#00078c] mb-6">
            Enter Vehicle Details
          </h2>
          <div class="space-y-4">
            <div>
              <label class="block text-gray-700 mb-1 font-medium"
                >Plate Number</label
              >
              <input
                type="text"
                v-model="formData.plateNumber"
                maxlength="5"
                class="w-full p-2 border  border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-[#00078c]"
                placeholder="Maximum 5 digits"
                @input="validatePlateNumber"
              />
              <p v-if="errors.plateNumber" class="text-red-500 text-sm mt-1">
                {{ errors.plateNumber }}
              </p>
            </div>

            <div>
              <label class="block text-gray-700 mb-1 font-medium"
                >Plate Code</label
              >
              <input
                type="text"
                v-model="formData.plateCode"
                maxlength="2"
                class="w-full p-2 border  border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-[#00078c]"
                placeholder="Maximum 2 characters"
                @input="validatePlateCode"
              />
              <p v-if="errors.plateCode" class="text-red-500 text-sm mt-1">
                {{ errors.plateCode }}
              </p>
            </div>

            <div>
              <label class="block text-gray-700 mb-1 font-medium"
                >National ID / Organization ID</label
              >
              <input
                type="text"
                v-model="formData.nationalId"
                class="w-full p-2 border  border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-[#00078c]"
                placeholder="Enter ID number"
                @input="validateNationalId"
              />
              <p v-if="errors.nationalId" class="text-red-500 text-sm mt-1">
                {{ errors.nationalId }}
              </p>
            </div>
          </div>
        </div>

        <!-- Step 2: Vehicle Information Display -->
        <div v-else-if="currentStep === 2">
          <h2 class="text-xl font-bold text-[#00078c] mb-6">
            Vehicle Information
          </h2>

          <div class="mb-6">
            <h3
              class="text-lg font-semibold border-b border-gray-200 pb-2 mb-3"
            >
              Registration Information
            </h3>
            <div class="grid grid-cols-2 gap-4">
              <div>
                <p class="text-gray-600">Plate Number</p>
                <p class="font-medium ">
                  {{ vehicleData.registration.plateNumber }}
                </p>
              </div>
              <div>
                <p class="text-gray-600">Plate Code</p>
                <p class="font-medium">
                  {{ vehicleData.registration.plateCode }}
                </p>
              </div>
              <div>
                <p class="text-gray-600">Expiry Date</p>
                <p class="font-medium">
                  {{ vehicleData.registration.expiryDate }}
                </p>
              </div>
              <div>
                <p class="text-gray-600">National ID</p>
                <p class="font-medium">
                  {{ vehicleData.registration.nationalId }}
                </p>
              </div>
            </div>
          </div>

          <div class="mb-6">
            <h3
              class="text-lg font-semibold border-b border-gray-200 pb-2 mb-3"
            >
              Vehicle Information
            </h3>
            <div class="grid grid-cols-2 gap-4">
              <div>
                <p class="text-gray-600">Type</p>
                <p class="font-medium">{{ vehicleData.vehicle.type }}</p>
              </div>
              <div>
                <p class="text-gray-600">Make</p>
                <p class="font-medium">{{ vehicleData.vehicle.make }}</p>
              </div>
              <div>
                <p class="text-gray-600">Model</p>
                <p class="font-medium">{{ vehicleData.vehicle.model }}</p>
              </div>
              <div>
                <p class="text-gray-600">Color</p>
                <p class="font-medium">{{ vehicleData.vehicle.color }}</p>
              </div>
              <div>
                <p class="text-gray-600">Year of Manufacture</p>
                <p class="font-medium">
                  {{ vehicleData.vehicle.yearOfManufacture }}
                </p>
              </div>
            </div>
          </div>

          <div>
            <h3
              class="text-lg font-semibold border-b border-gray-200 pb-2 mb-3"
            >
              Insurance Information
            </h3>
            <div class="grid grid-cols-2 gap-4">
              <div>
                <p class="text-gray-600">Company Name</p>
                <p class="font-medium">
                  {{ vehicleData.insurance.companyName }}
                </p>
              </div>
              <div>
                <p class="text-gray-600">Insurance Type</p>
                <p class="font-medium">
                  {{ vehicleData.insurance.insuranceType }}
                </p>
              </div>
              <div>
                <p class="text-gray-600">Policy Number</p>
                <p class="font-medium">
                  {{ vehicleData.insurance.policyNumber }}
                </p>
              </div>
              <div>
                <p class="text-gray-600">Date of Issue</p>
                <p class="font-medium">
                  {{ vehicleData.insurance.dateOfIssue }}
                </p>
              </div>
              <div>
                <p class="text-gray-600">Expiry Date</p>
                <p class="font-medium">
                  {{ vehicleData.insurance.expiryDate }}
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Step 3: Payment Information -->
        <div v-else-if="currentStep === 3">
          <h2 class="text-xl font-bold text-[#00078c] mb-6">
            Payment Information
          </h2>

          <div class="bg-gray-50 p-4 rounded-lg mb-6">
            <h3 class="text-lg font-semibold mb-3">Fees Breakdown</h3>
            <div class="space-y-2">
              <div class="flex justify-between py-1 border-b">
                <span>Renewal Fees</span>
                <span class="font-medium"
                  >{{ vehicleData.fees.renewalFees.toFixed(2) }} OMR</span
                >
              </div>
              <div class="flex justify-between py-1 border-b">
                <span>Inspection Fees</span>
                <span class="font-medium"
                  >{{ vehicleData.fees.inspectionFees.toFixed(2) }} OMR</span
                >
              </div>
              <div class="flex justify-between py-1 border-b">
                <span>Plate Fees</span>
                <span class="font-medium"
                  >{{ vehicleData.fees.plateFees.toFixed(2) }} OMR</span
                >
              </div>
              <div class="flex justify-between py-1 border-b">
                <span>Card Fees</span>
                <span class="font-medium"
                  >{{ vehicleData.fees.cardFees.toFixed(2) }} OMR</span
                >
              </div>
              <div class="flex justify-between py-1 border-b">
                <span>Passenger Fees</span>
                <span class="font-medium"
                  >{{ vehicleData.fees.passengerFees.toFixed(2) }} OMR</span
                >
              </div>
              <div class="flex justify-between py-1 border-b">
                <span>Renewal Fines</span>
                <span class="font-medium"
                  >{{ vehicleData.fees.renewalFines.toFixed(2) }} OMR</span
                >
              </div>
              <div class="flex justify-between py-1 border-b">
                <span>Other Fees</span>
                <span class="font-medium"
                  >{{ vehicleData.fees.otherFees.toFixed(2) }} OMR</span
                >
              </div>
              <div class="flex justify-between py-2 font-bold text-lg">
                <span>Total</span>
                <span>{{ calculateTotal().toFixed(2) }} OMR</span>
              </div>
            </div>
          </div>

          <div class="mb-4">
            <label class="flex items-center">
              <input
                type="checkbox"
                v-model="agreeToTerms"
                class="mr-2 h-4 w-4 text-[#00078c]"
              />
              <span>I agree to the terms and conditions</span>
            </label>
            <p v-if="errors.terms" class="text-red-500 text-sm mt-1">
              {{ errors.terms }}
            </p>
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 mb-1 font-medium"
              >Payment Method</label
            >
            <select
              v-model="paymentMethod"
              class="w-full p-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-[#00078c]"
            >
              <option value="">Select payment method</option>
              <option value="credit_card">Credit Card</option>
              <option value="debit_card">Debit Card</option>
              <option value="bank_transfer">Bank Transfer</option>
            </select>
            <p v-if="errors.payment" class="text-red-500 text-sm mt-1">
              {{ errors.payment }}
            </p>
          </div>
        </div>

        <!-- Step 4: Confirmation -->
        <div v-else-if="currentStep === 4">
          <div class="text-center py-6">
            <div
              class="w-20 h-20 mx-auto mb-4 bg-green-100 rounded-full flex items-center justify-center"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-12 w-12 text-green-600"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M5 13l4 4L19 7"
                />
              </svg>
            </div>

            <h2 class="text-2xl font-bold text-[#00078c] mb-2">
              Renewal Confirmed!
            </h2>
            <p class="text-gray-600 mb-6">
              Your vehicle renewal has been processed successfully.
            </p>

            <div class="bg-gray-50 p-4 rounded-lg mb-6 text-left">
              <h3 class="text-lg font-semibold mb-3 text-center">
                Transaction Summary
              </h3>
              <div class="space-y-2">
                <div class="flex justify-between py-1 border-b">
                  <span>Reference Number</span>
                  <span class="font-medium">{{ transactionRef }}</span>
                </div>
                <div class="flex justify-between py-1 border-b">
                  <span>Transaction Date</span>
                  <span class="font-medium">{{
                    new Date().toLocaleDateString()
                  }}</span>
                </div>
                <div class="flex justify-between py-1 border-b">
                  <span>Vehicle Registration</span>
                  <span class="font-medium"
                    >{{ vehicleData.registration.plateCode }}
                    {{ vehicleData.registration.plateNumber }}</span
                  >
                </div>
                <div class="flex justify-between py-1 border-b">
                  <span>New Expiry Date</span>
                  <span class="font-medium">{{
                    calculateNewExpiryDate()
                  }}</span>
                </div>
                <div class="flex justify-between py-1 border-b font-bold">
                  <span>Total Amount Paid</span>
                  <span>{{ calculateTotal().toFixed(2) }} OMR</span>
                </div>
              </div>
            </div>

            <div class="flex space-x-4 justify-center">
              <button
                @click="printReceipt"
                class="flex items-center px-4 py-2 bg-gray-200 text-gray-800 rounded hover:bg-gray-300"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 mr-2"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M17 17h2a2 2 0 002-2v-4a2 2 0 00-2-2H5a2 2 0 00-2 2v4a2 2 0 002 2h2m2 4h6a2 2 0 002-2v-4a2 2 0 00-2-2H9a2 2 0 00-2 2v4a2 2 0 002 2zm8-12V5a2 2 0 00-2-2H9a2 2 0 00-2 2v4h10z"
                  />
                </svg>
                Print Receipt
              </button>
              <button
                @click="startNewRenewal"
                class="px-4 py-2 bg-[#00078c] text-white rounded hover:bg-blue-900"
              >
                New Renewal
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Navigation Buttons -->
      <div class="px-6 py-4 bg-gray-50 flex justify-between">
        <button
          v-if="currentStep > 1"
          @click="prevStep"
          class="px-4 py-2 border border-gray-300 rounded text-gray-700 hover:bg-gray-100 flex items-center"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5 mr-1"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M15 19l-7-7 7-7"
            />
          </svg>
          Previous
        </button>
        <div v-else></div>

        <button
          v-if="currentStep < 4"
          @click="nextStep"
          class="px-4 py-2 bg-[#00078c] text-white rounded hover:bg-blue-900 flex items-center"
        >
          Next
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5 ml-1"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M9 5l7 7-7 7"
            />
          </svg>
        </button>
        <div v-else></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VehicleRenewal",
  data() {
    return {
      currentStep: 1,
      stepTitles: ["Enter Details", "Vehicle Info", "Payment", "Confirmation"],
      formData: {
        plateNumber: "",
        plateCode: "",
        nationalId: "",
      },
      errors: {
        plateNumber: "",
        plateCode: "",
        nationalId: "",
        terms: "",
        payment: "",
      },
      agreeToTerms: false,
      paymentMethod: "",
      transactionRef:
        "ROP" +
        Math.floor(Math.random() * 1000000)
          .toString()
          .padStart(6, "0"),
      // Mock data that would come from an API in a real application
      vehicleData: {
        registration: {
          plateNumber: "12345",
          plateCode: "AB",
          expiryDate: "2024-12-31",
          nationalId: "9876543210",
        },
        vehicle: {
          type: "Sedan",
          make: "Toyota",
          model: "Camry",
          color: "White",
          yearOfManufacture: 2022,
        },
        insurance: {
          companyName: "Oman Insurance",
          insuranceType: "Comprehensive",
          policyNumber: "POL123456789",
          dateOfIssue: "2024-01-01",
          expiryDate: "2024-12-31",
        },
        fees: {
          renewalFees: 50.0,
          inspectionFees: 20.0,
          plateFees: 10.0,
          cardFees: 5.0,
          passengerFees: 15.0,
          renewalFines: 0.0,
          otherFees: 0.0,
        },
      },
    };
  },
  methods: {
    validatePlateNumber() {
      if (!this.formData.plateNumber) {
        this.errors.plateNumber = "Plate number is required";
      } else if (!/^\d{1,5}$/.test(this.formData.plateNumber)) {
        this.errors.plateNumber =
          "Plate number must contain only digits (max 5)";
      } else {
        this.errors.plateNumber = "";
      }
    },
    validatePlateCode() {
      if (!this.formData.plateCode) {
        this.errors.plateCode = "Plate code is required";
      } else if (!/^[A-Za-z]{1,2}$/.test(this.formData.plateCode)) {
        this.errors.plateCode = "Plate code must contain only letters (max 2)";
      } else {
        this.errors.plateCode = "";
      }
    },
    validateNationalId() {
      if (!this.formData.nationalId) {
        this.errors.nationalId = "National ID is required";
      } else {
        this.errors.nationalId = "";
      }
    },
    nextStep() {
      if (this.currentStep === 1) {
        this.validatePlateNumber();
        this.validatePlateCode();
        this.validateNationalId();

        if (
          this.errors.plateNumber ||
          this.errors.plateCode ||
          this.errors.nationalId
        ) {
          return;
        }

        // In a real app, we would fetch data from API based on form input
        // For demo purposes, we're using the mock data
        this.vehicleData.registration.plateNumber = this.formData.plateNumber;
        this.vehicleData.registration.plateCode = this.formData.plateCode;
        this.vehicleData.registration.nationalId = this.formData.nationalId;
      } else if (this.currentStep === 3) {
        if (!this.agreeToTerms) {
          this.errors.terms = "You must agree to the terms and conditions";
          return;
        }

        if (!this.paymentMethod) {
          this.errors.payment = "Please select a payment method";
          return;
        }

        this.errors.terms = "";
        this.errors.payment = "";
      }

      this.currentStep++;
    },
    prevStep() {
      if (this.currentStep > 1) {
        this.currentStep--;
      }
    },
    calculateTotal() {
      const fees = this.vehicleData.fees;
      return (
        fees.renewalFees +
        fees.inspectionFees +
        fees.plateFees +
        fees.cardFees +
        fees.passengerFees +
        fees.renewalFines +
        fees.otherFees
      );
    },
    calculateNewExpiryDate() {
      // Add one year to current expiry date
      const expiryDate = new Date(this.vehicleData.registration.expiryDate);
      expiryDate.setFullYear(expiryDate.getFullYear() + 1);
      return expiryDate.toLocaleDateString();
    },
    printReceipt() {
      // In a real app, this would trigger a print dialog
      window.print();
    },
    startNewRenewal() {
      // Reset the form
      this.currentStep = 1;
      this.formData = {
        plateNumber: "",
        plateCode: "",
        nationalId: "",
      };
      this.agreeToTerms = false;
      this.paymentMethod = "";
      this.errors = {
        plateNumber: "",
        plateCode: "",
        nationalId: "",
        terms: "",
        payment: "",
      };
      this.transactionRef =
        "ROP" +
        Math.floor(Math.random() * 1000000)
          .toString()
          .padStart(6, "0");
    },
  },
};
</script>

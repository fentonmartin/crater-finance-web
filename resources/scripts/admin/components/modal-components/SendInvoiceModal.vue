<template>
  <BaseModal
    :show="modalActive"
    @close="closeSendInvoiceModal"
    @open="setInitialData"
  >
    <template #header>
      <div class="flex justify-between w-full">
        {{ modalTitle }}
        <BaseIcon
          name="XIcon"
          class="w-6 h-6 text-gray-500 cursor-pointer"
          @click="closeSendInvoiceModal"
        />
      </div>
    </template>
    <form v-if="!isPreview" action="">
      <!-- v-if -->
      <div v-if="isMailSenderExist" class="px-8 py-8 sm:p-6">
        <BaseInputGrid layout="one-column" class="col-span-7">
          <BaseInputGroup
            :label="$tc('settings.mail_sender.title', 1)"
            required
            :error="
              v$.mail_sender_id.$error && v$.mail_sender_id.$errors[0].$message
            "
          >
            <BaseMultiselect
              v-model="invoiceMailForm.mail_sender_id"
              :invalid="v$.mail_sender_id.$error"
              label="name"
              :options="mailSenders"
              value-prop="id"
              :can-deselect="false"
              :can-clear="false"
              :placeholder="$t(`settings.mail_sender.select_mail_sender`)"
              searchable
              track-by="name"
              :content-loading="isFetchingInitialData"
            />
          </BaseInputGroup>
          <BaseInputGroup
            :label="$t('general.to')"
            required
            :error="v$.to.$error && v$.to.$errors[0].$message"
          >
            <BaseInput
              v-model="invoiceMailForm.to"
              type="text"
              :invalid="v$.to.$error"
              @input="v$.to.$touch()"
            />
          </BaseInputGroup>
          <BaseInputGroup
            :error="v$.subject.$error && v$.subject.$errors[0].$message"
            :label="$t('general.subject')"
            required
          >
            <BaseInput
              v-model="invoiceMailForm.subject"
              type="text"
              :invalid="v$.subject.$error"
              @input="v$.subject.$touch()"
            />
          </BaseInputGroup>
          <BaseInputGroup
            :label="$t('general.body')"
            :error="v$.body.$error && v$.body.$errors[0].$message"
            required
          >
            <BaseCustomInput
              v-model="invoiceMailForm.body"
              :fields="invoiceMailFields"
            />
          </BaseInputGroup>
        </BaseInputGrid>
      </div>
      <!-- v-else -->
      <div v-else-if="!isMailSenderExist && !isFetchingInitialData">
        <FeedbackAlert
          :title="$t('settings.mail_sender.no_mail_sender_found')"
          :description="
            $t('settings.mail_sender.no_mail_sender_found_description')
          "
          type="warning"
        >
          <template #action>
            <div class="mt-4">
              <div class="-mx-2 -my-1.5 flex">
                <button
                  type="button"
                  class="
                    bg-yellow-50
                    px-2
                    py-1.5
                    rounded-md
                    text-sm
                    font-medium
                    text-yellow-800
                    hover:bg-yellow-100
                    focus:outline-none
                    focus:ring-2
                    focus:ring-offset-2
                    focus:ring-offset-yellow-50
                    focus:ring-yellow-600
                  "
                  @click="gotoMailSender"
                >
                  {{ $t('settings.mail_sender.manage_mail_sender') }}
                </button>
              </div>
            </div>
          </template>
        </FeedbackAlert>
      </div>
      <!-- end v-if-else -->
      <div
        class="z-0 flex justify-end p-4 border-t border-gray-200 border-solid"
      >
        <BaseButton
          class="mr-3"
          variant="primary-outline"
          type="button"
          @click="closeSendInvoiceModal"
        >
          {{ $t('general.cancel') }}
        </BaseButton>
        <BaseButton
          v-if="isMailSenderExist"
          :loading="isLoading"
          :disabled="isLoading"
          variant="primary"
          type="button"
          class="mr-3"
          @click="submitForm"
        >
          <template #left="slotProps">
            <BaseIcon
              v-if="!isLoading"
              :class="slotProps.class"
              name="PhotographIcon"
            />
          </template>
          {{ $t('general.preview') }}
        </BaseButton>
      </div>
    </form>
    <div v-else>
      <div class="my-6 mx-4 border border-gray-200 relative">
        <BaseButton
          class="absolute top-4 right-4"
          :disabled="isLoading"
          variant="primary-outline"
          @click="cancelPreview"
        >
          <BaseIcon name="PencilIcon" class="h-5 mr-2" />
          Edit
        </BaseButton>

        <iframe
          :src="templateUrl"
          frameborder="0"
          class="w-full"
          style="min-height: 500px"
        ></iframe>
      </div>
      <div
        class="z-0 flex justify-end p-4 border-t border-gray-200 border-solid"
      >
        <BaseButton
          class="mr-3"
          variant="primary-outline"
          type="button"
          @click="closeSendInvoiceModal"
        >
          {{ $t('general.cancel') }}
        </BaseButton>

        <BaseButton
          :loading="isLoading"
          :disabled="isLoading"
          variant="primary"
          type="button"
          @click="submitForm()"
        >
          <BaseIcon
            v-if="!isLoading"
            name="PaperAirplaneIcon"
            class="h-5 mr-2"
          />
          {{ $t('general.send') }}
        </BaseButton>
      </div>
    </div>
  </BaseModal>
</template>

<script setup>
import { ref, computed, reactive, onMounted } from 'vue'
import { useModalStore } from '@/scripts/stores/modal'
import { useCompanyStore } from '@/scripts/admin/stores/company'
import { useNotificationStore } from '@/scripts/stores/notification'
import { useI18n } from 'vue-i18n'
import { useInvoiceStore } from '@/scripts/admin/stores/invoice'
import { useVuelidate } from '@vuelidate/core'
import { required, email, helpers } from '@vuelidate/validators'
import { useMailSenderStore } from '@/scripts/admin/stores/mail-sender'
import FeedbackAlert from '@/scripts/admin/components/FeedbackAlert.vue'
import { useRouter } from 'vue-router'

const modalStore = useModalStore()
const companyStore = useCompanyStore()
const notificationStore = useNotificationStore()
const invoiceStore = useInvoiceStore()
const mailSenderStore = useMailSenderStore()
const router = useRouter()

const { t } = useI18n()
let isLoading = ref(false)
const templateUrl = ref('')
const isPreview = ref(false)
const mailSenders = ref(null)
const isFetchingInitialData = ref(false)
const emailTemplates = ref(null)

const emit = defineEmits(['update'])

const invoiceMailFields = ref([
  'customer',
  'customerCustom',
  'invoice',
  'invoiceCustom',
  'company',
])

const invoiceMailForm = reactive({
  id: null,
  mail_sender_id: null,
  to: null,
  subject: 'New Invoice',
  body: null,
})

const modalActive = computed(() => {
  return modalStore.active && modalStore.componentName === 'SendInvoiceModal'
})

const modalTitle = computed(() => {
  return modalStore.title
})

const modalData = computed(() => {
  return modalStore.data
})

const rules = {
  mail_sender_id: {
    required: helpers.withMessage(t('validation.required'), required),
  },
  to: {
    required: helpers.withMessage(t('validation.required'), required),
    email: helpers.withMessage(t('validation.email_incorrect'), email),
  },
  subject: {
    required: helpers.withMessage(t('validation.required'), required),
  },
  body: {
    required: helpers.withMessage(t('validation.required'), required),
  },
}

const v$ = useVuelidate(
  rules,
  computed(() => invoiceMailForm)
)

function cancelPreview() {
  isPreview.value = false
}

async function setInitialData() {
  invoiceMailForm.id = modalStore.id
  if (modalData.value) {
    invoiceMailForm.to = modalData.value.customer.email
  }

  invoiceMailForm.body = companyStore.selectedCompanySettings.invoice_mail_body

  isFetchingInitialData.value = true
  let mailSenderData = await mailSenderStore.fetchMailSenders({ limit: 'all' })
  if (mailSenderData.data) {
    mailSenders.value = mailSenderData.data.data
    let defaultMailSender = mailSenderData.data.data.find(
      (mailSender) => mailSender.is_default == true
    )
    invoiceMailForm.mail_sender_id = defaultMailSender
      ? defaultMailSender.id
      : null
    isFetchingInitialData.value = false
  }
}

async function submitForm() {
  v$.value.$touch()

  if (v$.value.$invalid) {
    return true
  }

  try {
    isLoading.value = true

    if (!isPreview.value) {
      const previewResponse = await invoiceStore.previewInvoice(invoiceMailForm)
      isLoading.value = false

      isPreview.value = true
      var blob = new Blob([previewResponse.data], { type: 'text/html' })
      templateUrl.value = URL.createObjectURL(blob)

      return
    }

    const response = await invoiceStore.sendInvoice(invoiceMailForm)

    isLoading.value = false

    if (response.data.success) {
      emit('update', modalStore.id)
      closeSendInvoiceModal()
      return true
    }
  } catch (error) {
    console.error(error)
    isLoading.value = false
    notificationStore.showNotification({
      type: 'error',
      message: t('invoices.something_went_wrong'),
    })
  }
}

function closeSendInvoiceModal() {
  modalStore.closeModal()
  setTimeout(() => {
    v$.value.$reset()
    isPreview.value = false
    templateUrl.value = null
  }, 300)
}

function getTickImage() {
  const imgUrl = new URL('/img/tick.png', import.meta.url)
  return imgUrl
}

const isMailSenderExist = computed(() => {
  return mailSenders.value && mailSenders.value.length
})

function gotoMailSender() {
  closeSendInvoiceModal()
  router.push('/admin/settings/mail-sender')
}
</script>

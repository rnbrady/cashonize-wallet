<script setup lang="ts">
  import { computed } from 'vue'
  import { type Verify } from '@walletconnect/types'
  import { useI18n } from 'vue-i18n'
  const { t } = useI18n()

  const props = defineProps<{
    verifyContext: Verify.Context
  }>()

  const validation = computed(() => props.verifyContext.verified.validation)
  const isScam = computed(() => props.verifyContext.verified.isScam === true)
</script>

<template>
  <!-- isScam takes priority -->
  <template v-if="isScam">
    <div class="verify-label verify-label-danger">&#x1F6D1; {{ t('walletConnect.verify.securityRisk') }}</div>
    <div class="verify-box verify-box-danger">
      {{ t('walletConnect.verify.scamWarning') }}
    </div>
  </template>
  <template v-else-if="validation === 'INVALID'">
    <div class="verify-label verify-label-danger">&#9888; {{ t('walletConnect.verify.domainMismatch') }}</div>
    <div class="verify-box verify-box-danger">
      {{ t('walletConnect.verify.invalidWarning') }}
    </div>
  </template>
  <template v-else-if="validation === 'UNKNOWN'">
    <div class="verify-label verify-label-warning">&#9888; {{ t('walletConnect.verify.cannotVerify') }}</div>
    <div class="verify-box verify-box-warning">
      {{ t('walletConnect.verify.unknownWarning') }}
    </div>
  </template>
  <!-- VALID: show nothing -->
</template>

<style scoped>
  .verify-label {
    font-size: small;
    font-weight: 600;
    margin-top: 0.5rem;
  }
  .verify-label-danger {
    color: #f44336;
  }
  .verify-label-warning {
    color: #e65100;
  }
  body.dark .verify-label-warning {
    color: #ffb74d;
  }

  .verify-box {
    font-size: small;
    padding: 8px 12px;
    border-radius: 4px;
    margin-top: 0.5rem;
  }
  .verify-box-danger {
    background-color: #ffebee;
    color: #b71c1c;
  }
  body.dark .verify-box-danger {
    background-color: #3d1b1b;
    color: #ef9a9a;
  }
  .verify-box-warning {
    background-color: #fff3e0;
    color: #e65100;
  }
  body.dark .verify-box-warning {
    background-color: #3d2e1b;
    color: #ffb74d;
  }
</style>

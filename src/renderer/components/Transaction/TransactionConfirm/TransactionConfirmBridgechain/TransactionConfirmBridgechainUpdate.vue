<template>
  <ListDivided
    class="TransactionConfirmBridgechainUpdate"
    :is-floating-label="true"
  >
    <ListDividedItem
      class="TransactionConfirmBridgechainUpdate__sender"
      :label="$t('TRANSACTION.SENDER')"
    >
      {{ senderLabel }}
      <span
        v-if="senderLabel !== currentWallet.address"
        class="text-sm text-theme-page-text-light"
      >
        {{ currentWallet.address }}
      </span>
    </ListDividedItem>

    <ListDividedItem
      v-if="transaction.asset.bridgechainUpdate.bridgechainRepository"
      class="TransactionConfirmBridgechainUpdate__bridgechain-repo"
      :label="$t('WALLET_BUSINESS.BRIDGECHAIN.BRIDGECHAIN_REPOSITORY')"
    >
      {{ transaction.asset.bridgechainUpdate.bridgechainRepository }}
    </ListDividedItem>

    <ListDividedItem
      v-if="transaction.asset.bridgechainUpdate.bridgechainAssetRepository"
      class="TransactionConfirmBridgechainUpdate__bridgechain-asset-repo"
      :label="$t('WALLET_BUSINESS.BRIDGECHAIN.BRIDGECHAIN_ASSET_REPOSITORY')"
    >
      {{ transaction.asset.bridgechainUpdate.bridgechainAssetRepository }}
    </ListDividedItem>

    <ListDividedItem
      v-if="transaction.asset.bridgechainUpdate.seedNodes"
      class="TransactionConfirmBridgechainUpdate__seed-nodes"
      :label="$t('WALLET_BUSINESS.BRIDGECHAIN.SEED_NODES')"
    >
      <div
        v-for="(seedNode, id) of transaction.asset.bridgechainUpdate.seedNodes"
        :key="id"
      >
        {{ seedNode }}
      </div>
    </ListDividedItem>

    <ListDividedItem
      v-if="apiPort"
      class="TransactionConfirmBridgechainUpdate__api-port"
      :label="$t('WALLET_BUSINESS.BRIDGECHAIN.API_PORT')"
    >
      {{ apiPort }}
    </ListDividedItem>
  </ListDivided>
</template>

<script>
import { TRANSACTION_GROUPS, TRANSACTION_TYPES } from '@config'
import { ListDivided, ListDividedItem } from '@/components/ListDivided'

export default {
  name: 'TransactionConfirmBridgechainUpdate',

  transactionGroup: TRANSACTION_GROUPS.MAGISTRATE,

  transactionType: TRANSACTION_TYPES.GROUP_2.BRIDGECHAIN_UPDATE,

  inject: ['currentWallet', 'transaction'],

  components: {
    ListDivided,
    ListDividedItem
  },

  computed: {
    senderLabel () {
      return this.wallet_formatAddress(this.currentWallet.address)
    },

    apiPort () {
      if (
        !this.transaction.asset.bridgechainUpdate.ports ||
        (
          this.transaction.asset.bridgechainUpdate.ports &&
          !this.transaction.asset.bridgechainUpdate.ports['@arkecosystem/core-api']
        )
      ) {
        return null
      }

      return this.transaction.asset.bridgechainUpdate.ports['@arkecosystem/core-api']
    }
  }
}
</script>

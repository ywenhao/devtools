<script setup lang="ts">
import type { Asset as AssetInfo } from '@rolldown/debug'

defineProps<{
  assets: AssetInfo[]
}>()
</script>

<template>
  <div p4>
    <div flex="~ col gap-2">
      <template v-for="asset in assets" :key="asset.id">
        <NuxtLink
          :to="{ query: { asset: asset.filename } }"
          font-mono border="~ rounded base" px2 py1 text-sm hover="bg-active"
        >
          <div flex="~ gap-1">
            <DisplayFileIcon :filename="asset.filename" />
            <span overflow-hidden text-ellipsis break-all line-clamp-2>
              {{ asset.filename }}
            </span>
          </div>
        </NuxtLink>
      </template>
    </div>
  </div>
</template>

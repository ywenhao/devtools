<script setup lang="ts">
import type { RolldownAssetInfo, RolldownChunkInfo, SessionContext } from '~~/shared/types'
import { useRpc } from '#imports'
import { computed, ref } from 'vue'

const props = defineProps<{
  chunks: RolldownChunkInfo[]
  session: SessionContext
  asset: RolldownAssetInfo
}>()

const rpc = useRpc()
const showSource = ref(false)
const assetChunks = computed(() => props.chunks.filter(c => c.chunk_id === props.asset.chunk_id))

function openInEditor() {
  rpc.value!['vite:open-in-editor'](`${props.session.meta.dir}/${props.asset.filename}`)
}
</script>

<template>
  <div flex="~ col gap-3">
    <div flex="~ gap-4 items-center wrap">
      <div flex="~ gap-2 items-center">
        <div i-catppuccin-java-class-abstract />
        <div>{{ asset.filename }}</div>
        <DisplayFileSizeBadge :bytes="asset.size" text-sm />
        <DisplayBadge :text="asset.type" />
      </div>
      <div flex-auto />
      <div flex="~ gap-2">
        <button btn-action @click="openInEditor">
          <div i-carbon-launch />
          Open in editor
        </button>
        <button btn-action @click="showSource = true">
          <div i-ph-file-text />
          View source
        </button>
      </div>
    </div>
    <template v-if="showSource">
      <div flex="~ gap-2 items-center">
        <div op50>
          Source
        </div>
        <span flex-auto />
        <DisplayCloseButton @click="showSource = false" />
      </div>
      <div class="w-full of-auto px2 py1" border="~ base rounded-lg">
        <CodeDiffEditor
          :from="asset.content!"
          :to="asset.content!"
          :diff="false"
          :one-column="true"
        />
      </div>
    </template>
    <div op50>
      Chunks
    </div>
    <div v-for="chunk of assetChunks" :key="chunk.chunk_id" border="~ base rounded-lg" px2 py1>
      <DataChunkDetails
        :chunk="chunk"
        :session="session"
        :show-modules="false"
      />
    </div>
  </div>
</template>

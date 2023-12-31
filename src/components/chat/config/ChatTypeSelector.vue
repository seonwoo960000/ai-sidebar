<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { ChatType } from '@/common/chat-type';

onMounted(() => {
  selectModel(selectedChatType.value);
});

const emits = defineEmits(['updateChatType']);

const chatTypes: ChatType[] = [ChatType.TEXT, ChatType.IMAGE];
const selectedChatType = ref<ChatType>(ChatType.TEXT);
const icon = {
  [ChatType.TEXT]: 'mdi-format-text',
  [ChatType.IMAGE]: 'mdi-image-outline',
};

/**
 * Select a role from the list of {@link OpenaiModel} values.
 * Used as a parameter for OpenAI API request.
 * @param chatTypeStr OpenAi Model
 */
function selectModel(chatTypeStr: ChatType) {
  selectedChatType.value = chatTypeStr;
  emits('updateChatType', selectedChatType.value);
}
</script>

<template>
  <v-menu>
    <template v-slot:activator="{ props }">
      <v-btn
        size="x-small"
        :prepend-icon="icon[selectedChatType]"
        rounded
        variant="outlined"
        color="primary"
        class="cy-chat-type-selector-button"
        v-bind="props"
      >
        {{ selectedChatType.toUpperCase() }}
      </v-btn>
    </template>
    <v-list>
      <v-list-item
        v-for="(chatType, index) in chatTypes"
        :key="index"
        @click="selectModel(chatType)"
        :class="`cy-chat-type-selector-${chatType}`"
      >
        <v-list-item-title>{{ chatType }}</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-menu>
</template>

<style scoped>
.cy-chat-type-selector-button {
  margin-right: 8px;
}
</style>

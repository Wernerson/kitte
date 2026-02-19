<script setup lang="ts">
const message = ref('')

const sampleMessages = [
  {
    id: 1,
    role: 'assistant' as const,
    content: 'Hello! 👋 I\'m Kitte, your AI assistant. How can I help you today?'
  },
  {
    id: 2,
    role: 'user' as const,
    content: 'Tell me about Kitte — what can you do?'
  },
  {
    id: 3,
    role: 'assistant' as const,
    content: 'I can help you with a wide range of tasks! Whether it\'s answering questions, brainstorming ideas, writing content, or solving problems — I\'m here for you. Just type your question below and let\'s get started! 🚀'
  }
]
</script>

<template>
  <div class="chat-container">
    <!-- Top Bar -->
    <header class="chat-header">
      <div class="chat-header-inner">
        <div class="logo-placeholder">
          <div class="logo-icon">
            K
          </div>
          <span class="logo-text">Kitte</span>
        </div>
      </div>
    </header>

    <!-- Chat Messages -->
    <main class="chat-messages">
      <div class="messages-inner">
        <div
          v-for="msg in sampleMessages"
          :key="msg.id"
          class="message-row"
          :class="msg.role === 'user' ? 'message-row-user' : 'message-row-assistant'"
        >
          <!-- Avatar -->
          <div
            v-if="msg.role === 'assistant'"
            class="avatar avatar-assistant"
          >
            <UIcon
              name="i-lucide-bot"
              class="avatar-icon"
            />
          </div>

          <!-- Bubble -->
          <div
            class="message-bubble"
            :class="msg.role === 'user' ? 'bubble-user' : 'bubble-assistant'"
          >
            {{ msg.content }}
          </div>

          <!-- User avatar -->
          <div
            v-if="msg.role === 'user'"
            class="avatar avatar-user"
          >
            <UIcon
              name="i-lucide-user"
              class="avatar-icon"
            />
          </div>
        </div>
      </div>
    </main>

    <!-- Input Bar -->
    <footer class="chat-input-bar">
      <div class="chat-input-inner">
        <div class="input-wrapper">
          <UInput
            v-model="message"
            placeholder="Type your message..."
            size="xl"
            class="input-field"
            :ui="{
              root: 'flex-1'
            }"
          />
          <UButton
            icon="i-lucide-send"
            size="xl"
            color="primary"
            aria-label="Send message"
            class="send-button"
          />
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
/* ───── Layout ───── */
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100dvh;
  background: var(--ui-bg);
}

/* ───── Header ───── */
.chat-header {
  flex-shrink: 0;
  border-bottom: 1px solid var(--ui-border);
  backdrop-filter: blur(12px);
  background: var(--ui-bg-elevated);
}

.chat-header-inner {
  max-width: 56rem;
  margin: 0 auto;
  padding: 0.75rem 1rem;
  display: flex;
  align-items: center;
}

.logo-placeholder {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.logo-icon {
  width: 2rem;
  height: 2rem;
  border-radius: 0.5rem;
  background: linear-gradient(135deg, var(--color-orange-400), var(--color-orange-600));
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 0.875rem;
}

.logo-text {
  font-weight: 700;
  font-size: 1.125rem;
  color: var(--ui-text);
}

/* ───── Messages area ───── */
.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: 1.5rem 1rem;
}

.messages-inner {
  max-width: 56rem;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

/* ───── Message row ───── */
.message-row {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
}

.message-row-user {
  justify-content: flex-end;
}

.message-row-assistant {
  justify-content: flex-start;
}

/* ───── Avatars ───── */
.avatar {
  flex-shrink: 0;
  width: 2rem;
  height: 2rem;
  border-radius: 9999px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.avatar-assistant {
  background: linear-gradient(135deg, var(--color-orange-400), var(--color-orange-600));
  color: white;
}

.avatar-user {
  background: var(--ui-bg-accented);
  color: var(--ui-text-muted);
}

.avatar-icon {
  width: 1rem;
  height: 1rem;
}

/* ───── Bubbles ───── */
.message-bubble {
  max-width: 75%;
  padding: 0.75rem 1rem;
  border-radius: 1rem;
  font-size: 0.9375rem;
  line-height: 1.5;
  word-break: break-word;
}

.bubble-assistant {
  background: linear-gradient(135deg, var(--color-orange-500), var(--color-orange-600));
  color: white;
  border-bottom-left-radius: 0.25rem;
}

.bubble-user {
  background: var(--ui-bg-accented);
  color: var(--ui-text);
  border-bottom-right-radius: 0.25rem;
}

/* ───── Input bar ───── */
.chat-input-bar {
  flex-shrink: 0;
  border-top: 1px solid var(--ui-border);
  background: var(--ui-bg-elevated);
  padding: 0.75rem 1rem;
  padding-bottom: max(0.75rem, env(safe-area-inset-bottom));
}

.chat-input-inner {
  max-width: 56rem;
  margin: 0 auto;
}

.input-wrapper {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.input-field {
  flex: 1;
}

.send-button {
  flex-shrink: 0;
}

/* ───── Responsive tweaks ───── */
@media (max-width: 640px) {
  .message-bubble {
    max-width: 85%;
    font-size: 0.875rem;
  }

  .chat-header-inner,
  .chat-input-inner {
    padding-left: 0.75rem;
    padding-right: 0.75rem;
  }
}
</style>

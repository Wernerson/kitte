<script setup lang="ts">
interface TextMessage {
  id: number
  role: 'user' | 'assistant'
  type: 'text'
  content: string
}

interface CandidateMessage {
  id: number
  role: 'assistant'
  type: 'candidate'
  signpost: string
  candidate: {
    name: string
    canton: string
    portrait: string
    bio: string
  }
}

interface PolicyMessage {
  id: number
  role: 'assistant'
  type: 'policy'
  summary: string
  paper: {
    title: string
    url: string
  }
}

type ChatMessage = TextMessage | CandidateMessage | PolicyMessage

const message = ref('')

const sampleMessages: ChatMessage[] = [
  {
    id: 1,
    role: 'assistant',
    type: 'text',
    content: 'Hello! 👋 I\'m Kitte, your AI assistant for the upcoming elections. Ask me about our candidates, policies, or anything else you\'d like to know!'
  },
  {
    id: 2,
    role: 'user',
    type: 'text',
    content: 'Who is running for the National Council in Zürich?'
  },
  {
    id: 3,
    role: 'assistant',
    type: 'candidate',
    signpost: 'Great question! Here\'s one of our candidates for Canton Zürich:',
    candidate: {
      name: 'Laura Meier',
      canton: 'Zürich',
      portrait: '/images/candidate.webp',
      bio: 'Laura Meier is a 42-year-old environmental engineer and city councillor in Winterthur. She has championed sustainable urban development and affordable housing for over a decade. As a mother of two, she is passionate about building a future that works for the next generation.'
    }
  },
  {
    id: 4,
    role: 'user',
    type: 'text',
    content: 'What is your stance on renewable energy?'
  },
  {
    id: 5,
    role: 'assistant',
    type: 'policy',
    summary: 'We are committed to achieving net-zero emissions by 2040. Our energy policy focuses on massively expanding solar and wind capacity, modernizing the grid, and providing incentives for households and businesses to transition to clean energy. We also advocate for phasing out fossil fuel subsidies and investing in green hydrogen research.',
    paper: {
      title: 'Energy Transition Roadmap 2040 — Full Policy Paper',
      url: '#'
    }
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
          <!-- Bot Avatar -->
          <div
            v-if="msg.role === 'assistant'"
            class="avatar avatar-assistant"
          >
            <UIcon
              name="i-lucide-bot"
              class="avatar-icon"
            />
          </div>

          <!-- Plain text bubble -->
          <div
            v-if="msg.type === 'text'"
            class="message-bubble"
            :class="msg.role === 'user' ? 'bubble-user' : 'bubble-assistant'"
          >
            {{ msg.content }}
          </div>

          <!-- Candidate card -->
          <div
            v-else-if="msg.type === 'candidate'"
            class="message-bubble bubble-assistant bubble-rich"
          >
            <p class="signpost-text">
              {{ msg.signpost }}
            </p>
            <div class="candidate-card">
              <img
                :src="msg.candidate.portrait"
                :alt="msg.candidate.name"
                class="candidate-photo"
              >
              <div class="candidate-info">
                <h3 class="candidate-name">
                  {{ msg.candidate.name }}
                </h3>
                <span class="candidate-canton">
                  <UIcon
                    name="i-lucide-map-pin"
                    class="canton-icon"
                  />
                  Canton {{ msg.candidate.canton }}
                </span>
                <p class="candidate-bio">
                  {{ msg.candidate.bio }}
                </p>
              </div>
            </div>
          </div>

          <!-- Policy card -->
          <div
            v-else-if="msg.type === 'policy'"
            class="message-bubble bubble-assistant bubble-rich"
          >
            <p class="policy-summary">
              {{ msg.summary }}
            </p>
            <a
              :href="msg.paper.url"
              class="policy-link"
              target="_blank"
            >
              <UIcon
                name="i-lucide-file-text"
                class="policy-link-icon"
              />
              <span>{{ msg.paper.title }}</span>
              <UIcon
                name="i-lucide-external-link"
                class="policy-link-external"
              />
            </a>
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

.bubble-rich {
  max-width: 85%;
  padding: 1rem;
}

/* ───── Candidate card ───── */
.signpost-text {
  margin-bottom: 0.75rem;
  font-size: 0.9375rem;
}

.candidate-card {
  display: flex;
  background: rgba(255, 255, 255, 0.15);
  border-radius: 0.75rem;
  overflow: hidden;
  backdrop-filter: blur(8px);
}

.candidate-photo {
  width: 7rem;
  align-self: stretch;
  object-fit: cover;
  object-position: top;
  flex-shrink: 0;
}

.candidate-info {
  padding: 0.75rem 1rem;
}

.candidate-name {
  font-size: 1.125rem;
  font-weight: 700;
  margin: 0 0 0.25rem 0;
}

.candidate-canton {
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
  font-size: 0.8125rem;
  opacity: 0.85;
  margin-bottom: 0.5rem;
}

.canton-icon {
  width: 0.875rem;
  height: 0.875rem;
}

.candidate-bio {
  font-size: 0.8125rem;
  line-height: 1.5;
  opacity: 0.9;
  margin: 0;
}

/* ───── Policy card ───── */
.policy-summary {
  margin: 0 0 0.75rem 0;
  font-size: 0.9375rem;
  line-height: 1.6;
}

.policy-link {
  display: flex;
  align-items: center;
  gap: 0.375rem;
  background: rgba(255, 255, 255, 0.15);
  border-radius: 0.5rem;
  padding: 0.625rem 0.75rem;
  color: white;
  text-decoration: none;
  font-size: 0.8125rem;
  font-weight: 500;
  transition: background 0.2s;
  backdrop-filter: blur(8px);
}

.policy-link:hover {
  background: rgba(255, 255, 255, 0.25);
}

.policy-link-icon {
  width: 0.75rem;
  height: 0.75rem;
  flex: 0 0 auto;
}

.policy-link span {
  flex: 1 1 auto;
  min-width: 0;
}

.policy-link-external {
  width: 0.625rem;
  height: 0.625rem;
  flex: 0 0 auto;
  opacity: 0.7;
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

  .bubble-rich {
    max-width: 92%;
  }

  .candidate-photo {
    aspect-ratio: 1;
  }

  .chat-header-inner,
  .chat-input-inner {
    padding-left: 0.75rem;
    padding-right: 0.75rem;
  }
}
</style>

<template>
  <div class="page">
    <a-layout class="layout">
      <a-layout-header class="header">GPT-Image-2 Web Studio</a-layout-header>
      <a-layout-content class="content">
        <a-row :gutter="16">
          <a-col :xs="24" :lg="8">
            <a-card title="API Settings" :bordered="false">
              <a-form layout="vertical">
                <a-form-item label="API Base URL">
                  <a-input v-model:value="baseUrl" placeholder="Example: https://your-proxy.com/v1" />
                </a-form-item>
                <a-form-item label="API Key">
                  <a-input-password v-model:value="apiKey" placeholder="Enter API key" />
                </a-form-item>
                <a-space>
                  <a-button type="primary" :loading="fetchingModels" @click="fetchModels">Fetch Models</a-button>
                  <a-checkbox v-model:checked="showAllModels">Show All Models</a-checkbox>
                </a-space>
                <p class="hint">{{ modelStatus }}</p>
                <a-form-item label="Model">
                  <a-select
                    v-model:value="selectedModel"
                    show-search
                    :options="modelOptions"
                    :filter-option="filterOption"
                    placeholder="Fetch models first"
                  />
                </a-form-item>
              </a-form>
            </a-card>

            <a-card title="Generation" :bordered="false" style="margin-top: 16px">
              <a-form layout="vertical">
                <a-form-item label="Prompt">
                  <a-textarea v-model:value="prompt" :rows="6" placeholder="Describe the image" />
                </a-form-item>
                <a-row :gutter="12">
                  <a-col :span="12">
                    <a-form-item label="Size">
                      <a-select v-model:value="size" :options="sizeOptions" />
                    </a-form-item>
                  </a-col>
                  <a-col :span="12">
                    <a-form-item label="Quality">
                      <a-select v-model:value="quality" :options="qualityOptions" />
                    </a-form-item>
                  </a-col>
                </a-row>
                <a-row :gutter="12">
                  <a-col :span="12">
                    <a-form-item label="Format">
                      <a-select v-model:value="outputFormat" :options="formatOptions" />
                    </a-form-item>
                  </a-col>
                  <a-col :span="12">
                    <a-form-item label="Count">
                      <a-input-number v-model:value="count" :min="1" :max="4" style="width: 100%" />
                    </a-form-item>
                  </a-col>
                </a-row>
                <a-form-item>
                  <a-button type="primary" block :loading="generating" @click="generateImages">Generate</a-button>
                </a-form-item>
              </a-form>
            </a-card>
          </a-col>

          <a-col :xs="24" :lg="16">
            <a-card title="Preview" :bordered="false">
              <div v-if="!images.length && !generating" class="empty">No images yet</div>
              <a-row v-else :gutter="12">
                <a-col v-for="(item, idx) in images" :key="idx" :xs="24" :md="12">
                  <a-card size="small" class="image-card">
                    <img :src="item.dataUrl" alt="generated" class="preview" />
                    <a-space style="margin-top: 10px">
                      <a-button size="small" @click="downloadImage(item, idx)">Download</a-button>
                      <a-button size="small" @click="copyPrompt">Copy Prompt</a-button>
                    </a-space>
                  </a-card>
                </a-col>
              </a-row>
            </a-card>
          </a-col>
        </a-row>
      </a-layout-content>
    </a-layout>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import { message } from 'ant-design-vue';

const baseUrl = ref('https://api.openai.com/v1');
const apiKey = ref('');
const fetchingModels = ref(false);
const generating = ref(false);
const modelStatus = ref('');
const allModels = ref([]);
const showAllModels = ref(false);
const selectedModel = ref('gpt-image-2');

const prompt = ref('A cinematic portrait of a cyberpunk fox in neon rain, ultra-detailed');
const size = ref('1024x1024');
const quality = ref('medium');
const outputFormat = ref('png');
const count = ref(1);

const images = ref([]);

const sizeOptions = [
  { value: '1024x1024', label: '1024x1024' },
  { value: '1536x1024', label: '1536x1024' },
  { value: '1024x1536', label: '1024x1536' },
  { value: 'auto', label: 'auto' }
];

const qualityOptions = [
  { value: 'low', label: 'low' },
  { value: 'medium', label: 'medium' },
  { value: 'high', label: 'high' },
  { value: 'auto', label: 'auto' }
];

const formatOptions = [
  { value: 'png', label: 'png' },
  { value: 'webp', label: 'webp' },
  { value: 'jpeg', label: 'jpeg' }
];

const modelOptions = computed(() => {
  const filtered = showAllModels.value
    ? allModels.value
    : allModels.value.filter((m) => /gpt-image|image/i.test(m));

  const merged = Array.from(new Set(['gpt-image-2', selectedModel.value, ...filtered].filter(Boolean)));
  return merged.map((m) => ({ value: m, label: m }));
});

function normalizeBaseUrl() {
  return String(baseUrl.value || '').trim().replace(/\/+$/, '');
}

function authHeaders() {
  return {
    Authorization: `Bearer ${apiKey.value.trim()}`
  };
}

function filterOption(input, option) {
  return String(option.label || '').toLowerCase().includes(String(input || '').toLowerCase());
}

async function fetchModels() {
  if (!apiKey.value.trim()) {
    message.warning('Please enter API key first');
    return;
  }

  fetchingModels.value = true;
  modelStatus.value = 'Fetching models...';

  try {
    const res = await fetch(`${normalizeBaseUrl()}/models`, {
      method: 'GET',
      headers: authHeaders()
    });

    const json = await res.json().catch(() => ({}));
    if (!res.ok) {
      throw new Error(json?.error?.message || json?.message || `HTTP ${res.status}`);
    }

    const list = Array.isArray(json.data) ? json.data.map((it) => it.id).filter(Boolean) : [];
    allModels.value = list.sort((a, b) => a.localeCompare(b));

    if (!allModels.value.includes(selectedModel.value) && allModels.value.length) {
      selectedModel.value = allModels.value[0];
    }

    modelStatus.value = `Fetched ${allModels.value.length} models`;
    message.success(modelStatus.value);
  } catch (err) {
    modelStatus.value = err?.message || 'Failed to fetch models';
    message.error(modelStatus.value);
  } finally {
    fetchingModels.value = false;
  }
}

async function generateImages() {
  if (!apiKey.value.trim()) {
    message.warning('Please enter API key first');
    return;
  }
  if (!selectedModel.value) {
    message.warning('Please select model first');
    return;
  }
  if (!prompt.value.trim()) {
    message.warning('Please enter prompt');
    return;
  }

  generating.value = true;
  images.value = [];

  try {
    const res = await fetch(`${normalizeBaseUrl()}/images/generations`, {
      method: 'POST',
      headers: {
        ...authHeaders(),
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        model: selectedModel.value,
        prompt: prompt.value.trim(),
        size: size.value,
        quality: quality.value,
        output_format: outputFormat.value,
        n: Number(count.value) || 1
      })
    });

    const json = await res.json().catch(() => ({}));
    if (!res.ok) {
      throw new Error(json?.error?.message || json?.message || `HTTP ${res.status}`);
    }

    const data = Array.isArray(json.data) ? json.data : [];
    const mime = outputFormat.value === 'jpg' ? 'image/jpeg' : `image/${outputFormat.value}`;

    images.value = data
      .map((item) => item?.b64_json || item?.result || '')
      .filter(Boolean)
      .map((b64) => ({ b64, dataUrl: `data:${mime};base64,${b64}` }));

    if (!images.value.length) {
      throw new Error('No image data in response');
    }

    message.success(`Generated ${images.value.length} image(s)`);
  } catch (err) {
    message.error(err?.message || 'Generation failed');
  } finally {
    generating.value = false;
  }
}

function downloadImage(item, index) {
  const ext = outputFormat.value === 'jpeg' ? 'jpg' : outputFormat.value;
  const link = document.createElement('a');
  link.href = item.dataUrl;
  link.download = `gpt-image2-${index + 1}.${ext}`;
  document.body.appendChild(link);
  link.click();
  link.remove();
}

async function copyPrompt() {
  try {
    await navigator.clipboard.writeText(prompt.value);
    message.success('Prompt copied');
  } catch {
    message.warning('Copy failed');
  }
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background: #f5f7fa;
}

.layout {
  min-height: 100vh;
}

.header {
  color: #fff;
  font-size: 18px;
  font-weight: 600;
  background: #111827;
}

.content {
  padding: 16px;
}

.hint {
  margin: 8px 0 10px;
  color: #6b7280;
  font-size: 12px;
}

.empty {
  height: 240px;
  display: grid;
  place-items: center;
  color: #94a3b8;
  background: #fafafa;
  border: 1px dashed #d9d9d9;
  border-radius: 8px;
}

.image-card {
  margin-bottom: 12px;
}

.preview {
  width: 100%;
  border-radius: 8px;
  display: block;
}
</style>

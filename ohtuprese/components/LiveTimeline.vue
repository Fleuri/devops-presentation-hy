<script setup lang="ts">
import {nextTick, onMounted, onUnmounted, ref} from 'vue'

interface Item {
  id: number
  text: string
  /** centre position as a percentage of the canvas */
  x: number
  y: number
  core?: boolean
}

const props = withDefaults(defineProps<{
  /** Unique key for persisting this timeline in localStorage */
  storageKey?: string
  /** Initial (core) item shown in the middle */
  initial?: string
  /** Canvas height (CSS length) */
  height?: string
}>(), {
  storageKey: 'live-timeline',
  initial: 'Koodin kirjoittaminen',
  height: '22rem',
})

const key = `slidev-live-timeline:${props.storageKey}`

function defaults(): Item[] {
  return [{id: 1, text: props.initial, x: 50, y: 50, core: true}]
}

function load(): Item[] {
  try {
    const raw = localStorage.getItem(key)
    if (raw) {
      const parsed = JSON.parse(raw) as Item[]
      // older saved data had no positions: spread them along the line
      return parsed.map((it, i) => ({
        ...it,
        x: it.x ?? ((i + 1) / (parsed.length + 1)) * 100,
        y: it.y ?? 50,
      }))
    }
  } catch {
  }
  return defaults()
}

const items = ref<Item[]>(defaults())

const instanceId = Math.random().toString(36).slice(2)
const SYNC_EVENT = 'slidev-live-timeline-sync'

function save() {
  localStorage.setItem(key, JSON.stringify(items.value))
  // tell other timelines in this same page (e.g. another slide) to reload
  window.dispatchEvent(new CustomEvent(SYNC_EVENT, { detail: { key, from: instanceId } }))
}

// another timeline in this page saved the same storage key
function onSync(e: Event) {
  const { key: k, from } = (e as CustomEvent).detail
  if (k === key && from !== instanceId && dragId.value === null)
    items.value = load()
}

// Keep presenter view and audience window in sync (they are separate tabs)
function onStorage(e: StorageEvent) {
  if (e.key === key && dragId.value === null)
    items.value = load()
}

onMounted(() => {
  items.value = load()
  window.addEventListener('storage', onStorage)
  window.addEventListener(SYNC_EVENT, onSync)
})
onUnmounted(() => {
  window.removeEventListener('storage', onStorage)
  window.removeEventListener(SYNC_EVENT, onSync)
})

const canvas = ref<HTMLElement | null>(null)
const inputEl = ref<HTMLInputElement[] | HTMLInputElement | null>(null)

function focusInput(select = false) {
  nextTick(() => {
    const el = Array.isArray(inputEl.value) ? inputEl.value[0] : inputEl.value
    el?.focus()
    if (select)
      el?.select()
  })
}

/** Convert a pointer position to canvas percentages (works with Slidev's scaling) */
function toPercent(clientX: number, clientY: number) {
  const rect = canvas.value!.getBoundingClientRect()
  const clamp = (v: number) => Math.min(100, Math.max(0, v))
  return {
    x: clamp(((clientX - rect.left) / rect.width) * 100),
    y: clamp(((clientY - rect.top) / rect.height) * 100),
  }
}

// --- adding: double-click on empty space, or the "+" button ---
const adding = ref<{ x: number, y: number } | null>(null)
const draft = ref('')

function startAddAt(e: MouseEvent) {
  editingId.value = null
  adding.value = toPercent(e.clientX, e.clientY)
  draft.value = ''
  focusInput()
}

function startAddDefault() {
  editingId.value = null
  // place new boxes in a free-ish spot above the line, cycling across
  const n = items.value.length
  adding.value = {x: 15 + ((n * 17) % 70), y: n % 2 ? 22 : 78}
  draft.value = ''
  focusInput()
}

function commitAdd() {
  const text = draft.value.trim()
  if (text && adding.value) {
    const id = Math.max(0, ...items.value.map(i => i.id)) + 1
    items.value.push({id, text, ...adding.value})
    save()
  }
  adding.value = null
  draft.value = ''
}

function cancelAdd() {
  adding.value = null
  draft.value = ''
}

// --- editing / removing ---
const editingId = ref<number | null>(null)
const editText = ref('')

function startEdit(item: Item) {
  adding.value = null
  editingId.value = item.id
  editText.value = item.text
  focusInput(true)
}

function commitEdit(item: Item) {
  const text = editText.value.trim()
  if (text)
    item.text = text
  editingId.value = null
  save()
}

function remove(item: Item) {
  items.value = items.value.filter(i => i.id !== item.id)
  save()
}

function reset() {
  items.value = defaults()
  save()
}

// --- free dragging with pointer events ---
const dragId = ref<number | null>(null)
let grabOffset = {x: 0, y: 0}
let startPoint = {x: 0, y: 0}
let moved = false

function onPointerDown(e: PointerEvent, item: Item) {
  if (e.button !== 0 || editingId.value === item.id)
    return
  dragId.value = item.id
  moved = false
  startPoint = {x: e.clientX, y: e.clientY}
  const p = toPercent(e.clientX, e.clientY)
  grabOffset = {x: item.x - p.x, y: item.y - p.y}
  ;(e.currentTarget as HTMLElement).setPointerCapture(e.pointerId)
}

function onPointerMove(e: PointerEvent, item: Item) {
  if (dragId.value !== item.id)
    return
  if (!moved && Math.hypot(e.clientX - startPoint.x, e.clientY - startPoint.y) < 4)
    return
  moved = true
  const p = toPercent(e.clientX, e.clientY)
  item.x = Math.min(100, Math.max(0, p.x + grabOffset.x))
  item.y = Math.min(100, Math.max(0, p.y + grabOffset.y))
  save()
}

function onPointerUp(e: PointerEvent) {
  if (dragId.value === null)
    return
        ;
  (e.currentTarget as HTMLElement).releasePointerCapture(e.pointerId)
  dragId.value = null
  if (moved)
    save()
}
</script>

<template>
  <div
      ref="canvas"
      class="live-timeline"
      :style="{ height: props.height }"
      @dblclick.self="startAddAt"
  >
    <div class="line"/>

    <div
        v-for="item in items"
        :key="item.id"
        class="node"
        :class="{ core: item.core, dragging: dragId === item.id }"
        :style="{ left: `${item.x}%`, top: `${item.y}%` }"
    >
      <input
          v-if="editingId === item.id"
          ref="inputEl"
          v-model="editText"
          class="card input"
          @keydown.stop
          @keydown.enter="commitEdit(item)"
          @keydown.esc="editingId = null"
          @blur="commitEdit(item)"
      >
      <div
          v-else
          class="card"
          title="Raahaa siirtääksesi, tuplaklikkaa muokataksesi"
          @pointerdown="onPointerDown($event, item)"
          @pointermove="onPointerMove($event, item)"
          @pointerup="onPointerUp"
          @pointercancel="onPointerUp"
          @dblclick.stop="startEdit(item)"
      >
        {{ item.text }}
        <span
            v-if="!item.core"
            class="remove"
            title="Poista"
            @pointerdown.stop
            @click.stop="remove(item)"
        >×</span>
      </div>
    </div>

    <!-- new item being typed -->
    <div v-if="adding" class="node" :style="{ left: `${adding.x}%`, top: `${adding.y}%` }">
      <input
          ref="inputEl"
          v-model="draft"
          class="card input"
          placeholder="Uusi vaihe…"
          @keydown.stop
          @keydown.enter="commitAdd"
          @keydown.esc="cancelAdd"
          @blur="commitAdd"
      >
    </div>

    <div class="controls">
      <button class="plus" title="Lisää uusi (tai tuplaklikkaa tyhjää kohtaa)" @click="startAddDefault">
        +
      </button>
      <button class="reset" title="Palauta alkutilaan" @click="reset">
        ↺
      </button>
    </div>
  </div>
</template>

<style scoped>
.live-timeline {
  position: relative;
  width: 100%;
  user-select: none;
}

.line {
  position: absolute;
  left: 0;
  right: 1.5rem;
  top: 50%;
  height: 3px;
  background: currentColor;
  opacity: 0.35;
  transform: translateY(-50%);
  pointer-events: none;
}

/* arrow head at the right end */
.line::after {
  content: '';
  position: absolute;
  right: -1.5rem;
  top: 50%;
  transform: translateY(-50%);
  border-left: 1.5rem solid currentColor;
  border-top: 0.6rem solid transparent;
  border-bottom: 0.6rem solid transparent;
}

.node {
  position: absolute;
  z-index: 1;
  transform: translate(-50%, -50%);
}

.node.dragging {
  z-index: 10;
}

.card {
  position: relative;
  padding: 0.6rem 0.9rem;
  border: 2px solid currentColor;
  border-radius: 0.5rem;
  background-color: white;
  font-size: 1rem;
  line-height: 1.2;
  text-align: center;
  width: max-content;
  max-width: 11rem;
  cursor: grab;
  touch-action: none;
}

.dragging .card {
  cursor: grabbing;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.2);
}

.core .card {
  font-weight: 700;
  border-width: 3px;
  background-color: #fff6d6;
}

.input {
  width: 10rem;
  outline: none;
  cursor: text;
  user-select: text;
}

.remove {
  position: absolute;
  top: -0.6rem;
  right: -0.6rem;
  width: 1.2rem;
  height: 1.2rem;
  line-height: 1.1rem;
  border-radius: 50%;
  background: #d33;
  color: white;
  font-size: 0.9rem;
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.15s;
}

.card:hover .remove {
  opacity: 1;
}

.controls {
  position: absolute;
  right: 0;
  bottom: 0;
  display: flex;
  gap: 0.5rem;
  opacity: 0.25;
  transition: opacity 0.15s;
}

.live-timeline:hover .controls {
  opacity: 0.8;
}

.plus {
  width: 1.8rem;
  height: 1.8rem;
  border-radius: 50%;
  border: 2px dashed currentColor;
  background-color: white;
  font-weight: 700;
  line-height: 1;
}
</style>

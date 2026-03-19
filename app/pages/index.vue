<template>
  <div class="min-h-screen bg-[#0b0f1a] font-sans antialiased">

    <!-- ─── NAVBAR ─── -->
    <nav class="bg-[#0b0f1a]/80 backdrop-blur-md border-b border-white/5 px-6 py-4 flex items-center justify-between sticky top-0 z-40">
      <a href="#" class="flex items-center gap-2.5">
        <!-- VaultDocs logo mark -->
        <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-blue-500 to-violet-600 flex items-center justify-center shadow-lg shadow-blue-500/20">
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="1" y="2" width="14" height="10" rx="2" stroke="white" stroke-width="1.5" fill="none"/>
            <path d="M4 6h8M4 9h5" stroke="white" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M8 12v2M5 14h6" stroke="white" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
        </div>
        <span class="text-white font-bold text-lg tracking-tight">Vault<span class="text-blue-400">Docs</span></span>
      </a>

      <div class="flex items-center gap-4">
        <a href="#examples" class="text-slate-400 hover:text-slate-200 text-sm transition hidden sm:block">Exchanges</a>
        <a href="#faq" class="text-slate-400 hover:text-slate-200 text-sm transition hidden sm:block">FAQ</a>
        <a
          href="https://stoplight.io/welcome"
          target="_blank"
          class="bg-blue-600 hover:bg-blue-500 text-white text-xs font-bold tracking-wider uppercase px-4 py-2 rounded-lg transition shadow-lg shadow-blue-500/20"
        >
          Get Started
        </a>
      </div>
    </nav>

    <!-- ─── HERO ─── -->
    <section class="relative px-6 pt-24 pb-28 overflow-hidden">
      <!-- Background gradient orbs -->
      <div class="absolute -top-40 -left-40 w-96 h-96 bg-blue-600/10 rounded-full blur-3xl pointer-events-none" />
      <div class="absolute -top-20 right-0 w-80 h-80 bg-violet-600/10 rounded-full blur-3xl pointer-events-none" />

      <div class="max-w-6xl mx-auto flex flex-col lg:flex-row items-center gap-16 relative">

        <!-- Left copy -->
        <div class="flex-1 max-w-xl">
          <div class="inline-flex items-center gap-2 bg-blue-500/10 border border-blue-500/20 text-blue-400 text-xs font-semibold px-3 py-1.5 rounded-full mb-6 tracking-widest uppercase">
            <svg class="w-3 h-3" fill="currentColor" viewBox="0 0 8 8"><circle cx="4" cy="4" r="3"/></svg>
            API Documentation Platform
          </div>
          <h1 class="text-white text-5xl lg:text-6xl font-extrabold leading-[1.1] tracking-tight mb-6">
            API docs that <br/>
            <span class="bg-gradient-to-r from-blue-400 to-violet-400 bg-clip-text text-transparent">just work.</span>
          </h1>
          <p class="text-slate-400 text-base leading-relaxed mb-10 max-w-md">
            Hosted public API documentation for every OAS (Swagger) and RAML spec.
            Preview, host, and share your exchange integrations from a single vault.
          </p>

          <!-- Input + CTA -->
          <div class="flex flex-col sm:flex-row gap-3">
            <input
              v-model="previewUrl"
              type="url"
              placeholder="Paste OpenAPI spec URL…"
              class="flex-1 bg-white/5 border border-white/10 text-slate-200 text-sm px-4 py-3 rounded-xl placeholder-slate-600 focus:outline-none focus:ring-2 focus:ring-blue-500/50 focus:border-blue-500/50 transition"
            />
            <div class="flex gap-2 shrink-0">
              <button
                class="bg-blue-600 hover:bg-blue-500 text-white text-sm font-bold px-5 py-3 rounded-xl transition shadow-lg shadow-blue-500/20"
                @click="handlePreview"
              >
                Preview
              </button>
              <button
                class="bg-white/5 hover:bg-white/10 border border-white/10 text-slate-300 text-sm font-medium px-4 py-3 rounded-xl flex items-center gap-2 transition"
                @click="handleHost"
              >
                <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><rect x="2" y="3" width="20" height="14" rx="2"/><path d="M8 21h8M12 17v4"/></svg>
                Host
              </button>
            </div>
          </div>
        </div>

        <!-- Right mockup -->
        <div class="flex-1 hidden lg:flex justify-end">
          <div class="bg-[#0e1420] rounded-2xl shadow-2xl overflow-hidden w-full max-w-lg border border-white/10">
            <!-- Browser chrome -->
            <div class="bg-[#161c2c] px-4 py-3 flex items-center gap-2 border-b border-white/5">
              <span class="w-2.5 h-2.5 rounded-full bg-red-400/70 inline-block"></span>
              <span class="w-2.5 h-2.5 rounded-full bg-yellow-400/70 inline-block"></span>
              <span class="w-2.5 h-2.5 rounded-full bg-green-400/70 inline-block"></span>
              <div class="flex-1 bg-white/5 rounded-lg text-slate-500 text-xs px-3 py-1 ml-2 border border-white/5">
                vaultdocs.io/exchange/binance
              </div>
            </div>
            <!-- Fake API docs UI -->
            <div class="flex h-64">
              <div class="w-36 bg-[#0b0f1a] border-r border-white/5 p-3 text-xs">
                <div class="text-slate-500 font-semibold mb-2 text-[10px] uppercase tracking-wider">Binance API</div>
                <div class="space-y-1">
                  <div class="bg-blue-500/20 text-blue-400 rounded px-2 py-1 font-medium">Spot</div>
                  <div class="text-slate-500 hover:bg-white/5 rounded px-2 py-1 cursor-pointer">Futures</div>
                  <div class="text-slate-500 hover:bg-white/5 rounded px-2 py-1 cursor-pointer">Wallet</div>
                </div>
                <div class="mt-4 space-y-1.5 text-[10px]">
                  <div class="flex items-center gap-1">
                    <span class="bg-green-500/80 text-white rounded px-1 font-bold">GET</span>
                    <span class="text-slate-500">/ticker</span>
                  </div>
                  <div class="flex items-center gap-1">
                    <span class="bg-blue-500/80 text-white rounded px-1 font-bold">POST</span>
                    <span class="text-slate-500">/order</span>
                  </div>
                  <div class="flex items-center gap-1">
                    <span class="bg-red-500/80 text-white rounded px-1 font-bold">DEL</span>
                    <span class="text-slate-500">/order</span>
                  </div>
                </div>
              </div>
              <div class="flex-1 p-4">
                <div class="flex items-center gap-2 mb-3">
                  <span class="bg-green-500/80 text-white text-xs font-bold rounded px-2 py-0.5">GET</span>
                  <span class="text-slate-200 text-sm font-semibold">24hr Ticker Price</span>
                </div>
                <p class="text-slate-500 text-xs mb-3 leading-relaxed">Returns 24 hour rolling window price change statistics.</p>
                <div class="text-xs font-semibold text-slate-400 mb-1.5">Parameters</div>
                <div class="bg-white/[0.03] border border-white/5 rounded-lg p-2 text-xs text-slate-500 space-y-1">
                  <div><span class="text-blue-400">symbol</span> <span class="text-slate-600">string · required</span></div>
                  <div><span class="text-blue-400">type</span> <span class="text-slate-600">enum</span></div>
                </div>
                <div class="flex gap-1 mt-3 text-[10px]">
                  <span class="border border-emerald-500/30 text-emerald-400 rounded px-1.5 py-0.5">200</span>
                  <span class="border border-red-500/20 text-red-400/70 rounded px-1.5 py-0.5">400</span>
                </div>
              </div>
            </div>
          </div>
        </div>

      </div>
    </section>

    <!-- ─── WHAT IS VAULTDOCS ─── -->
    <section class="py-20 px-6 bg-[#0e1420] border-y border-white/5">
      <div class="max-w-4xl mx-auto">
        <h2 class="text-3xl font-bold text-white mb-4">What is VaultDocs?</h2>
        <p class="text-slate-400 text-base leading-relaxed">
          <span class="text-blue-400 font-semibold">VaultDocs</span> is a free platform that hosts API documentation
          and includes a built-in API design tool to simplify editing OpenAPI spec files. Connect your exchange, preview
          your spec, and share production-ready docs with your team — all in one secure vault.
          Powered by
          <a href="https://stoplight.io" target="_blank" class="text-blue-400 hover:text-blue-300 underline underline-offset-2 transition">VaultDocs</a>.
        </p>
      </div>
    </section>

    <!-- ─── EXCHANGE GRID ─── -->
    <div id="examples">
      <ExchangeGrid @connect="onConnect" />
    </div>

    <!-- ─── FAQ ─── -->
    <section id="faq" class="py-20 px-6 bg-[#0e1420] border-t border-white/5">
      <div class="max-w-4xl mx-auto">
        <h2 class="text-3xl font-bold text-white mb-10">Frequently asked questions</h2>
        <div class="space-y-6">

          <FaqItem
            question="How much does VaultDocs cost?"
            answer='VaultDocs is free for public API documentation. You can set up a workspace and generate OpenAPI documentation in minutes. For self-hosting, use the open source tool <a href="https://stoplight.io/open-source/elements" target="_blank" class="text-blue-400 hover:text-blue-300 underline">Elements</a>.'
          />
          <FaqItem
            question="Which specification formats are supported?"
            answer="OAS 3.1 / 3.0 / 2.0 (Swagger). For other formats please use a transformer first."
          />
          <FaqItem
            question="Can I use a custom domain?"
            answer='This is a preview powered by <a href="https://stoplight.io/open-source/elements" target="_blank" class="text-blue-400 hover:text-blue-300 underline">VaultDocs Elements</a>. Create a workspace to configure a custom domain.'
          />
          <FaqItem
            question="Can I edit my docs?"
            answer='Not directly on VaultDocs — this is a preview tool. Edit OpenAPI files directly from the <a href="https://stoplight.io" target="_blank" class="text-blue-400 hover:text-blue-300 underline">VaultDocs platform</a> when you create a workspace.'
          />
          <FaqItem
            question="Who built this?"
            answer='VaultDocs is built on top of the tools from <a href="https://stoplight.io" target="_blank" class="text-blue-400 hover:text-blue-300 underline">VaultDocs</a> — helping teams design, build, and document better APIs, faster.'
          />
          <FaqItem
            question="I have more questions!"
            answer='Visit <a href="https://community.stoplight.io" target="_blank" class="text-blue-400 hover:text-blue-300 underline">community.vaultdocs.io</a> on Discord to start a conversation and get help from the community.'
          />

        </div>
      </div>
    </section>

    <!-- ─── FOOTER ─── -->
    <footer class="bg-[#0b0f1a] border-t border-white/5 py-14 px-6 text-center">
      <div class="flex items-center justify-center gap-2.5 mb-4">
        <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-blue-500 to-violet-600 flex items-center justify-center shadow shadow-blue-500/20">
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="1" y="2" width="14" height="10" rx="2" stroke="white" stroke-width="1.5" fill="none"/>
            <path d="M4 6h8M4 9h5" stroke="white" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M8 12v2M5 14h6" stroke="white" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
        </div>
        <span class="text-white font-bold text-xl tracking-tight">Vault<span class="text-blue-400">Docs</span></span>
      </div>
      <p class="text-slate-500 text-sm mb-6">Powered by VaultDocs · Open, secure, and always free.</p>
      <a
        href="https://stoplight.io/welcome"
        target="_blank"
        class="inline-block bg-blue-600 hover:bg-blue-500 text-white text-sm font-bold tracking-wider uppercase px-6 py-3 rounded-xl transition shadow-lg shadow-blue-500/20 mb-8"
      >
        Host on VaultDocs →
      </a>
      <p class="text-slate-600 text-xs">
        Document, mock, test, and more — with the VaultDocs API Designer.
      </p>
    </footer>

  </div>
</template>

<script setup lang="ts">
// Page meta — title and description
useHead({
  title: 'VaultDocs — Secure API Documentation for Crypto Exchanges',
  meta: [
    {
      name: 'description',
      content:
        'VaultDocs is a free API documentation platform for crypto exchanges. Connect Binance, Bybit, OKX, KuCoin and 50+ more — preview, host, and share OpenAPI specs from a single secure vault.',
    },
    { name: 'og:title', content: 'VaultDocs — Secure API Documentation for Crypto Exchanges' },
    {
      name: 'og:description',
      content:
        'Connect to 50+ crypto exchanges and host API documentation instantly. Built on OpenAPI / Swagger. Powered by VaultDocs.',
    },
    { name: 'og:type', content: 'website' },
    { name: 'twitter:card', content: 'summary_large_image' },
    { name: 'twitter:title', content: 'VaultDocs — API Docs for Crypto Exchanges' },
    {
      name: 'twitter:description',
      content: 'Preview and host OpenAPI documentation for 50+ crypto exchanges. Free, secure, and instant.',
    },
  ],
})

const previewUrl = ref('')

const handlePreview = () => {
  if (!previewUrl.value) return
  alert(`Previewing: ${previewUrl.value}`)
}

const onConnect = (payload: any) => {
  console.log('Connected exchange:', payload.exchange, payload)
}

const handleHost = () => {
  window.open('https://stoplight.io/welcome', '_blank')
}
</script>
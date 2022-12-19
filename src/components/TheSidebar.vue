<template>
  <OnClickOutside @trigger="isExpanded = false">
    <aside>
      <div class="sm:hidden" :style="{ height: '57px' }"></div>
      <div
        class="fixed bottom-0 inset-x-0 border-t border-slate-700 sm:border-t-0 pwa:sm:border-t pwa:sm:border-t-slate-900 pwa:sm:shadow-[inset_0_1px_0_rgb(30_30_37)] sm:border-r content-start transition-[height] sm:transition-none sm:!h-screen sm:w-[240px] sm:static bg-slate-800 sm:overflow-auto"
        :style="{
          height: isExpanded ? `${expandableContentHeight + 57}px` : `57px`,
        }"
      >
        <div ref="expandableContent">
          <div class="grid gap-y-5 px-3 py-4">
            <div class="grid gap-y-2 justify-start">
              <label class="font-semibold text-xs">Theme</label>
              <div class="grid items-center gap-2 grid-cols-7">
                <button
                  v-for="theme in themes"
                  @click="setTheme(theme, $event)"
                  class="group rounded-full focus:outline-none"
                  :title="`Use ${theme.name} theme`"
                >
                  <div
                    class="w-6 h-6 rounded-full group-hover:opacity-100 transition group-hover:scale-105 group-active:scale-95 group-focus:shadow-[inset_0_0_0_1px_rgba(255,255,255,.21)] group-focus:ring-[3px] ring-blue-800"
                    :class="{
                      'opacity-50': store.currentTheme !== theme.key,
                    }"
                    :style="{ background: theme.background }"
                  ></div>
                </button>
              </div>
            </div>

            <div class="grid gap-y-2">
              <div class="grid gap-y-1">
                <label for="language" class="font-semibold text-xs">Language</label>
                <BaseSelect
                  id="language"
                  :model-value="store.language"
                  @update:model-value="setLanguage"
                  :options="AVAILABLE_LANGUAGES"
                />
              </div>

              <div class="grid gap-y-1">
                <label for="windowControls" class="font-semibold text-xs">Window controls</label>
                <BaseSelect
                  id="windowControls"
                  :model-value="store.windowControls"
                  @update:model-value="store.windowControls = $event"
                  :options="[
                    { label: 'None', value: WindowControls.None },
                    { label: 'macOS - Color', value: WindowControls.MacColor },
                    { label: 'macOS - Gray', value: WindowControls.MacGray },
                    { label: 'macOS - Outline', value: WindowControls.MacOutline },
                  ]"
                />
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center grid-cols-[1fr_auto_auto] gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="showTwitterBadge"
                  >Twitter Badge</label
                >
                <BaseButton
                  class="text-blue-500 px-2.5 font-semibold text-xs bg-blue-600/30 hover:bg-blue-600/40 h-5 rounded"
                  @click="store.expandTwitterOptions = !store.expandTwitterOptions"
                >
                  <IconChevronDown
                    width="12"
                    class="transition-transform"
                    :class="{
                      'rotate-180': store.expandTwitterOptions,
                    }"
                  />
                </BaseButton>
                <BaseSwitch v-model="store.showTwitterBadge" id="showTwitterBadge" />
              </div>

              <div class="grid gap-y-1 gap-x-2 items-start grid-cols-[auto_1fr]" v-if="store.expandTwitterOptions">
                <div v-if="store.picture" class="row-start-1 row-end-3 relative group">
                  <BaseButton
                    @click="store.picture = ''"
                    class="h-5 w-5 absolute right-0 top-0 group-hover:opacity-100 opacity-0 bg-red-600/80 hover:bg-red-600 transition rounded-full justify-center text-white"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      xmlns:xlink="http://www.w3.org/1999/xlink"
                      aria-hidden="true"
                      role="img"
                      width="16"
                      height="16"
                      preserveAspectRatio="xMidYMid meet"
                      viewBox="0 0 20 20"
                    >
                      <g fill="currentColor">
                        <path
                          d="M7.172 14.243a1 1 0 1 1-1.415-1.415l7.071-7.07a1 1 0 0 1 1.415 1.414l-7.071 7.07Z"
                        ></path>
                        <path
                          d="M5.757 7.172a1 1 0 1 1 1.415-1.415l7.07 7.071a1 1 0 0 1-1.414 1.415l-7.07-7.071Z"
                        ></path>
                      </g>
                    </svg>
                  </BaseButton>
                  <img
                    :src="store.picture"
                    alt=""
                    class="w-14 h-14 border
                    rounded-full"
                  />
                </div>
                <label
                  v-else
                  class="w-14 h-14 border border-slate-700 bg-slate-700/30 hover:bg-slate-700/50 text-slate-600 hover:text-slate-400 transition-colors cursor-pointer rounded-full flex items-center justify-center row-start-1 row-end-3"
                >
                  <input type="file" class="sr-only" @change="handlePicture" />
                  <svg version="1.0" xmlns="http://www.w3.org/2000/svg"
 width="24" height="24" viewBox="0 0 1280.000000 1280.000000"
 preserveAspectRatio="xMidYMid meet">

<g transform="translate(0.000000,1280.000000) scale(0.100000,-0.100000)"
fill="#000000" stroke="none">
<path d="M6340 11390 c-122 -16 -203 -46 -301 -111 -30 -21 -79 -63 -108 -95
-111 -121 -163 -255 -165 -429 -1 -125 11 -187 60 -293 43 -95 115 -184 189
-237 141 -100 221 -122 495 -135 339 -16 489 -37 785 -107 148 -35 382 -108
461 -144 22 -10 64 -27 93 -39 29 -11 105 -46 169 -79 338 -168 627 -377 895
-647 599 -601 971 -1398 1057 -2265 14 -140 14 -558 0 -679 -36 -320 -81 -534
-169 -792 -229 -670 -609 -1192 -1211 -1660 -156 -122 -211 -180 -268 -288
-57 -107 -76 -196 -69 -323 9 -161 63 -287 175 -408 109 -117 228 -184 376
-210 182 -31 358 24 561 179 538 407 949 862 1276 1411 88 147 242 458 307
621 163 407 269 844 323 1335 17 145 17 755 1 900 -78 705 -235 1252 -523
1828 -83 166 -97 192 -191 350 -414 696 -1035 1306 -1733 1701 -133 76 -401
207 -510 251 -105 41 -105 41 -179 69 -188 73 -548 173 -741 206 -66 12 -225
38 -300 50 -178 28 -655 54 -755 40z"/>
<path d="M3800 10576 c-134 -29 -246 -93 -455 -261 -145 -116 -350 -305 -423
-389 -62 -71 -125 -193 -146 -286 -49 -206 17 -422 178 -584 90 -91 159 -135
268 -173 103 -37 270 -38 373 -4 135 44 181 76 392 266 120 108 131 117 265
223 64 51 136 114 160 141 232 261 214 638 -43 893 -154 152 -369 218 -569
174z"/>
<path d="M6070 9041 c-275 -39 -530 -116 -774 -234 -274 -133 -474 -275 -692
-492 -149 -149 -231 -249 -336 -410 -343 -524 -480 -1148 -388 -1762 66 -440
239 -842 519 -1203 101 -130 330 -355 468 -458 453 -338 978 -513 1542 -513
716 1 1394 296 1873 816 440 479 678 1085 678 1730 0 675 -265 1316 -741 1793
-389 391 -881 640 -1434 728 -161 25 -547 28 -715 5z m-40 -2486 c0 -232 4
-395 9 -395 5 0 25 29 45 64 l36 64 0 331 0 331 35 0 35 0 0 -255 c0 -140 3
-255 8 -255 4 0 68 114 144 253 l136 252 36 3 c20 2 36 0 36 -3 0 -7 -29 -61
-267 -495 -77 -140 -167 -303 -199 -362 -56 -103 -61 -108 -91 -108 l-33 0 0
485 0 485 35 0 35 0 0 -395z m639 298 c-168 -311 -209 -388 -209 -397 0 -5 11
-28 24 -50 39 -68 226 -415 226 -421 0 -3 -15 -5 -34 -5 -37 0 -32 -7 -205
313 -23 42 -48 77 -55 77 -7 0 -20 -16 -29 -36 -17 -35 -17 -37 7 -79 37 -62
146 -265 146 -271 0 -3 -16 -4 -36 -2 -35 3 -38 7 -99 118 -35 63 -68 116 -73
118 -5 1 -38 -52 -73 -118 l-64 -120 -38 0 -39 0 43 78 c60 107 211 385 359
660 l125 231 38 1 39 0 -53 -97z m-939 27 c89 -32 159 -102 185 -185 25 -78
25 -208 1 -290 -14 -49 -28 -70 -74 -115 -85 -82 -125 -93 -392 -99 l-215 -6
-3 -85 c-4 -113 -11 -120 -114 -120 -128 0 -118 -38 -118 465 0 418 0 425 21
446 21 21 27 21 332 17 296 -5 315 -6 377 -28z m1835 6 c206 -57 289 -152 315
-358 19 -160 -16 -317 -91 -402 -53 -60 -107 -91 -205 -117 -69 -19 -114 -22
-342 -27 -241 -4 -265 -3 -283 13 -18 17 -19 35 -19 451 l0 433 23 16 c19 15
52 16 282 12 200 -4 274 -9 320 -21z"/>
<path d="M5232 6548 l3 -183 165 0 c184 0 225 9 264 56 52 61 57 174 12 241
-35 54 -92 67 -284 68 l-162 0 2 -182z"/>
<path d="M7170 6446 l0 -286 26 -6 c39 -10 252 5 310 21 63 18 117 74 135 137
13 49 16 183 5 245 -11 60 -58 121 -114 146 -40 17 -72 21 -204 25 l-158 4 0
-286z"/>
<path d="M2094 7815 c-274 -63 -467 -266 -513 -540 -42 -251 -56 -435 -56
-755 0 -334 11 -478 56 -757 24 -149 80 -261 182 -365 64 -65 160 -127 241
-156 56 -20 172 -42 221 -42 50 0 165 22 228 44 177 62 329 220 392 407 24 72
28 100 28 199 0 63 -5 147 -11 185 -20 120 -32 302 -32 490 0 164 9 283 40
555 6 52 8 124 4 160 -27 268 -221 497 -479 566 -83 22 -226 26 -301 9z"/>
<path d="M3340 4174 c-236 -27 -444 -189 -532 -414 -55 -140 -60 -280 -13
-427 38 -120 85 -189 219 -322 502 -495 1176 -906 1836 -1119 367 -119 588
-169 940 -216 268 -36 645 -46 765 -21 236 49 429 240 494 490 18 69 13 232
-9 314 -62 223 -225 388 -450 456 -37 11 -123 19 -260 26 -484 22 -827 92
-1235 252 -443 173 -823 420 -1205 783 -88 83 -172 135 -266 166 -78 26 -209
40 -284 32z"/>
</g>
</svg>
                </label>

                <div class="grid gap-y-1 col-start-2">
                  <BaseInput
                    class="placeholder:text-slate-600/75"
                    type="text"
                    id="name"
                    autocomplete="off"
                    spellcheck="false"
                    placeholder="Name"
                    v-model="store.name"
                  />
                </div>

                <div class="grid gap-y-1 col-start-2">
                  <BaseInput
                    class="placeholder:text-slate-600/75"
                    type="text"
                    id="username"
                    autocomplete="off"
                    spellcheck="false"
                    placeholder="Username"
                    v-model="store.username"
                  />
                </div>
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="showLineNumbers"
                  >Line numbers</label
                >
                <BaseSwitch v-model="store.showLineNumbers" id="showLineNumbers" />
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="showBackground">Background</label>
                <BaseSwitch v-model="store.showBackground" id="showBackground" />
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="reflection">Reflection</label>
                <BaseSwitch v-model="store.reflection" id="reflection" />
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="paddingX">Padding X</label>
                <div class="grid gap-x-2 grid-flow-col text-sm">
                  <input
                    id="paddingX"
                    class="accent-blue-700"
                    type="range"
                    min="32"
                    max="256"
                    step="8"
                    :value="store.paddingX"
                    @input="store.paddingX = parseInt(($event.target as HTMLInputElement).value)"
                  />
                </div>
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="paddingY">Padding Y</label>
                <div class="grid gap-x-2 grid-flow-col text-sm">
                  <input
                    id="paddingY"
                    class="accent-blue-700"
                    type="range"
                    min="32"
                    max="128"
                    step="8"
                    :value="store.paddingY"
                    @input="store.paddingY = parseInt(($event.target as HTMLInputElement).value)"
                  />
                </div>
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="diff">Diff</label>
                <BaseSwitch v-model="store.diff" id="diff" />
              </div>
            </div>
          </div>
        </div>

        <div
          class="grid grid-cols-[1fr_auto_1fr] sm:grid-cols-1 gap-2 fixed inset-x-0 bottom-0 py-2 px-3 bg-slate-800 sm:static sm:bg-transparent sm:px-3 sm:py-0"
        >
          <label class="font-semibold text-xs hidden sm:block">Export</label>
          <BaseButton
            class="px-4 w-full bg-blue-600/30 text-blue-500 hover:bg-blue-600/40 group"
            @click="handleCopyLink"
          >
            <IconClipboardLink width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
            <span class="truncate">
              {{ exportState === ExportState.JustCopiedLink ? "Copied!" : "Copy Link to Clipboard" }}
            </span>
          </BaseButton>
          <BaseButton
            class="px-4 w-full hidden sm:flex bg-emerald-600/30 text-emerald-500 hover:bg-emerald-600/40 group"
            @click="handleCopy"
          >
            <IconClipboard width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
            <span class="truncate">
              {{
                exportState === ExportState.PreparingToCopy
                  ? "..."
                  : exportState === ExportState.JustCopied
                  ? "Copied!"
                  : exportState === ExportState.CopyFailure
                  ? "Error! Try to download"
                  : "Copy Image to Clipboard"
              }}
            </span>
          </BaseButton>
          <BaseButton
            class="bg-slate-700 text-slate-500 hover:bg-slate-700/80 group sm:hidden w-10 justify-center"
            @click="isExpanded = !isExpanded"
            square="w-10"
          >
            <IconChevronDown
              width="16"
              class="transition"
              :class="{
                'rotate-180': !isExpanded,
              }"
            />
          </BaseButton>
          <BaseButton
            class="px-4 w-full bg-rose-500/30 text-rose-300 hover:bg-rose-500/40 group"
            @click="handleDownload"
          >
            <IconDownload width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
            <span class="truncate">
              {{
                exportState === ExportState.PreparingToDownload
                  ? "..."
                  : exportState === ExportState.JustDownloaded
                  ? "Downloaded!"
                  : "Download PNG"
              }}
            </span>
          </BaseButton>
        </div>

        <div
          class="sm:grid grid-cols-[1fr_auto_1fr] sm:grid-cols-1 gap-2 bg-slate-800 hidden sm:static sm:bg-transparent sm:px-3 sm:py-0 mt-4"
        >
          <div class="grid grid-flow-col gap-y-2 items-center grid-cols-[1fr_auto] gap-x-2">
            <label
              class="font-semibold text-xs hidden sm:block"
              :class="{
                'opacity-0': !store.expandSupportSection,
              }"
              >Support</label
            >
            <BaseButton
              class="px-2.5 font-semibold text-xs hover:bg-blue-600/40 h-5 rounded"
              :class="{
                'bg-slate-700 ': !store.expandSupportSection,
                'bg-blue-600/30 text-blue-500': store.expandSupportSection,
              }"
              @click="store.expandSupportSection = !store.expandSupportSection"
            >
              <IconChevronDown
                width="12"
                class="transition-transform"
                :class="{
                  'rotate-180': store.expandSupportSection,
                }"
              />
            </BaseButton>
          </div>

          <div class="grid gap-y-2" v-if="store.expandSupportSection">
            <BaseButton
              is="a"
              class="px-4 w-full border border-[#1da1f2]/10 text-[#1da1f2]/90 hover:border-[#1da1f2]/40 group"
              href="https://twitter.com/intent/follow?screen_name=Idered"
              target="_blank"
            >
              <IconTwitter width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
              <span class="truncate text-slate-600">Follow on Twitter</span>
            </BaseButton>
            <BaseButton
              is="a"
              class="px-4 w-full border border-yellow-500/10 text-yellow-300/80 hover:border-yellow-400/40 hover:text- group"
              href="https://www.buymeacoffee.com/idered"
              target="_blank"
            >
              <IconCoffee width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
              <span class="truncate text-slate-600">Buy me a coffee</span>
            </BaseButton>
            <label
              class="font-semibold text-xs hidden sm:block mt-1"
              :class="{
                'opacity-0': !store.expandSupportSection,
              }"
              >Learn</label
            >
            <BaseButton
              is="a"
              class="px-4 w-full border border-slate-700 text-slate-600 hover:border-slate-600/40 group"
              href="https://github.com/Idered/chalk.ist"
              target="_blank"
            >
              <IconGithub width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
              <span class="truncate">View on GitHub</span>
            </BaseButton>
            <BaseButton
              is="a"
              class="px-4 w-full border border-slate-700 text-slate-600 hover:border-slate-600/40 group"
              href="https://umami.kasper.io/share/WCDyKkOU/chalk.ist"
              target="_blank"
            >
              <IconAnalytics width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
              <span class="truncate">View Analytics</span>
            </BaseButton>

            <div class="text-xs hidden sm:block mt-2">
              <span class="opacity-75">Powered By</span>
              <a
                href="https://www.techypranav.eu.org/"
                class="hover:text-white transition outline-none font-medium focus:text-white"
              >
                PRANAV K DILEEP
              </a>
            </div>
          </div>
        </div>
      </div>
    </aside>
  </OnClickOutside>
</template>

<script setup lang="ts">
import { nextTick, ref } from "vue";
import { OnClickOutside } from "@vueuse/components";
import { isExporting, store } from "~/composables/store";
import * as themes from "~/themes";
import BaseSwitch from "./BaseSwitch.vue";
import BaseSelect from "./BaseSelect.vue";
import { AVAILABLE_LANGUAGES } from "~/constants";
import BaseInput from "./BaseInput.vue";
import BaseButton from "./BaseButton.vue";
import IconDownload from "./IconDownload.vue";
import IconClipboard from "./IconClipboard.vue";
import IconChevronDown from "./IconChevronDown.vue";
import { useElementSize } from "@vueuse/core";
import { Theme } from "~/composables/theme-utils";
import { exportState, ExportState } from "~/composables/export-state";
import IconClipboardLink from "./IconClipboardLink.vue";
import { resizeImage, cropImage } from "~/composables/image";
import IconCoffee from "./IconCoffee.vue";
import IconTwitter from "./IconTwitter.vue";
import IconGithub from "./IconGithub.vue";
import IconAnalytics from "./IconAnalytics.vue";
import { WindowControls } from "~/types";
import * as htmlToImage from "html-to-image";

const isExpanded = ref(false);
const timeout = ref();
const expandableContent = ref();
const { height: expandableContentHeight } = useElementSize(expandableContent);

// const fontEmbedCSS = ref("");

// onMounted(async () => {
//   const frame = document.querySelector<HTMLDivElement>("[data-editor-frame]");
//   if (!frame) return;
//   fontEmbedCSS.value = await htmlToImage.getFontEmbedCSS(frame);
// });

const downloadPng = (blob: Blob | null) => {
  if (!blob) return;
  const url = URL.createObjectURL(blob);
  const link = document.createElement("a");
  link.href = url;
  link.download = "screenshot.png";
  link.click();
  exportState.value = ExportState.JustDownloaded;
  clearTimeout(timeout.value);
  timeout.value = setTimeout(() => {
    exportState.value = ExportState.Idle;
  }, 1000);
};

const copyToClipboard = (blob: Blob | null) => {
  if (!blob) return;
  try {
    const item = new ClipboardItem({ "image/png": blob });
    navigator.clipboard.write([item]);
    exportState.value = ExportState.JustCopied;
    clearTimeout(timeout.value);
    timeout.value = setTimeout(() => {
      exportState.value = ExportState.Idle;
    }, 1000);
  } catch {
    exportState.value = ExportState.CopyFailure;
    clearTimeout(timeout.value);
    timeout.value = setTimeout(() => {
      exportState.value = ExportState.Idle;
    }, 1000);
  }
};

const handleCopy = async () => {
  const frame = document.querySelector<HTMLDivElement>("[data-editor-frame]");
  if (!frame) return;
  umami.trackEvent("Copy to Clipboard", "export");
  exportState.value = ExportState.PreparingToCopy;
  isExporting.value = true;
  await nextTick();
  htmlToImage
    .toBlob(frame, {
      // fontEmbedCSS: fontEmbedCSS.value,
    })
    .then(function (blob) {
      isExporting.value = false;
      copyToClipboard(blob);
    });
};

const handleCopyLink = async () => {
  const frame = document.querySelector<HTMLDivElement>("[data-editor-frame]");
  if (!frame) return;
  umami.trackEvent("Copy Link", "export");

  // copy location.href to clipboard
  const { content } = store.value;
  const str = window.btoa(
    JSON.stringify({
      c: encodeURIComponent(content),
      t: store.value.currentTheme,
      tt: store.value.title,
      l: store.value.language,
      px: store.value.paddingX,
      py: store.value.paddingY,
      w: store.value.frameWidth,
      n: store.value.name,
      u: store.value.username,
      b: store.value.showTwitterBadge,
      r: store.value.reflection,
      ln: store.value.showLineNumbers,
      wc: store.value.windowControls,
    })
  );
  const url = `${window.location.origin}/share/${str}`;
  navigator.clipboard.writeText(url);

  exportState.value = ExportState.JustCopiedLink;
  clearTimeout(timeout.value);
  timeout.value = setTimeout(() => {
    exportState.value = ExportState.Idle;
  }, 1000);
};

const handleDownload = async () => {
  const frame = document.querySelector<HTMLDivElement>("[data-editor-frame]");
  if (!frame) return;
  umami.trackEvent("Download PNG", "export");
  exportState.value = ExportState.PreparingToDownload;
  isExporting.value = true;
  await nextTick();
  htmlToImage.toBlob(frame).then(function (blob) {
    isExporting.value = false;
    downloadPng(blob);
  });
};

function handlePicture(event: Event) {
  const target = event.target as HTMLInputElement;
  if (target.files) {
    const file = target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = async (e) => {
      const croppedImage = await cropImage(reader.result as string, 1);
      const resizedImage = await resizeImage(croppedImage, 56 * 2);
      store.value.picture = resizedImage.toDataURL(file.type);
    };
    reader.readAsDataURL(file);
  }
}

function setTheme(theme: Theme, event: MouseEvent) {
  store.value.currentTheme = theme.key;
  store.value.useAltBackground = event.altKey;
  umami.trackEvent(store.value.currentTheme, "theme");
}

function setLanguage(language: string) {
  store.value.language = language;
  umami.trackEvent(store.value.language, "language");
}
</script>

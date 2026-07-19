<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<div class="_gaps_m">
	<div :class="$style.banner" :style="{ backgroundImage: instance.bannerUrl ? `url(${ instance.bannerUrl })` : undefined }">
		<div style="overflow: clip;">
			<img :src="instance.iconUrl ?? '/favicon.ico'" alt="" :class="$style.bannerIcon"/>
			<div :class="$style.bannerName">
				<b>{{ instance.name ?? host }}</b>
			</div>
		</div>
	</div>

	<MkKeyValue>
		<template #key>{{ i18n.ts.description }}</template>
		<template #value><div v-html="instance.description"></div></template>
	</MkKeyValue>

	<FormSection>
		<div class="_gaps_m">
			<FormSplit>
				<MkKeyValue :copy="version">
					<template #key>Misskey</template>
					<template #value>{{ version }}</template>
				</MkKeyValue>
				<MkKeyValue :copy="instance.maintainerName">
					<template #key>{{ i18n.ts.administrator }}</template>
					<template #value>
						<template v-if="instance.maintainerName">{{ instance.maintainerName }}</template>
						<span v-else style="opacity: 0.7;">({{ i18n.ts.none }})</span>
					</template>
				</MkKeyValue>
				<MkKeyValue :copy="instance.maintainerEmail">
					<template #key>{{ i18n.ts.contact }}</template>
					<template #value>
						<template v-if="instance.maintainerEmail">{{ instance.maintainerEmail }}</template>
						<span v-else style="opacity: 0.7;">({{ i18n.ts.none }})</span>
					</template>
				</MkKeyValue>
				<MkKeyValue>
					<template #key>{{ i18n.ts.inquiry }}</template>
					<template #value>
						<MkLink v-if="instance.inquiryUrl" :url="instance.inquiryUrl" target="_blank">{{ instance.inquiryUrl }}</MkLink>
						<span v-else style="opacity: 0.7;">({{ i18n.ts.none }})</span>
					</template>
				</MkKeyValue>
			</FormSplit>
			<div class="_gaps_s">
				<FormLink to="/about-misskey">
					<template #icon><i class="ti ti-info-circle"></i></template>
					{{ i18n.ts.aboutMisskey }}
				</FormLink>
				<FormLink v-if="instance.impressumUrl" :to="instance.impressumUrl" external>
					<template #icon><i class="ti ti-user-shield"></i></template>
					<template #default>{{ i18n.ts.impressum }}</template>
				</FormLink>
				<FormLink to="/@7261230/pages/term-of-services">
					<template #icon><i class="ti ti-license"></i></template>
					<template #default>{{ i18n.ts.serverRules }}</template>
				</FormLink>
				<FormLink to="/channels/an05k2kq5knq00ap">
					<template #icon><i class="ti ti-message"></i></template>
					<template #default>{{ i18n.ts.feedback }}</template>
				</FormLink>
				<FormLink to="https://yhfx.jwznb.com/share?key=UGHUaiFnRlLU&ts=1780400878" external>
					<template #icon><i class="ti ti-messages"></i></template>
					交流群
				</FormLink>
				<FormLink to="/@7261230/pages/opensource">
					<template #icon><i class="ti ti-code"></i></template>
					{{ i18n.ts.sourceCode }}
				</FormLink>
			</div>
		</div>
	</FormSection>

	<MkSuspense v-slot="{ result: stats }" :p="initStats">
		<FormSection>
			<template #label>{{ i18n.ts.statistics }}</template>
			<FormSplit>
				<MkKeyValue>
					<template #key>{{ i18n.ts.users }}</template>
					<template #value>{{ number(stats.originalUsersCount) }}</template>
				</MkKeyValue>
				<MkKeyValue>
					<template #key>{{ i18n.ts.notes }}</template>
					<template #value>{{ number(stats.originalNotesCount) }}</template>
				</MkKeyValue>
			</FormSplit>
		</FormSection>
	</MkSuspense>
</div>
</template>

<script lang="ts" setup>
import { host, version } from '@@/js/config.js';
import { i18n } from '@/i18n.js';
import { instance } from '@/instance.js';
import number from '@/filters/number.js';
import { misskeyApi } from '@/utility/misskey-api.js';
import FormLink from '@/components/form/link.vue';
import FormSection from '@/components/form/section.vue';
import FormSplit from '@/components/form/split.vue';
import MkFolder from '@/components/MkFolder.vue';
import MkKeyValue from '@/components/MkKeyValue.vue';
import MkLink from '@/components/MkLink.vue';
import MkInfo from '@/components/MkInfo.vue';

const initStats = () => misskeyApi('stats', {});
</script>

<style lang="scss" module>
.banner {
	text-align: center;
	border-radius: 10px;
	overflow: clip;
	background-color: var(--MI_THEME-panel);
	background-size: cover;
	background-position: center center;
}

.bannerIcon {
	display: block;
	margin: 16px auto 0 auto;
	height: 64px;
	border-radius: 8px;
}

.bannerName {
	display: block;
	padding: 16px;
	color: #fff;
	text-shadow: 0 0 8px #000;
	background: linear-gradient(transparent, rgba(0, 0, 0, 0.7));
}

.rules {
	counter-reset: item;
	list-style: none;
	padding: 0;
	margin: 0;
}

.rule {
	display: flex;
	gap: 8px;
	word-break: break-word;

	&::before {
		flex-shrink: 0;
		display: flex;
		position: sticky;
		top: calc(var(--MI-stickyTop, 0px) + 8px);
		counter-increment: item;
		content: counter(item);
		width: 32px;
		height: 32px;
		line-height: 32px;
		background-color: var(--MI_THEME-accentedBg);
		color: var(--MI_THEME-accent);
		font-size: 13px;
		font-weight: bold;
		align-items: center;
		justify-content: center;
		border-radius: 999px;
	}
}

.ruleText {
	padding-top: 6px;
}
</style>

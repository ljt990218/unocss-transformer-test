<script setup lang="ts">
import type { PickerColumn } from 'vant'
import useAppStore from '@/stores/modules/app'
import { languageColumns, locale } from '@/utils/i18n'

definePage({
  name: 'home',
  meta: {
    level: 1,
  },
})

const appStore = useAppStore()
const checked = ref<boolean>(isDark.value)

watch(
  () => isDark.value,
  (newMode) => {
    checked.value = newMode
  },
  { immediate: true },
)

function toggle() {
  toggleDark()
  appStore.swithMode(isDark.value ? 'dark' : 'light')
}

const { t } = useI18n()

const showLanguagePicker = ref(false)
const languageValues = ref<Array<string>>([locale.value])
const language = computed(() => languageColumns.find(l => l.value === locale.value).text)

function onLanguageConfirm(event: { selectedOptions: PickerColumn }) {
  locale.value = event.selectedOptions[0].value as string
  showLanguagePicker.value = false
}

const menuItems = computed(() => ([
  { title: t('home.mockGuide'), route: 'mock' },
  { title: t('home.404Demo'), route: 'unknown' },
]))
</script>

<template>
  <Container paddingT="46" paddingX="0">
    <VanCellGroup inset>
      <VanCell center :title="t('home.darkMode')">
        <template #right-icon>
          <VanSwitch v-model="checked" size="20px" aria-label="on/off Dark Mode" @click="toggle()" />
        </template>
      </VanCell>

      <VanCell is-link :title="t('home.language')" :value="language" @click="showLanguagePicker = true" />

      <van-popup v-model:show="showLanguagePicker" position="bottom">
        <van-picker v-model="languageValues" :columns="languageColumns" @confirm="onLanguageConfirm"                    @cancel="showLanguagePicker = false" />
      </van-popup>

      <template v-for="item in menuItems" :key="item.route">
        <VanCell :title="item.title" :to="item.route" is-link />
      </template>
    </VanCellGroup>

    <!-- unocss -->
     <div class="unocss">
       unocss
     </div>
  </Container>
</template>
<style lang="less">
.unocss{
  // width: 200px;
  // height: 50px;
  // line-height: 50px;
  // text-align: center;
  // font-size: 20px;
  // background-color: rgb(251 191 36);

  --uno: w-200 h-50 lh-50 bg-amber text-center text-20;
}
</style>
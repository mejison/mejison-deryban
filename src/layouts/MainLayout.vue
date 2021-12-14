<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          {{ mainInfo.nameApp }}
        </q-toolbar-title>

      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          {{ mainInfo.nameApp }}
        </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink.vue'

const linksList = [
  {
    title: 'Instagram',
    caption: '@mejison.dev',
    icon: 'instagram',
    link: 'https://instagram.com/mejison.dev'
  },
  {
    title: 'Facebook',
    caption: '@Kovalchuk Olexandr',
    icon: 'facebook',
    link: 'https://www.facebook.com/profile.php?id=100003850392866'
  },
  {
    title: 'Twitter',
    caption: '@mejison.dev',
    icon: 'rss_feed',
    link: 'https://twitter.com/mejison_dev'
  },
];

const mainInfo = {
  nameApp: 'Mejison Deryban'
}

import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  setup () {
    const leftDrawerOpen = ref(false)

    return {
      essentialLinks: linksList,
      mainInfo,
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
})
</script>

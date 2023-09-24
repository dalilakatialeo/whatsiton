<template>
  <q-layout view="lHh Lpr lFf" style="background-color:#0d0b24;">
    <q-header elevated>
      <q-toolbar class="bg-amber">

        <q-toolbar-title style="font-family: 'Dela Gothic One', cursive; color: #0d0b24; text-transform: none">
          Whatsiton?
        </q-toolbar-title>

      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout',

}
</script>

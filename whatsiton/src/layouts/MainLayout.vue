<template>
  <q-layout view="lHh Lpr lFf" style="background-color: #0d0b24">
    <q-header elevated>
      <q-toolbar class="bg-amber">
        <q-toolbar-title
          style="
            font-family: 'Dela Gothic One', cursive;
            color: #0d0b24;
            text-transform: none;
          "
        >
          Whatsiton?
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
    <q-footer elevated style="background-color: #0d0b24">
      <q-toolbar>
        <q-toolbar-title
          class="footer"
          style="color: white; text-transform: none"
        >
          ©
          <a href="https://github.com/dalilakatialeo" target="_blank"
            ><span class="link-text">DKL</span></a
          ></q-toolbar-title
        >
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",
};
</script>
<style scoped>
.footer {
  font-size: 0.85rem;
}
a .link-text a:visited .link-text {
  color: white;
}
a:active .link-text,
a:hover .link-text {
  color: #ffc107;
}
</style>

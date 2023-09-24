<template>
  <q-page class="flex flex-center">
    <q-col>
      <Search />
    </q-col>

  </q-page>
</template>

<script>

import Search from "components/Search.vue"

export default {
  name: 'PageIndex',
  components: { Search }
}
</script>

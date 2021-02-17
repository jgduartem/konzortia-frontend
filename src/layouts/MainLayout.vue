<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-dark">
      <q-toolbar>
        <q-toolbar-title @click="toHome()">
          Konzortia Capital Test
        </q-toolbar-title>
        <div>
          <q-btn flat text-color="grey-3" label="Movies" to="/Movies"/>
          <q-btn flat text-tcolor="grey-3" label="Clasifications" to="/Clasifications"/>
        </div>
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
  data () {
    return {
    }
  },
  methods: {
    toHome(){
      this.$router.push('/')
    }
  }
}
</script>

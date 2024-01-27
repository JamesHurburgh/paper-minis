<template>
  <PaperMiniMaker />
  <v-footer app>
      <v-spacer></v-spacer>
      <small>
        <a href="https://github.com/JamesHurburgh/paper-minis">GitHub</a>
        | <a href="https://github.com/JamesHurburgh/paper-minis/issues/new">Report issue</a>
        | © {{ new Date().getFullYear() }} —
        <strong>
          Made by <a href="https://x.com/JamesHurburgh">James Hurburgh</a>
        </strong>
      </small>
      <v-spacer></v-spacer>
    </v-footer>
</template>

<script lang="ts" setup>
  import PaperMiniMaker from '@/components/PaperMiniMaker.vue'
</script>

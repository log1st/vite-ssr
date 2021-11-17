<template>
  <input v-model="state.user">
  <Page/>
</template>

<script lang="ts">
import {defineComponent, inject, provide, ref, watch} from 'vue'
import {
  NButton,
  NSpace,
  NInputNumber,
  NCascader,
  NSelect
} from 'naive-ui'
import Page from "./Page.vue";


export default defineComponent({
  components: {
    Page,
    NButton,
    NSpace,
    NInputNumber,
    NCascader,
    NSelect
  },
  created() {
    this.state.user = 1
  },
  setup() {
    const ctx = inject('context');

    const state = ref({
      user: '',
      ...(ctx?.newStateWrapper ? {} : (window._STATE || {}))
    });

    if(ctx?.newStateWrapper) {
      ctx.newStateWrapper.state = state.value;

      watch(state, (newState) => {
        ctx.newStateWrapper.state = newState;
      }, {
        deep: true,
      })
    }

    provide('state', state);

    return {
      state,
    }
  },
})
</script>

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
  setup() {
    const ctx = inject('context');

    const state = ref<{
      user: string
    }>({
      ...{
        user: '',
      },
      ...ctx ? (() => {
        try {
          return JSON.parse(ctx.request.cookies.state)
        } catch (e) {
          return {}
        }
      })() : (window._STATE || {})
    });

    if(ctx) {
      ctx.newStateWrapper.state = state.value;
    }

    watch(state, (newState) => {
      if(ctx) {
        ctx.response.cookie('state', newState, {
          path: '/',
        });
        ctx.newStateWrapper.state = newState;
      } else {
        document.cookie = `state=${JSON.stringify(newState)}`
      }
    }, {
      deep: true,
    })

    provide('state', state);

    return {
      state,
    }
  },
})
</script>

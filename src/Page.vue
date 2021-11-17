<template>
  <div>User: {{user}}</div>
</template>

<script lang="ts">
import {computed, defineComponent, inject, Ref} from "vue";

export default defineComponent({
  setup() {
    const state = inject<Ref<{
      user: string
    }>>('state');

    const user = computed(() => state!.value.user);

    return {
      user,
    }
  }
})
</script>

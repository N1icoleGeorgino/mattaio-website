<template>
<no-ssr>

  <portal to="modals">
    <div v-if="showModal" class="absolute inset-0 justify-center block sm:flex sm:items-center">
      <transition
        @before-leave="backdropLeaving = true"
        @after-leave="backdropLeaving = false"
        enter-active-class="transition-all transition-fast ease-out-quad"
        leave-active-class="transition-all transition-medium ease-in-quad"
        enter-class="opacity-0"
        enter-to-class="opacity-100"
        leave-class="opacity-100"
        leave-to-class="opacity-0"
        appear
      >
        <div v-if="showBackdrop">
          <div class="fixed inset-0 bg-black opacity-25" @click="close"></div>
        </div>
      </transition>

      <transition
        @before-leave="cardLeaving = true"
        @after-leave="cardLeaving = false"
        enter-active-class="transition-all transition-fast ease-out-quad"
        leave-active-class="transition-all transition-medium ease-in-quad"
        enter-class="opacity-0 scale-70"
        enter-to-class="scale-100 opacity-100"
        leave-class="scale-100 opacity-100"
        leave-to-class="opacity-0 scale-70"
        appear
      >
        <div v-if="showContent" class="relative m-4 sm:m-0">
          <slot></slot>
        </div>
      </transition>
    </div>
  </portal>
  </no-ssr>
</template>

<script>
export default {
  props: ['open'],
  name: 'Modal',
  data() {
    return {
      showModal: false,
      showBackdrop: false,
      showContent: false,
      backdropLeaving: false,
      cardLeaving: false,
    }
  },
  watch: {
    open: {
      handler: function (newValue) {
        if (newValue) {
          this.show()
        } else {
          this.close()
        }
      },
      immediate: true
    },
    leaving(newValue) {
      if (newValue === false) {
        this.showModal = false
        this.$emit('close')
      }
    }
  },
  computed: {
    leaving() {
      return this.backdropLeaving || this.cardLeaving
    }
  },
  methods: {
    show() {
      this.showModal = true
      this.showBackdrop = true
      this.showContent = true
    },
    triggerClose(event) {
      // console.log(event)
      if (this.open) {
        this.close()
      }
    },
    close() {
      this.showBackdrop = false
      this.showContent = false
    }
  }
}
</script>

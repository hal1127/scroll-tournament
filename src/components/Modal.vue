<template>
  <transition name="fade"><!-- transitionは不要なら外してOK -->
    <div v-if="sample_modal">
      <div class="modal">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h4 class="modal-title">始めますか？</h4>
              <button type="button" class="close" @click="close_modal">×</button>
            </div>
            <div class="modal-body">
              <p>ボタンを押すとスタート</p>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-primary" @click="close_modal">スタート</button>
            </div>
          </div>
        </div>
      </div>
      <div class="modal-backdrop show"></div>
    </div>
  </transition>
</template>

<script>
  function scroll_control(event) {
    event.preventDefault();
  }

export default {
  mounted() {
    this.no_scroll();
  },
  name: 'Modal',
  data(){
    return {
      sample_modal: true,
    }
  },
  props: {

  },
  methods: {
    close_modal() {
      this.sample_modal = false;
      this.return_scroll();
      window.scrollTo(0, 0);
      this.$emit('scroll_start');
    },
    no_scroll() {
      // PCでのスクロール禁止
      document.addEventListener("mousewheel", scroll_control, { passive: false });
      // スマホでのタッチ操作でのスクロール禁止
      document.addEventListener("touchmove", scroll_control, { passive: false });
    },
    return_scroll() {
      // PCでのスクロール禁止解除
      document.removeEventListener("mousewheel", scroll_control, { passive: false });
      // スマホでのタッチ操作でのスクロール禁止解除
      document.removeEventListener('touchmove', scroll_control, { passive: false });
    }
  }
}
</script>

<style>
/* 表示/非表示はvueで制御するので最初から表示状態にする */
.modal {
  display: block;
}

/* vueのtransitionを使わないなら不要 */
.fade-enter-active, .fade-leave-active {
  transition: opacity .15s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
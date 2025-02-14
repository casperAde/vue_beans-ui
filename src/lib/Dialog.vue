<template>
  <template v-if="visible">
    <Teleport to="body">
      <div class="beans-dialog-overlay" @click="onClickOverlay"></div>
      <div class="beans-dialog-wrapper">
        <div class="beans-dialog">
          <header>
            <slot name="title"/>
            <span @click="close" class="beans-dialog-close"></span></header>
          <main>
            <slot name="content"/>
          </main>
          <footer>
            <Button level="main" @click="onClickOk">OK</Button>
            <Button @click="onClickCancel">Cancel</Button>
          </footer>
        </div>
      </div>
    </Teleport>
  </template>
</template>

<script lang="ts">
import Button from './Button.vue';

type Prop = {
  ok: () => boolean; cancel: () => boolean; closeOnclickOverlay: boolean;
}
type Context = {
  emit: (arg0: string, arg1: boolean) => void;
}
export default {
  props: {
    visible: {
      type: Boolean,
      default: false
    },
    closeOnclickOverlay: {
      type: Boolean,
      default: false
    },
    ok: Function,
    cancel: Function
  },
  components: {
    Button
  },

  setup(props: Prop, context: Context) {
    const close = () => {
      context.emit('update:visible', false);
    };
    const onClickOk = () => {
      // 简写 props.ok?.() !== false
      if (props.ok && props.ok() !== false) {
        close();
      }
    };
    const onClickCancel = () => {
      if (props.cancel && props.cancel() !== false) {
        close();
      }
    };
    const onClickOverlay = () => {
      if (props.closeOnclickOverlay) {
        close();
      }
    };
    return {close, onClickOverlay, onClickOk, onClickCancel};
  }

};
</script>

<style lang="scss">
@import "src/styles/var";

$radius: 4px;
$border-color: #d9d9d9;
.beans-dialog {
  background: white;
  border-radius: $radius;
  box-shadow: 0 0 3px fade_out(black, 0.5);
  min-width: 15em;
  max-width: 90%;

  &-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: fade_out(black, 0.5);
    z-index: 10;
  }

  &-wrapper {
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    z-index: 11;
  }

  > header {
    padding: 12px 16px;
    border-bottom: 1px solid $border-color;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 20px;
  }

  > main {
    padding: 12px 16px;
  }

  > footer {
    border-top: 1px solid $border-color;
    padding: 12px 16px;
    text-align: right;
  }

  &-close {
    position: relative;
    display: inline-block;
    width: 16px;
    height: 16px;
    cursor: pointer;

    &::before,
    &::after {
      content: '';
      position: absolute;
      height: 1px;
      background: black;
      width: 100%;
      top: 50%;
      left: 50%;
    }

    &::before {
      transform: translate(-50%, -50%) rotate(-45deg);
    }

    &::after {
      transform: translate(-50%, -50%) rotate(45deg);
    }
  }
}
</style>
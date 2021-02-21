<template>
  <div class="input__wrapper" :class="{'shadow': shadow}">
    <div class="input__editor"
         id="input" tabindex="1"
         ref="input" contenteditable="true"
         @focusin="toggleShadow(true)"
         @focusout="toggleShadow(false)"
         @input="changeValue"
         :data-placeholder="placeholder"
         v-html="renderedValue"/>
    <div class="done__check">
      <input id="done" type="checkbox">
      <label for="done"/>
    </div>
    <EditorInputActionsComponent class="input__actions"/>
    <div class="background__helper"/>
  </div>
</template>

<script>
import EditorInputActionsComponent from '@/components/@ui/EditorInputActionsComponent.vue';

export default {
  name: 'EditorInput',
  components: {
    EditorInputActionsComponent,
  },
  data() {
    return {
      renderedValue: '',
      shadow: false,
    };
  },
  props: {
    value: {
      type: String,
      required: true,
    },
    placeholder: {
      type: String,
      default: 'Write a new task',
    },
  },
  methods: {
    toggleShadow(shadow) {
      this.shadow = shadow;
    },
    getCaretPosition(el) {
      let sel;
      let caretOffset = 0;
      const doc = el.ownerDocument || el.document;
      const win = doc.defaultView || doc.parentWindow;

      if (typeof win.getSelection !== 'undefined') {
        sel = win.getSelection();
        if (sel.rangeCount > 0) {
          const range = win.getSelection().getRangeAt(0);
          const preCaretRange = range.cloneRange();
          preCaretRange.selectNodeContents(el);
          preCaretRange.setEnd(range.endContainer, range.endOffset);
          caretOffset = preCaretRange.toString().length;
        }
      } else if (sel === doc.selection && sel.type !== 'Control') {
        const textRange = sel.createRange();
        const preCaretTextRange = doc.body.createTextRange();
        preCaretTextRange.moveToElementText(el);
        preCaretTextRange.setEndPoint('EndToEnd', textRange);
        caretOffset = preCaretTextRange.text.length;
      }

      return caretOffset;
    },

    setCaretPosition(el, pos) {
      // eslint-disable-next-line
      for (const node of el.childNodes) {
        if (node.nodeType === 3) { // If text node
          if (node.length >= pos) {
            const range = document.createRange();
            const sel = window.getSelection();
            range.setStart(node, pos);
            range.collapse(true);
            sel.removeAllRanges();
            sel.addRange(range);
            return -1; // Done
          }
          // eslint-disable-next-line
          pos -= node.length;
        } else {
          // eslint-disable-next-line
          pos = this.setCaretPosition(node, pos);
          if (pos === -1) {
            return -1;
          }
        }
      }

      return pos;
    },
    changeValue(e) {
      const str = e.target.innerText;
      const pattern = new RegExp(/(\/\/)(.*)/);
      const cursorPosition = this.getCaretPosition(e.target);
      let commentModifier = 'comment__first';
      if (document.getElementsByClassName('comment__start').length) {
        commentModifier = '';
      }
      this.renderedValue = str.replace(pattern, `<span class="comment__item comment__start ${commentModifier}">$1</span><span class="comment__item comment__text" data-placeholder="write note">$2</span>`);

      this.$nextTick(() => {
        this.setCaretPosition(e.target, cursorPosition);
      });

      this.$emit('input', this.renderedValue);
    }
    ,
  },
};
</script>

<style scoped lang="scss">
@import '../../assets/scss/animations';
@import '../../assets/scss/colors';

$gray: #EAE8EA;
$white: #FFFFFF;

$transitionTime: .3s;
$translateX: 35px;

$backgroundBorderHelper: 15px;

.background__helper {
  position: absolute;
  top: 0;
  left: 0;
  background-color: $white;
  width: 100%;
  height: 100%;
  z-index: 1;
  opacity: 0;
  transition: $transitionTime;
  border-radius: $backgroundBorderHelper;
}

.input__wrapper {
  background-color: #CFD1DB;
  transition: $transitionTime;
  border-radius: $backgroundBorderHelper;
  padding: 15px;
  width: 500px;
  box-sizing: border-box;
  position: relative;
  display: flex;
  justify-content: space-between;
  overflow: hidden;

  &.shadow {
    box-shadow: 0 5px 10px 0 #0000001f;
  }

  &:hover {
    background-color: #C7C9D3;
  }

  .done__check {
    position: absolute;
    top: 15px;
    z-index: 2;
    display: none;
    transform: translateX(-40px);

    &:before {
      content: "";
      background-color: #DFDFE5;
      height: 20px;
      display: block;
      position: absolute;
      left: -32px;
      transform: rotate(
          45deg
      );
      border-radius: 10% 100%;
      width: 20px;
    }

    > input {
      display: none;

      + label {
        background: $gray;
        width: 20px;
        height: 20px;
        display: block;
        border-radius: 5px;
        cursor: pointer;
      }

      &:checked {

        + label {
          background: #aacc8570;
        }
      }
    }
  }

  &:focus {
    background-color: $white;
    box-shadow: 0px 6px 10px 0px #00000010;
    padding-left: 20px;
  }

  .input__editor::v-deep {
    outline: none;
    color: #111011;
    cursor: text;
    transform: translateX(0);
    transition: $transitionTime;
    z-index: 2;
    position: relative;
    flex-grow: 1;
    word-break: break-word;

    .comment__item {
      color: #734F14;

      &.comment__start {
        position: relative;
        display: inline-block;

        &.comment__first {
          animation: commentScale .4s ease-in-out;

          &:before {
            content: "";
            width: 0;
            height: 0;
            top: 12px;
            left: 6px;
            position: absolute;
            z-index: -1;
            animation: shadowBlur 1s ease-in-out;
          }
        }
      }

      &.comment__text {

        &:empty {

          &:after {
            content: attr(data-placeholder);
            color: #C5BBA1;
          }
        }

        &:before {
          content: "";
          width: 435px;
          height: 0px;
          top: 60px;
          display: block;
          position: absolute;
          box-shadow: 0 0 50px 30px #f4eddc;
          z-index: -1;
        }
      }
    }

    &:empty {

      &:after {
        content: attr(data-placeholder);
        color: #5A5D64;
      }
    }

    &:focus, &:not(:empty) {
      padding-left: $translateX;

      + .done__check {
        display: block;
        animation: dripIn .3s ease-in-out;
        transform: translateX(0px);

        &:before {
          animation: dripOut .5s ease-in-out;
        }
      }
    }

    &:focus {
      ~ .background__helper {
        opacity: 1;
        box-shadow: 0 5px 10px 0 #0000001f;
      }

      ~ .input__actions {
        display: flex;
        animation: inputActions .5s ease-in-out;
      }
    }
  }

  .input__actions {
    z-index: 2;
    margin-left: 10px;
    flex-shrink: 0;
    display: none;
  }
}
</style>

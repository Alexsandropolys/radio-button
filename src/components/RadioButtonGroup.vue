<template>
  <div class="radio-button-group">
    <label
      v-for="(item, index) in items"
      :key="`radio-button-group-item-key-${item.value}`"
      class="radio-button-group__item radio-button"
      :class="buildLabelClass(index)"
    >
      <input
        class="radio-button__input"
        :value="item.value"
        ref="radioButtonInputs"
        v-model="safeValue"
        :name="name"
        type="radio"
        :disabled="item.disabled || disabled"
      />
      <span class="radio-button__text">
        {{ item.title }}
      </span>
    </label>
  </div>
</template>

<script>
import { nanoid } from "nanoid";

export default {
  name: "RadioButtonGroup",
  props: {
    value: { type: String, default: "" },
    items: { type: Array, default: () => [] },
    disabled: { type: Boolean, default: false }
  },
  model: {
    event: "change"
  },
  data: () => {
    return {
      name: ""
    };
  },
  computed: {
    safeValue: {
      get() {
        return this.value;
      },
      set(newValue) {
        if (!this.disabled) {
          this.emitChange(newValue);
        }
        this.$nextTick(this.synchronizeValue);
      }
    }
  },
  methods: {
    emitChange(value) {
      this.$emit("change", value);
    },
    synchronizeValue() {
      this.$refs.radioButtonInputs.forEach(radioButtonInput => {
        radioButtonInput.checked = radioButtonInput.value === this.safeValue;
      });
    },
    createNameForRadioButton() {
      this.name = `radio-button-input-name-${nanoid()}`;
    },

    buildLabelClass(itemIdx) {
      return {
        "radio-button-group__item_disabled":
          this.disabled || this.items[itemIdx].disabled
      };
    }
  },
  mounted() {
    this.createNameForRadioButton();
  }
};
</script>
<style lang="less">
@default-background-color: #fff;
@primary-color: #6264a7;
@border-color: #484644;
@outline-color: #bdbde6;
@disable-color: #c8c6c4;

.visually-hidden() {
  border-width: 0;
  clip: rect(1px, 1px, 1px, 1px);
  height: 1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}

.radio-button-group {
  color: @border-color;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;

  .radio-button {
    display: flex;
    cursor: pointer;
    position: relative;
    overflow: hidden;

    &__input {
      .visually-hidden();

      &:focus + .radio-button__text {
        &:before {
          box-shadow: 0 0 0 2px @outline-color;
        }
      }

      &:checked + .radio-button__text {
        &:before {
          background-color: #6264a7;
          border: 1px solid #6264a7;
        }
      }

      &:disabled + .radio-button__text {
        color: @disable-color;

        &:before {
          border-color: @disable-color;
        }
      }

      &:checked:disabled + .radio-button__text {
        &:before {
          background-color: @disable-color;
        }
      }
    }

    &__text {
      display: flex;
      align-items: center;
      padding: 2px;
      transition: 300ms ease;
      font-size: 12px;
      line-height: 16px;

      &:before {
        display: flex;
        flex-shrink: 0;
        content: "";
        background-color: @default-background-color;
        width: 12px;
        height: 12px;
        border-radius: 50%;
        margin-right: 12px;
        transition: 300ms ease;
        border: 1px solid @border-color;
      }
    }

    &:hover {
      .radio-button__text {
        &:before {
          box-shadow: 0 0 0 2px @outline-color;
        }
      }
    }
  }

  &__item {
    &_disabled {
      cursor: default;
      pointer-events: none;
    }
  }
}
</style>

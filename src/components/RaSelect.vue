<template>
  <div class="dropdown-container" :style="{ width: $props.width || '' }" v-click-outside="handleClickOutside">
    <div class="dropdown-select" @click="toggleDropdown">
      {{ selectedOption || $props.placeholder }}
      <span class="dropdown-caret">
        <svg width="10px" height="10px" viewBox="0 0 1024 1024" class="icon"  version="1.1" xmlns="http://www.w3.org/2000/svg"><path d="M903.232 256l56.768 50.432L512 768 64 306.432 120.768 256 512 659.072z" fill="#6c757d" /></svg>
      </span>
    </div>
    <transition name="fade">
      <ul v-if="isOpen" class="dropdown-list">
        <li
          v-for="(option, index) in options"
          :key="index"
          @click.stop="selectOption(option)"
          class="dropdown-item"
        >
          {{ option }}
        </li>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isOpen: false,
      selectedOption: this.$props.value || '',
      options: this.$props.items || ['Option 1', 'Option 2', 'Option 3', 'Option 4'],
    };
  },
  props: {
    value: {
      type: String,
      default: ""
    },
    placeholder: {
      type: String,
    },
    items: {
      type: Array,
    },
    width: {
      type: String
    }
  },
  directives: {
    clickOutside: {
      mounted(el, binding, vnode) {
        // The event listener function
        el.clickOutsideEvent = function(event) {
          // Check that click was outside the el and his children
          if (!(el === event.target || el.contains(event.target))) {
            // Call the provided method
            vnode.context[binding.expression](event);
            console.log("outside")
          }
        };
        // Attach the listener function to the global click event
        document.addEventListener('click', el.clickOutsideEvent);
      },
      unmounted(el) {
        // Detach the event listener from the global click event
        document.removeEventListener('click', el.clickOutsideEvent);
      },
    }
  },
  methods: {
    toggleDropdown() {
      this.isOpen = !this.isOpen;
    },
    selectOption(option) {
      this.selectedOption = option;
      this.isOpen = false;
    },
    handleClickOutside() {
      this.isOpen = false;
    }
  }
};
</script>

<style scoped lang="scss">
.dropdown-container {
  position: relative;
  width: 100%;
}

.dropdown-select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #fff;
  color: #6c757d;
}

.dropdown-caret {
  transition: transform 0.3s ease;
}

.dropdown-list {
  position: absolute;
  width: 100%;
  min-width: 228px;
  box-shadow: 0 8px 16px rgba(0,0,0,0.1);
  list-style: none;
  padding: 0;
  margin-top: 10px;
  border: 1px solid #e9ecef;
  border-radius: 5px;
  background-color: #fff;
  max-height: 200px;
  overflow-y: auto;
  z-index: 1000;
}

.dropdown-item {
  padding: 15px 25px;
  cursor: pointer;
}

.dropdown-item:last-child {
  border-bottom: none;
}

.dropdown-item:hover {
  background-color: #f6f6f6;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}

/* Rotate the caret when the dropdown is open */
.dropdown-container.is-open .dropdown-caret {
  transform: rotate(180deg);
}
</style>
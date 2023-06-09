<script setup lang="ts">
import { computed, ref, type Ref } from "vue";
import { sign, join } from "../http/userHttp.js";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();

const props = defineProps({
  create: String,
});

const username = ref("");
const name = ref("");
const password = ref("");
const error = ref("");

const showError = (el: Ref, text: string) => {
  el.value = text;
  setTimeout(function () {
    el.value = "";
  }, 5000);
};

const click = async () => {
  try {
    if (props.create === "create") {
      await sign(username.value, name.value, password.value);
    } else {
      await join(username.value, name.value, password.value);
    }
    router.push(`/room/${name.value}`);
  } catch (e: any) {
    showError(error, e);
  }
};
</script>

<template>
  <div class="createRoom">
    <form
      class="form"
      autocomplete="off"
      @submit.prevent="click"
    >
      <div
        class="error-message"
        v-if="error"
      >
        <span class="error-text">{{ error }}</span>
      </div>
      <div class="control">
        <h1 v-if="create === 'create'">Create your own room</h1>
        <h1 v-else>Join somebody's room</h1>
      </div>
      <div class="control block-cube block-input">
        <input
          name="username"
          v-model="username"
          type="text"
          placeholder="Username"
          autocomplete="off"
          readonly
          onfocus="this.removeAttribute('readonly')"
          minlength="3"
          maxlength="20"
        />
        <div class="bg-top">
          <div class="bg-inner"></div>
        </div>
        <div class="bg-right">
          <div class="bg-inner"></div>
        </div>
        <div class="bg">
          <div class="bg-inner"></div>
        </div>
      </div>
      <div class="control block-cube block-input">
        <input
          name="name"
          v-model="name"
          type="text"
          id="name"
          placeholder="Room name"
          autocomplete="off"
          readonly
          onfocus="this.removeAttribute('readonly')"
          minlength="3"
          maxlength="20"
        />
        <div class="bg-top">
          <div class="bg-inner"></div>
        </div>
        <div class="bg-right">
          <div class="bg-inner"></div>
        </div>
        <div class="bg">
          <div class="bg-inner"></div>
        </div>
      </div>
      <div class="control block-cube block-input">
        <input
          v-if="create === 'create'"
          name="password"
          v-model="password"
          type="password"
          placeholder="Room password (optional)"
          autocomplete="off"
          readonly
          onfocus="this.removeAttribute('readonly')"
          minlength="3"
          maxlength="20"
        />
        <input
          v-else
          name="password"
          v-model="password"
          type="password"
          placeholder="Room password (if needed)"
          autocomplete="off"
          readonly
          onfocus="this.removeAttribute('readonly')"
        />
        <div class="bg-top">
          <div class="bg-inner"></div>
        </div>
        <div class="bg-right">
          <div class="bg-inner"></div>
        </div>
        <div class="bg">
          <div class="bg-inner"></div>
        </div>
      </div>
      <button
        class="btn block-cube block-cube-hover"
        type="submit"
      >
        <div class="bg-top">
          <div class="bg-inner"></div>
        </div>
        <div class="bg-right">
          <div class="bg-inner"></div>
        </div>
        <div class="bg">
          <div class="bg-inner"></div>
        </div>
        <div
          v-if="create === 'create'"
          class="text"
        >
          Create room
        </div>
        <div
          v-else
          class="text"
        >
          Join room
        </div>
      </button>
    </form>
  </div>
</template>

<style scoped lang="scss">
// room creation styles
*,
::after,
::before {
  box-sizing: border-box;
}

$bg_body: #212121;
$bg_gradient_last_color: rgba(0, 212, 255, 1);
$bg_gradient: linear-gradient(
  90deg,
  rgba(2, 0, 36, 1) 0%,
  rgba(52, 9, 121, 1) 37%,
  $bg_gradient_last_color 94%
);

body {
  background-color: $bg_body;
  color: #fff;
  font-family: monospace, serif;
  letter-spacing: 0.05em;
}

h1 {
  font-size: 23px;
}

.error-message {
  background-color: transparent;
  border: 1px solid #730707;
  padding: 20px 30px;
  margin: 0px 0 30px 0;
  opacity: 0;
  animation-name: error-animation;
  animation-duration: 1s;
  animation-timing-function: ease-in-out;
  animation-fill-mode: forwards;
}
.error-text {
  color: #b90d0d;
  font-family: Helvetica, Arial, sans-serif;
  font-size: 13px;
  font-weight: bold;
  line-height: 20px;
  // text-shadow: 1px 1px rgba(250, 250, 250, 0.3);
}

@keyframes error-animation {
  0% {
    transform: translateY(-170%);
    opacity: 0;
  }
  50% {
    transform: translateY(20%);
    opacity: 1;
  }
  100% {
    transform: translateY(0%);
    opacity: 1;
  }
}

.form {
  width: 480px;
  padding: 64px 15px 24px 0;
  // margin: 0 auto;
  .control {
    margin: 0 0 24px;
    input {
      width: 100%;
      padding: 14px 16px;
      border: 0;
      background: transparent;
      color: #fff;
      font-family: monospace, serif;
      letter-spacing: 0.05em;
      font-size: 16px;
      &:hover,
      &:focus {
        outline: none;
        border: 0;
      }
    }
  }
  .btn {
    width: 100%;
    display: block;
    padding: 14px 16px;
    background: transparent;
    outline: none;
    border: 0;
    color: #fff;
    letter-spacing: 0.1em;
    font-weight: bold;
    font-family: monospace;
    font-size: 16px;
    cursor: pointer;
  }
}

.block-cube {
  position: relative;
  .bg-top {
    position: absolute;
    height: 10px;
    background: rgb(2, 0, 36);
    background: $bg_gradient;
    bottom: 100%;
    left: 5px;
    right: -5px;
    transform: skew(-45deg, 0);
    margin: 0;
    .bg-inner {
      bottom: 0;
    }
  }
  .bg {
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    background: rgb(2, 0, 36);
    background: $bg_gradient;
  }
  .bg-right {
    position: absolute;
    background: rgb(2, 0, 36);
    background: rgba(0, 212, 255, 1);
    top: -5px;
    z-index: 0;
    bottom: 5px;
    width: 10px;
    left: 100%;
    transform: skew(0, -45deg);
    .bg-inner {
      left: 0;
    }
  }
  .bg {
    .bg-inner {
      transition: all 0.2s ease-in-out;
    }
  }
  .bg-inner {
    background: $bg_body;
    position: absolute;
    left: 2px;
    top: 2px;
    right: 2px;
    bottom: 2px;
  }
  .text {
    position: relative;
    z-index: 2;
  }
  &.block-input {
    input {
      position: relative;
      z-index: 2;
      &:focus ~ .bg-right .bg-inner,
      &:focus ~ .bg-top .bg-inner,
      &:focus ~ .bg-inner .bg-inner {
        top: 100%;
        background: rgba(255, 255, 255, 0.5);
      }
    }
    .bg-top,
    .bg-right,
    .bg {
      background: rgba(255, 255, 255, 0.5);
      transition: background 0.2s ease-in-out;
    }
    .bg-right,
    .bg-top {
      .bg-inner {
        transition: all 0.2s ease-in-out;
      }
    }
    &:focus,
    &:hover {
      .bg-top,
      .bg-right,
      .bg {
        background: rgba(255, 255, 255, 0.8);
      }
    }
  }
  // State hover, focus
  &.block-cube-hover:focus,
  &.block-cube-hover:hover {
    .bg {
      .bg-inner {
        top: 100%;
      }
    }
  }
}

.credits {
  position: fixed;
  left: 0;
  bottom: 0;
  padding: 15px 15px;
  width: 100%;
  z-index: 111;

  a {
    opacity: 0.6;
    color: #fff;
    font-size: 11px;
    text-decoration: none;
    &:hover {
      opacity: 1;
    }
  }
}
@media (max-width: 567px) {
  .form {
    width: 100%;
  }
}
</style>

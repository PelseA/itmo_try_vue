<script setup>
import { RouterLink, RouterView, useRouter, useRoute } from "vue-router";
import { useTodosStore } from "@/stores/todos";
import { storeToRefs } from "pinia";
import { ref, Transition, KeepAlive } from "vue";
import Routes from "@/consts/Routes";

const todoStore = useTodosStore();
const { numberOfTodos } = storeToRefs(todoStore);

const router = useRouter();

const isCurrentRouteIndex = () => {
  const route = useRoute();
  const result = route.path === "/";
  console.log("isCurrentRouteIndex:", result);
  return result;
};

const canRenderNumberOfTodos = ref(isCurrentRouteIndex());
</script>

<template>
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="@/assets/logo.svg"
      width="25"
      height="25"
    />
    <div v-if="canRenderNumberOfTodos">Todos created: {{ numberOfTodos }}</div>
    <div>Current route: {{ $route.path }}</div>
    <div class="wrapper">
      <nav>
        <RouterLink :to="Routes.HOME">Todos</RouterLink>
        <RouterLink :to="Routes.COUNTER">Counter</RouterLink>
        <!--     можно так   <RouterLink to="/counter">Counter</RouterLink>-->
        <RouterLink :to="Routes.ABOUT">About</RouterLink>
      </nav>
    </div>
  </header>
  <main>
    <!-- <KeepAlive></KeepAlive> кэширует и не вызывает setup, onMounted
      то, что не нужно кэшировать, помечается exclude -->
    <RouterView v-slot="{ Component }">
      <KeepAlive exclude="TodoItemView">
        <component :is="Component" />
      </KeepAlive>
    </RouterView>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    margin: 2rem 0;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>

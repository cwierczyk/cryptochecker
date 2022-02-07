<template>
  <router-view/>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Roboto', sans-serif;
}

body {
  background: #E5E5E5;
}

.container {
  display: flex;
  justify-content: center;
  padding: 2rem 0.5rem;

  @media (min-width: 1280px) {
    padding: 2rem 8rem;
  }
}

.flex {
  display: flex;
}

.flex-col {
  flex-direction: column;
}

.mobile-hidden {
  display: none;

  @media (min-width: 768px) {
    display: block;
  }
}
</style>

<template>
  <div class="Module">
    <div class="icon">
      <img :src="module.icon" :alt="module.title">
    </div>
    <div class="content">
      <h3>{{module.title}}</h3>
      <p class="desc">{{module.description}}</p>
      <ul>
          <li v-for="(lo, i) in module.key_learnings" :key="i" class="Vlt-list-item">
            <span class="Vlt-list-item__icon">
              <svg class="Vlt-icon Vlt-icon--smaller Vlt-blue-light">
                <use
                  xlink:href="../../node_modules/@vonagevolta/volta2/dist/symbol/volta-icons.svg#Vlt-icon-check-circle-negative"
                />
              </svg>
            </span>
            <span class="Vlt-list-item__text">{{lo}}</span>
          </li>
      </ul>
      <a class="Vlt-btn Vlt-btn--primary" :href="module.path">Go to class</a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    module: {
      type: Object,
      required: true,
    },
  },
}
</script>

<style scoped>
.Module {
  display: grid;
  grid-template-columns: 100px auto;
  gap: 3em;
  margin-top: 2em;
}
img {
  margin-top: 0.25em;
}
.desc {
  font-size: 1.25em;
  margin-bottom: 1em;
}
ul {
  margin-bottom: 1em;
}
@media only screen and (max-width: 515px) {
  .Module {
    grid-template-columns: 1fr;
    gap: 1em;
  }
  img {
    width: 100px;
  }
}
</style>

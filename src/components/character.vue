<template>
  <div class="column is-12-mobile is-4-tablet is-3-desktop">
    <div class="card" style="margin-top:3rem">
      <div class="card-header">
       <figure class="image is-5by4" style="margin: 0 auto; width:28rem; margin:1rem" >
        <img  :src="character.image" :alt="character.name">
        </figure>
      </div>
      <div class="card-content">
        <h3 class="title is-size-5">{{character.name}}</h3>
        <button class="button is-dark is-rounded is-medium is-size-6" @click="showMore(character.id)"> VER MAS</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["character"],
  methods: {
    showMore(id) {
      this.$emit("showModal", id);
    }
  }
};
</script>
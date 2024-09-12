<template>
  <div class="Rectangle-3">
    <span class="Snap-photos-and-share-like-never-before">
      Snap photos and share like never before
    </span>

    <div class="Group">
      <img
        src="img/ellipse.png"
        srcset="img/ellipse@2x.png 2x, img/ellipse@3x.png 3x"
        class="Ellipse"
      />
      <span class="Sed-ut-perspiciatis"> Sed ut perspiciatis </span>
    </div>

    <q-page class="row items-center justify-evenly">
      <card-component lorem=""></card-component>
    </q-page>
  </div>
</template>

<script setup lang="ts">
// import { ref } from 'vue';
// import { Todo, Meta } from 'components/models';
// import ExampleComponent from 'components/ExampleComponent.vue';
import CardComponent from 'components/CardComponent.vue';

defineOptions({
  name: 'IndexPage'
});
</script>

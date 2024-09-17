<script lang="ts">
import { ref } from 'vue';
import { api } from 'boot/axios';
// import { Todo, Meta } from 'components/models';
// import ExampleComponent from 'components/ExampleComponent.vue';
// import CardComponent from 'components/CardComponent.vue';

export default {
  setup() {
    const UserDetail = ref([
      {
        index: 1,
        image: '../img/ellipse.png',
        srcset: 'img/ellipse@2x.png 2x, img/ellipse@3x.png 3x',
        title: 'Sed ut perspiciatis',
        detail:
          'Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem.'
      },
      {
        index: 2,
        image: '../img/ellipse.png',
        srcset: 'img/ellipse@2x.png 2x, img/ellipse@3x.png 3x',
        title: 'Lorem ipsum dolor',
        detail:
          'Amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis.'
      },
      {
        index: 3,
        image: '../img/ellipse.png',
        srcset: 'img/ellipse@2x.png 2x, img/ellipse@3x.png 3x',
        title: 'Nemo enim ipsam',
        detail:
          'Consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor.'
      }
    ]);
    const PhotoData = ref();

    const mixing = () => {
      for (let i = 0; i < UserDetail.value.length; i++) {
        let RandomNumber = Math.floor(Math.random() * UserDetail.value.length);
        let store = UserDetail.value[0];
        UserDetail.value[0] = UserDetail.value[RandomNumber];
        UserDetail.value[RandomNumber] = store;
      }
    };

    const data = async () => {
      const photo = await api({
        method: 'get',
        url: '/photos/random',
        params: {
          client_id: import.meta.env.VITE_API_ACCESS_KEY,
          Secret_key: import.meta.env.VITE_API_SECRET_KEY
        }
      }).catch((e) => {
        console.log(e);
      });

      PhotoData.value = photo?.data.urls.full;

      console.log(PhotoData.value);
    };

    data();
    mixing();
    return {
      UserDetail
    };
  }
};
</script>

<template>
  <div class="Screen">
    <div class="Title">
      <span class="Word-1"> Snap photos and share like never before </span>
    </div>
    <!-- 머리 -->
    <div class="Section-1">
      <div v-for="user in UserDetail" :key="user.index" class="Group">
        <div class="Ellipse">
          <img src="../img/ellipse.png" :srcset="user.srcset" class="Ellipse" />
        </div>
        <div class="User-Title">{{ user.title }}</div>
        <div class="User-Word">
          {{ user.detail }}
        </div>
        <div class="Learn-more">Learn more</div>
      </div>
    </div>
    <!-- 유저 -->
    <div class="Image-Box">
      <div class="Box-Word1">Sed ut perspiciatis unde omnis</div>
      <div class="Box-Word2">
        There are many variations of passages of Lorem Ipsum available, but the
        majority have suffered alteration in some form, by injected humour, or
        randomised words which don't look even slightly believable. If you are
        going to use a passage of Lorem Ipsum, you need to be sure there isn't
        anything embarrassing hidden in the middle of text. All the Lorem Ipsum
        generators on the Internet tend to repeat predefined chunks as
        necessary.
      </div>
      <div class="contour"></div>
      <div class="Box-Word2">
        Sed ut perspiciatis unde omnis iste natus error sit voluptatem
        accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab
        illo inventore.
      </div>
      <div class="Box-Word4">
        Sed ut perspiciatis unde omnis iste natus error sit voluptatem
        accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab
        illo inventore.
      </div>
      <div class="Box-Word5">Subscribe to our newsletter</div>
      <div class="Text-Box"></div>
    </div>
    <!-- 이미지 박스 -->
    <div class="Section-2">
      <div class="Word-2">Duis tincidunt ut ligula vitae mollis.</div>
      <div class="AlignBox">
        <div class="Frame-1">
          <div class="Align-Location"><span class="All"> All </span></div>
          <div class="Align-Location"><span class="All"> All </span></div>
        </div>
        <div class="Frame-2">
          <div class="Number-Align">
            <div class="Number">1000</div>
          </div>
          <div class="Countour"></div>
          <div class="Number-Align">
            <div class="Number">1000</div>
          </div>
        </div>
      </div>
    </div>
    <!-- 정렬 박스 -->
    <div class="Location-Group">
      <div class="Location-Box">
        <div class="Location-Align">
          <span class="Location-Title"> Italy, Pica </span
          ><span class="Location-Value"> 1173 </span>
        </div>
        <img
          src="../img/image.png"
          srcset="img/image@2x.png 2x, img/image@3x.png 3x"
          class="Location-Image"
        />
        <span class="Location-Description">
          The Leaning Tower of Pisa, or simply the Tower of Pisa (torre di
          Pisa), is the campanile, or freestanding bell tower, of Pisa
          Cathedral. It is known for its nearly four-degree lean, the result of
          an unstable foundation. The tower is one of three structures in the
          Pisa's Cathedral Square (Piazza del Duomo), which includes the
          cathedral and Pisa Baptistry.
        </span>
      </div>
      <div class="Location-Box">
        <div class="Location-Align">
          <span class="Location-Title"> Italy, Pica </span
          ><span class="Location-Value"> 1173 </span>
        </div>
        <img
          src="../img/image.png"
          srcset="img/image@2x.png 2x, img/image@3x.png 3x"
          class="Location-Image"
        />
        <span class="Location-Description">
          The Leaning Tower of Pisa, or simply the Tower of Pisa (torre di
          Pisa), is the campanile, or freestanding bell tower, of Pisa
          Cathedral. It is known for its nearly four-degree lean, the result of
          an unstable foundation. The tower is one of three structures in the
          Pisa's Cathedral Square (Piazza del Duomo), which includes the
          cathedral and Pisa Baptistry.
        </span>
      </div>
    </div>

    <!-- <img
      src="../img/rectangle.png"
      srcset="../img/rectangle@2x.png 2x, ../img/rectangle@3x.png 3x"
      class="Rectangle"
    /> -->
  </div>

  <!-- <q-page class="row items-center justify-evenly">
      <card-component lorem=""></card-component>
    </q-page> -->
</template>

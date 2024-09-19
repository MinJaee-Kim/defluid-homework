<script lang="ts">
import { ref } from 'vue';
import { api } from 'boot/axios';

export default {
  setup() {
    const UserDetail = ref([
      {
        index: 1,
        image: 'src/img/ellipse.png',
        srcset: 'src/img/ellipse@2x.png 2x, src/img/ellipse@3x.png 3x',
        title: 'Sed ut perspiciatis',
        detail:
          'Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem.'
      },
      {
        index: 2,
        image: 'src/img/ellipse2.png',
        srcset: 'src/img/ellipse2@2x.png 2x, src/img/ellipse2@3x.png 3x',
        title: 'Lorem ipsum dolor',
        detail:
          'Amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis.'
      },
      {
        index: 3,
        image: 'src/img/ellipse3.png',
        srcset: 'src/img/ellipse3@2x.png 2x, src/img/ellipse3@3x.png 3x',
        title: 'Nemo enim ipsam',
        detail:
          'Consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor.'
      }
    ]);
    const location = ref('All');
    const locationOption = ref([
      { label: 'All', value: 'All' },
      { label: 'Asia', value: 'Asia' },
      { label: 'Europe', value: 'Europe' },
      { label: 'America', value: 'America' },
      { label: 'Oceania', value: 'Oceania' }
    ]);
    const email = ref('');
    const checkEmail = ref('none');
    const year = ref(1000);
    const PlaceDetails = ref([
      {
        index: 1,
        image: 'src/img/image-italy.png',
        srcset: 'src/img/image-italy@2x.png 2x, src/img/image-italy@3x.png 3x',
        title: 'Italy, Pica',
        year: 1173,
        location: 'Europe',
        detail:
          "The Leaning Tower of Pisa, or simply the Tower of Pisa (torre di Pisa), is the campanile, or freestanding bell tower, of Pisa Cathedral. It is known for its nearly four-degree lean, the result of an unstable foundation. The tower is one of three structures in the Pisa's Cathedral Square (Piazza del Duomo), which includes the cathedral and Pisa Baptistry."
      },
      {
        index: 2,
        image: 'src/img/image-spain.png',
        srcset: 'src/img/image-spain@2x.png 2x, src/img/image-spain@3x.png 3x',
        title: 'Spain, Sagrada Família',
        year: 1882,
        location: 'Europe',
        detail:
          'The Basílica i Temple Expiatori de la Sagrada Família, otherwise known as Sagrada Família, is a church under construction in the Eixample district of Barcelona, Catalonia, Spain. It is the largest unfinished Catholic church in the world. Designed by Catalan architect Antoni Gaudí (1852–1926), in 2005 his work on Sagrada Família was added to an existing (1984) UNESCO World Heritage Site, "Works of Antoni Gaudí". On 7 November 2010, Pope Benedict XVI consecrated the church and proclaimed it a minor basilica.'
      },
      {
        index: 3,
        image: 'src/img/image-us.png',
        srcset: 'src/img/image-us@2x.png 2x, src/img/image-us@3x.png 3x',
        title: 'US, Fallingwater',
        year: 1935,
        location: 'America',
        detail:
          "Fallingwater is a house designed by the architect Frank Lloyd Wright in 1935. Situated in the Mill Run section of Stewart township, in the Laurel Highlands of southwest Pennsylvania, about 70 miles (110 km) southeast of Pittsburgh in the United States, it is built partly over a waterfall on the Bear Run river. The house was designed to serve as a weekend retreat for Liliane and Edgar J. Kaufmann, the owner of Pittsburgh's Kaufmann's Department Store."
      },
      {
        index: 4,
        image: 'src/img/image-russia.png',
        srcset:
          'src/img/image-russia@2x.png 2x, src/img/image-russia@3x.png 3x',
        title: "Russia, Saint Basil's Cathedral",
        year: 1561,
        location: 'Europe',
        detail:
          "The Cathedral of Vasily the Blessed (Russian: Собор Василия Блаженного, romanized: Sobor Vasiliya Blazhennogo), known in English as Saint Basil's Cathedral, is an Orthodox church in Red Square of Moscow, and is one of the most popular cultural symbols of Russia."
      }
    ]);
    const FilterPlaceDetails = ref([
      {
        index: 1,
        image: '',
        srcset: '',
        title: '',
        year: 1,
        location: '',
        detail: ''
      }
    ]);
    const PhotoData = ref();
    const saveCardData = ref('');

    FilterPlaceDetails.value = PlaceDetails.value;

    const mixing = () => {
      for (let i = 0; i < UserDetail.value.length; i++) {
        let RandomNumber = Math.floor(Math.random() * UserDetail.value.length);
        let store = UserDetail.value[0];
        UserDetail.value[0] = UserDetail.value[RandomNumber];
        UserDetail.value[RandomNumber] = store;
      }
    };

    const checkEmailRegex = () => {
      let pattern = new RegExp(
        '^[a-zA-Z0-9+-\_.]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$'
      );

      checkEmail.value = pattern.test(email.value) ? 'true' : 'false';
    };

    const alignCard = (card: string) => {
      saveCardData.value = card;
      if (card == 'All') {
        FilterPlaceDetails.value = PlaceDetails.value.filter((value) => {
          return year.value < value.year;
        });
      } else {
        FilterPlaceDetails.value = PlaceDetails.value.filter((value) => {
          return value.location == card && year.value < value.year;
        });
      }
    };

    const setYearAndAlignCard = (num: number) => {
      year.value = num;
      alignCard(saveCardData.value);
    };

    const setPhoto = async () => {
      let date = new Date();
      date.setDate(date.getDate() + 1);
      if (!document.cookie) {
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

        document.cookie =
          'url=' + photo?.data.urls.full + '; expires=' + date.toUTCString();
        PhotoData.value = photo?.data.urls.full;
      } else {
        PhotoData.value = document.cookie.split(';')[0].substring(4);
      }
    };

    setPhoto();
    mixing();
    return {
      UserDetail,
      PhotoData,
      PlaceDetails,
      FilterPlaceDetails,
      location,
      locationOption,
      email,
      checkEmail,
      year,
      alignCard,
      setYearAndAlignCard,
      checkEmailRegex
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
          <img :src="user.image" :srcset="user.srcset" class="Ellipse" />
        </div>
        <div class="User-Title">{{ user.title }}</div>
        <div class="User-Word">
          {{ user.detail }}
        </div>
        <div class="Learn-more">Learn more</div>
      </div>
    </div>
    <!-- 유저 -->
    <div
      class="Image-Box"
      :style="{ backgroundImage: 'url(' + PhotoData + ')' }"
    >
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
      <div
        class="Text-Box"
        :class="{
          'Input-green': checkEmail == 'true',
          'Input-red': checkEmail == 'false'
        }"
      >
        <q-input
          v-model="email"
          @keyup.enter="checkEmailRegex"
          placeholder="Enter User Email"
          :dense="true"
        >
          <template v-slot:append>
            <q-icon
              name="send"
              @click="checkEmailRegex"
              :color="
                checkEmail == 'true' || checkEmail == 'none' ? 'white' : 'grey'
              "
              class="cursor-pointer"
            />
          </template>
        </q-input>
        <div
          class="Alert-Message"
          :class="{
            'Display-None': checkEmail == 'none' || checkEmail == 'true'
          }"
        >
          Please enter a valid email
        </div>
      </div>
    </div>
    <!-- 이미지 박스 -->
    <div class="Section-2">
      <div class="Word-2">Duis tincidunt ut ligula vitae mollis.</div>
      <div class="AlignBox">
        <div class="Frame-1">
          <q-btn-toggle
            v-model="location"
            toggle-color="white"
            flat
            rounded
            :options="locationOption"
            @update:model-value="alignCard"
          />
        </div>
        <div class="Frame-2">
          <div class="Number-Align">
            <q-btn
              round
              color="black"
              label="1000"
              @click="setYearAndAlignCard(1000)"
            />
          </div>
          <div
            :class="{ 'Non-Select-Countour': year < 1300 }"
            class="Countour"
          ></div>
          <div class="Number-Align">
            <q-btn
              round
              color="black"
              label="1300"
              @click="setYearAndAlignCard(1300)"
              :class="{ 'Non-Select-Round': year < 1300 }"
            />
          </div>
          <div
            :class="{ 'Non-Select-Countour': year < 1700 }"
            class="Countour"
          ></div>
          <div class="Number-Align">
            <q-btn
              round
              color="black"
              label="1700"
              @click="setYearAndAlignCard(1700)"
              :class="{ 'Non-Select-Round': year < 1700 }"
            />
          </div>
          <div
            :class="{ 'Non-Select-Countour': year < 2000 }"
            class="Countour"
          ></div>
          <div class="Number-Align">
            <q-btn
              round
              color="black"
              label="2000"
              @click="setYearAndAlignCard(2000)"
              :class="{ 'Non-Select-Round': year < 2000 }"
            />
          </div>
        </div>
      </div>
    </div>
    <!-- 정렬 박스 -->
    <q-scroll-area class="Location-Area" id="scroll-area-with-virtual-scroll-1">
      <q-virtual-scroll
        :items="FilterPlaceDetails"
        virtual-scroll-horizontal
        v-slot="{ item }"
      >
        <div class="Location-Group">
          <div class="Location-Box">
            <div class="Location-Align">
              <span class="Location-Title"> {{ item.title }} </span
              ><span class="Location-Value"> {{ item.year }} </span>
            </div>
            <img
              :src="item.image"
              :srcset="item.srcset"
              class="Location-Image"
            />
            <span class="Location-Description">
              {{ item.detail }}
            </span>
          </div>
        </div>
      </q-virtual-scroll>
      <div
        class="Title"
        :style="{ display: FilterPlaceDetails.length ? 'none' : 'block' }"
      >
        <span class="Word-1"> Sorry, No item found. </span>
      </div>
    </q-scroll-area>
  </div>
</template>

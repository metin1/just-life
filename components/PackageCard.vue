<template>
  <ElContainer class="package-container" id="app">
    <h1>JUST LIFE Packages</h1>
    <ElCarousel
      type="card"
      arrow="always"
      :interval="40000"
      :autoplay="false"
      height="380px"
      width="300px"
    >
      <ElCarouselItem
        class="carousel-container"
        v-for="pack in packages"
        v-bind:key="pack"
      >
        <transition-group name="flip" tag="div" class="flipper">
          <div class="front" v-show="!detailCard[pack.id]" key="front">
            <ElCard
              class="front-content w-full"
              :style="
                selectedPackage?.id === pack.id
                  ? 'border: #006658 4px solid;'
                  : 'border: #ebebeb 4px solid;'
              "
            >
              <div class="package-card-header" slot="header">
                <h3>{{ pack.name }}</h3>
                <h3>{{ selectedPackage?.id  }}</h3>
              </div>
              <div>
                <div class="grey-text">FROM</div>
                <div class="price-section">
                  <div class="price">{{ pack.price }} SAR/hr</div>
                  <div v-if="pack.discount">
                    <ElTag class="save-tag" effect="light" round>
                      SAVE {{ pack.discount * 100 }}% OFF
                    </ElTag>
                  </div>
                </div>
              </div>
              <button
                class="detail-button front-detail-button"
                @click="flipCardSwitch(pack.id)"
              >
                Detail
              </button>
              <button
                class="select-button front-select-button"
                @click="handleSelectPackage(pack)"
              >
                Select
              </button>
            </ElCard>
          </div>
          <div class="back" v-show="detailCard[pack.id]" key="back">
            <ElCard class="back-content">
              <div slot="header">
                <h3>{{ pack.name }}</h3>
              </div>
              <ul>
                <li
                  v-for="(info, index) in pack.extraInformation"
                  v-bind:key="index"
                >
                  {{ info }}
                </li>
                <li v-if="packages.length === index">
                  Save up to
                  {{ calcSave(pack.price, pack.discount, pack.visit) }} SAR
                </li>
              </ul>
              <button class="close-button" @click="flipCardSwitch(pack.id)">
                Close
              </button>
            </ElCard>
          </div>
        </transition-group>
      </ElCarouselItem>
    </ElCarousel>
  </ElContainer>
</template>

<script>
export default {
  name: 'app',
  components: {},
  data() {
    return {
      flipCard: false,
      detailCard: [false, false, false],
      cards: [1, 2, 3],
      packages: [
        {
          id: 0,
          packageName: 'One-time cleaning',
          name: 'One-time cleaning',
          description:
            'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl.',
          isOneTime: true,
          price: 40,
          discount: 0,
          image:
            'https://images.unsplash.com/photo-1616161616161-1b1b1b1b1b1b?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80',
          extraInformation: [],
        },
        {
          id: 1,
          packageName: '1 visit per week package',
          name: '1 visit per week',
          description:
            'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl.',
          isOneTime: false,
          visit: 1,
          price: 30,
          discount: 0.12,
          image:
            'https://images.unsplash.com/photo-1616161616161-1b1b1b1b1b1b?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80',
          extraInformation: [
            '4 visits per month',
            'Same cleaner guaranteed',
            'Monthly contract',
            'Valid for 1 month, Available for all weekdays excluding Friday',
            'Save up to {{calcSave(pack.price, pack.discount, pack.visit)}} SAR',
          ],
        },
        {
          id: 2,
          packageName: '2 visits per week package',
          name: '2 visit per week',
          description:
            'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl.',
          isOneTime: false,
          visit: 2,
          price: 27,
          discount: 0.15,
          image:
            'https://images.unsplash.com/photo-1616161616161-1b1b1b1b1b1b?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80',
          extraInformation: [
            '8 visits per month',
            'Same cleaner guaranteed',
            'Monthly contract',
            'Valid for 1 month, Available for all weekdays excluding Friday',
            'Save up to {price} SAR',
          ],
        },
        {
          id: 3,
          packageName: '3 visits per week package',
          name: '3 visit per week',
          description:
            'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl.',
          isOneTime: false,
          visit: 3,
          price: 25,
          discount: 0.18,
          image:
            'https://images.unsplash.com/photo-1616161616161-1b1b1b1b1b1b?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80',
          extraInformation: [
            '12 visits per month',
            'Same cleaner guaranteed',
            'Monthly contract',
            'Valid for 1 month, Available for all weekdays excluding Friday',
            'Save up to {price} SAR',
          ],
        },
      ],
      selectedPackage: {},
    }
  },
  methods: {
    flipCardSwitch: function (id) {
      this.flipCard = !this.flipCard
      const transferFlip = [...this.detailCard]
      transferFlip[id] = !transferFlip[id]
      this.detailCard = transferFlip
    },
    calcSave(price, discount, visit) {
      return (40 - price * discount) * 4 * visit
    },
    handleSelectPackage: function (pack) {
    console.log(`LL: pack`, pack)
      this.selectedPackage = pack
      localStorage.setItem('selectedPackage', JSON.stringify(pack))
    },
  },
}
</script>

<style>
.package-card-header {
  display: flex;
  justify-items: center;
  justify-content: space-between;
}
.package-container {
  max-width: 800px;
  flex-direction: column;
  justify-items: center;
}
.price {
  font-size: 30px;
  font-weight: 600;
  color: #006658;
}
.price-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.save-tag {
  font-size: 18px;
  font-weight: 400;
  color: #e4e4e4;
  background-color: #006658;
  border-radius: 50px;
  border: none;
}
.grey-text {
  margin: 32px 0 8px 0;
  font-size: 22px;
  font-weight: 500;
  color: #8f8f8f;
}
.close-button,
.detail-button,
.select-button {
  display: flex;
  border-radius: 10px;
  margin-top: 16px;
  width: 100%;
  min-height: 45px;
  justify-content: center;
  padding: auto;
  align-items: center;
}
.select-button {
  color: #e4e4e4;
  background-color: #008000;
}
.close-button,
.detail-button {
  color: #000000;
  border: 2px solid #008000;
  background-color: #a5e887;
}
.close-button {
  color: #e4e4e4;
  background-color: #008000;
  position: absolute;
  bottom: 16px;
  width: 85%;
}
.front-select-button {
  margin-top: 8px;
}
.flipper,
.card {
  width: 400px;
  height: 330px;
  margin: 12px;
  border-radius: 13px;
}
.front,
.back {
  width: 400px;
  backface-visibility: hidden;
  position: absolute;
  top: 0;
  left: 0;
}

.front-content,
.back-content {
  border-radius: 13px;

  height: 350px;
  min-height: 350px;
}

.front-content {
  background-color: #a5e887;
}

.back-content {
  background-color: #00cc77;
  color: #f5f5f5;
}

.flip-enter-active,
.flip-leave-active {
  transition: all 0.6s ease;
}
.flip-enter,
.flip-leave-to {
  transform: rotateY(180deg);
}
</style>

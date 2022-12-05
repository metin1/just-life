<template>
  <ElContainer class="package-container" id="app">
    <h1>JUST LIFE Packages</h1>
    <ElCarousel
      type="card"
      arrow="always"
      :interval="0"
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
                  ? 'border: var(--secondary-color) 5px solid; '
                  : 'border: var(--light-color) 4px solid;'
              "
            >
              <div class="package-card-header" slot="header">
                <h3>{{ pack.name }}</h3>
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
                class="button detail-button front-detail-button"
                @click="flipCardSwitch(pack.id)"
              >
                Detail
              </button>
              <button
                class="button select-button front-select-button"
                @click="handleSelectPackage(pack)"
                :style="
                selectedPackage?.id === pack.id
                  ? 'backgroundColor: var(--secondary-color);'
                  : 'backgroundColor: var(--dark-color);'
              "
              >
                {{selectedPackage?.id === pack.id ? 'Selected' : 'Select'}}
              </button>
            </ElCard>
          </div>
          <div class="back" v-show="detailCard[pack.id]" key="back">
            <ElCard
              class="back-content"
              :style="
                selectedPackage?.id === pack.id
                  ? 'border: var(--secondary-color) 5px solid;'
                  : 'border: var(--light-color) 4px solid;'
              "
            >
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
                <li v-if="pack?.extraInformation?.length > 1">
                  Save up to {{ calcSave(pack.price, pack.visit) }} SAR
                </li>
              </ul>
              <button
                class="button close-button"
                @click="flipCardSwitch(pack.id)"
              >
                Close
              </button>
            </ElCard>
          </div>
        </transition-group>
      </ElCarouselItem>
    </ElCarousel>
    <div class="selection">
      <h4
        :style="
          selectedPackage?.name
            ? 'color: var(--dark-color)'
            : 'color: var(--secondary-color)'
        "
      >
        Selected Package: {{ selectedPackage?.name || 'none' }}
      </h4>
      <p v-if="!!selectedPackage?.name">
        If you are ready to proceed, please click the button below.
      </p>
      <p v-else>Please select a package to proceed.</p>
      <button class="button next-button" :disabled="!selectedPackage?.name">
        Next
      </button>
    </div>
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
          extraInformation: ["We'll clean your home from top to bottom"],
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
          ],
        },
      ],
      selectedPackage: {},
    }
  },
  methods: {
    flipCardSwitch: function (id) {
      this.flipCard = !this.flipCard
      const transferFlip = [false, false, false]
      transferFlip[id] = !this.detailCard[id]
      this.detailCard = transferFlip
    },
    calcSave(price, visit) {
      return (40 - price) * 4 * visit
    },
    handleSelectPackage: function (pack) {
      if (this.selectedPackage?.name === pack.name) {
        this.selectedPackage = {}
      } else {
        this.selectedPackage = pack
      }
      this.detailCard = [false, false, false]
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
  font-size: 16px;
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
.button {
  display: flex;
  border-radius: 10px;
  margin-top: 16px;
  width: 100%;
  min-height: 45px;
  font-size: 0.9rem;
  cursor: pointer;
  justify-content: center;
  align-items: center;
}

.button:hover {
  font-size: 1.1rem;
}
.button:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}
.select-button {
  color: #e4e4e4;
  background-color: var(--dark-color);
}
.next-button {
  color: #e4e4e4;
  background-color: var(--secondary-color);
}
.close-button,
.detail-button {
  color: #000000;
  border: 2px solid var(--dark-color);
  background-color: var(--light-color);
}
.close-button {
  color: #e4e4e4;
  background-color: var(--dark-color);
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
  height: 360px;
  margin: 12px;
  border-radius: 13px;
}
.front,
.back {
  width: 390px;
  backface-visibility: hidden;
  position: absolute;
  top: 0;
  left: 0;
}

.front-content,
.back-content {
  border-radius: 13px;
  height: 370px;
  min-height: 370px;
}

.front-content {
  background-color: var(--light-color);
}

.back-content {
  background-color: var(--primary-color);
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

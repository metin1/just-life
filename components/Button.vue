<template>
  <ElContainer class="package-container" id="app">
    <h1>JUST LIFE Packages</h1>
    <ElCarousel
      type="card"
      arrow="always"
      :interval="40000"
      :autoplay="false"
      height="330px"
      width="300px"
    >
      <ElCarouselItem
        class="carousel-container"
        v-for="pack in packages"
        v-bind:key="pack"
      >
        <transition-group name="flip" tag="div" class="flipper">
          <div class="front" v-show="!detailCard[pack.id]" key="front">
            <ElCard class="front-content w-full">
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
                class="detail-button"
                @click="flipCardSwitch(pack.id)"
              >
                Detail
              </button>
            </ElCard>
          </div>
          <div class="back" v-show="detailCard[pack.id]" key="back">
            <ElCard class="back-content">
              <div slot="header">
                <h3>{{ pack.name }}</h3>
                <ElButton
                  type="primary"
                  style="float: right; padding: 0 1px"
                  @click="flipCardSwitch(pack.id)"
                >
                  Select
                </ElButton>
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
  methods: {
    flipCardSwitch: function (id) {
      console.log(`LL: id`, id)
      this.flipCard = !this.flipCard
      const transferFlip = [...this.detailCard]
      transferFlip[id] = !transferFlip[id]
      console.log(`LL: transferFlip`, transferFlip)
      this.detailCard = transferFlip
      console.log(`LL: this.detailCard `, this.detailCard)
    },
    calcSave(price, discount, visit) {
      return (40 - price * discount) * 4 * visit
    },
  },
}
</script>

<style>
/* .carousel-container {
  max-width: 600px;
} */
.package-card-header {
  display: flex;
  justify-items: center;
  justify-content: space-between;
}
.package-container {
  max-width: 800px;
  /* display: flex; */
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
  margin: 22px 0 8px 0;
  font-size: 22px;
  font-weight: 500;
  color: #8f8f8f;
}
.detail-button {
  display: flex;
  color: #e4e4e4;
  background-color: #008000;
  border-radius: 10px;
  margin-top: 16px;
  width: 100%;
  min-height: 45px;
  justify-content: center;
  padding: auto;
  align-items: center;
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
  border: #ebebeb 4px solid;
  height: 290px;
  min-height: 290px;
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

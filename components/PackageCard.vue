<template>
  <ElContainer id="app">
    <div class="card" v-for="pack in packages" v-bind:key="pack">
      <div class="flipper">
        <transition-group name="flip" tag="div">
          <div class="front" v-show="!detailCard[pack.id]" key="front">
            <ElCard class="w-full">
              <div slot="header">
                <span>card n°{{ pack.id }}</span>
                <ElButton
                  type="primary"
                  style="float: right; padding: 0 1px"
                  @click="flipCardSwitch(pack.id)"
                >
                  Detail
                  {{detailCard[pack.id] ? "on" : "off"}}
                </ElButton>
              </div>
              <div>{{pack.description}}</div>
            </ElCard>
          </div>
          <div class="back" v-show="detailCard[pack.id]" key="back">
            <ElCard class="back-content">
              <div slot="header">
                <span>card n°{{ pack.id }}</span>
                <ElButton
                  type="primary"
                  style="float: right; padding: 0 1px"
                  @click="flipCardSwitch(pack.id)"
                >
                  Select
                </ElButton>
              </div>
              <ul v-for="info in pack.extraInformation" v-bind:key="info">
                <li>{{info}}</li>
              </ul>
            </ElCard>
          </div>
        </transition-group>
      </div>
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
          price: 100,
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
          price: 30,
          discount: 0.12,
          image:
            'https://images.unsplash.com/photo-1616161616161-1b1b1b1b1b1b?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80',
          extraInformation: [
            '4 visits per month',
            'Same cleaner guaranteed',
            'Monthly contract',
            'Valid for 1 month, Available for all weekdays excluding Friday',
            'Save up to {price} SAR',
          ],
        },
        {
          id: 2,
          packageName: '2 visits per week package',
          name: '2 visit per week',
          description:
            'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl. Donec auctor, nisl eget ultricies tincidunt, nunc nisl aliquam nisl, eget aliquam nisl nunc eget nisl.',
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
    }
  },
  methods: {
    flipCardSwitch: function (id) {
      console.log(`LL: id`, id)
      this.flipCard = !this.flipCard
      const transferFlip = [...this.detailCard]
      transferFlip[id] = !transferFlip[id]
      console.log(`LL: transferFlip`, transferFlip)
      this.detailCard = transferFlip
      console.log(`LL: this.detailCard `, this.detailCard )
    },
  },
}
</script>

<style>
.card {
  position: relative;
  width: 270px;
  height: 330px;
  margin: 12px;
}

.front,
.back {
  width: 270px;
  height: 330px;
}

/* hide back of pane during swap */
.front,
.back {
  backface-visibility: hidden;
  position: absolute;
  top: 0;
  left: 0;
}

/* back, initially hidden pane */
.back-content {
  background-color: #e3e3e3;
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

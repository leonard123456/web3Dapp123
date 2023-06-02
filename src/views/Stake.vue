<template>
  <div class="stake-view">
    <div class="container mini">
      <h1>質押</h1>
      <div>
        <div>
          <div>
            <p>sFISH Price ｜ {{ SFish_USDPrice }}</p>
            <p>Value ｜ 1個 sFISH = {{ Fish_SFishPrice }} 個FISH</p>
            <p>Your FISH Balance ｜ {{ FISHBalanceOf }} （FISH）</p>
            <p>You have staked sFISH ｜ {{ SFISHBalanceOf }} （sFISH）</p>
          </div>

          <div>
            <div>
              <div>
                <div class="row">
                  <div class="col-6">
                    <div
                      type="button"
                      @click="reverse()"
                      :class="!stakeState ? '' : 'action'"
                    >
                      Stake
                    </div>
                  </div>
                  <div class="col-6">
                    <div
                      type="button"
                      @click="reverse()"
                      :class="stakeState ? '' : 'action'"
                    >
                      unStake
                    </div>
                  </div>
                </div>
              </div>
              <div>
                <div class="row">
                  <div class="col text-start stake-left-table-thead">
                    {{ stakeState ? 'FISH Balance' : 'sFISH Balance' }}
                  </div>
                  <div class="col text-end">
                    {{ stakeState ? FISHBalanceOf : SFISHBalanceOf }}
                  </div>
                </div>
                <div class="input-group mb-3" v-if="stakeState">
                  <span class="input-group-text" id="inputGroup-sizing-default"
                    >Fish</span
                  >
                  <input
                    type="number"
                    class="form-control"
                    v-model="fishInput"
                  />
                </div>
                <div class="input-group mb-3" v-else>
                  <span class="input-group-text" id="inputGroup-sizing-default"
                    >sFish</span
                  >
                  <input
                    type="number"
                    class="form-control"
                    v-model="sFishInput"
                  />
                </div>
              </div>

              <button
                type="button"
                class="btn btn-warning stake-btn"
                @click="apporve()"
                v-if="
                  (stakeState && !fishIsApporve) ||
                  (!stakeState && !sFishIsApporve)
                "
              >
                APPROVE
              </button>
              <button
                type="button"
                class="btn btn-warning stake-btn"
                @click="stake()"
                v-if="fishIsApporve && stakeState"
              >
                Stake
              </button>
              <button
                type="button"
                class="btn btn-warning stake-btn"
                @click="unStake()"
                v-if="sFishIsApporve && !stakeState"
              >
                unStake
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const TokenIcon = () => import('@/components/UiComponents/TokenIcon');
import stakeMixin from '@/mixins/stake.js';
export default {
  mixins: [stakeMixin],
  components: {
    TokenIcon,
  },
  data() {
    return {
      connectLoader: 0,
      cefreshLoader: false,
    };
  },
  computed: {
    pools() {
      return this.$store.getters.getPools;
    },
  },
  created() {
    const isConnected = this.$store.getters.getWalletIsConnected;

    if (!isConnected) {
      this.$router.push({ name: 'home' });
      alert('Please link your wallet first');
      return false;
    }
  },
};
</script>

<style lang="scss" scoped>
.action {
  background-color: #ffc107;
  color: #000;
}
</style>

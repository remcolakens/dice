<template>
  <div class="playfield">
    <div class="cube-wrap">
      <div ref="cube" class="cube">
        <div class="face"><span class="dots">1</span></div>
        <div class="face"><span class="dots">2</span></div>
        <div class="face"><span class="dots">3</span></div>
        <div class="face"><span class="dots">4</span></div>
        <div class="face"><span class="dots">5</span></div>
        <div class="face"><span class="dots">6</span></div>
      </div>
    </div>

    <button style="position: absolute; top: 0; right: 0;" @click="roll">
      Roll
    </button>
    <button style="position: absolute; top: 20px; right: 0;" @click="reset">
      Reset
    </button>
  </div>
</template>

<script>
import { TimelineLite, Power0, Sine } from "gsap";
import "../customEase";

export default {
  name: "dice",
  props: ["magicNumber"],

  data() {
    return {
      tl: null,
      cube: null,
      faces: { top: 1, left: 2, front: 3, back: 4, right: 5, bottom: 6 },
      result: null,
      flips: null,

      amountOfCalculations: null
    };
  },

  mounted() {
    this.cube = this.$refs.cube;
    this.tl = new TimelineLite();
  },

  methods: {
    reset() {
      this.tl.restart();
      this.tl.clear();
      this.result = this.faces;
    },

    calcScore(X, Y, Z) {
      const rotateX = face => ({
        top: face.back,
        left: face.left,
        front: face.top,
        bottom: face.front,
        right: face.right,
        back: face.bottom
      });

      for (let turn = 0; turn < X; turn++) {
        this.result = rotateX(this.result);
      }

      const rotateY = face => ({
        top: face.top,
        left: face.back,
        front: face.left,
        bottom: face.bottom,
        right: face.front,
        back: face.right
      });

      for (let turn = 0; turn < Y; turn++) {
        this.result = rotateY(this.result);
      }

      const rotateZ = face => ({
        top: face.left,
        left: face.bottom,
        front: face.front,
        bottom: face.right,
        right: face.top,
        back: face.back
      });

      for (let turn = 0; turn < Z; turn++) {
        this.result = rotateZ(this.result);
      }
    },

    generateFlips() {
      this.flips = null;

      this.flips = {
        X: Math.floor(Math.random() * 5 + 1),
        Y: Math.floor(Math.random() * 4 + 1),
        Z: Math.floor(Math.random() * 5 + 1)
      };

      if (this.flips.X >= 3) {
        if (this.flips.Z > 3 || this.flips.Y > 3) {
          this.generateFlips();
        }
      }

      if (this.flips.Y >= 3) {
        if (this.flips.Z > 4 || (this.flips.X <= 1 && this.flips.X > 3)) {
          this.generateFlips();
        }
      }

      if (this.flips.Z >= 3) {
        if (this.flips.X > 4 || this.flips.Y >= 3) {
          this.generateFlips();
        }
      }

      this.amountOfCalculations++;
    },

    roll() {
      this.reset();
      this.generateFlips();
      this.calcScore(this.flips.X, this.flips.Y, this.flips.Z);

      // console.log(`X: ${this.flips.X}`);
      // console.log(`Y: ${this.flips.Y}`);
      // console.log(`Z: ${this.flips.Z}`);

      if (parseFloat(this.result.top) !== parseFloat(this.magicNumber)) {
        this.roll();
      } else {
        console.log(`System demands: ${parseFloat(this.magicNumber)}`);
        console.log(`You've got: ${this.result.top} sorry system is boss`);
        console.log(
          `We did in total: ${parseFloat(
            this.amountOfCalculations
          )} calculations`
        );
        this.amountOfCalculations = null;
        // const randomTimer = parseFloat(Math.random() * 1 + 1.1).toFixed(1);

        this.tl.to(this.cube, 1.4, {
          y: 300,
          ease: CustomEase.create(
            "custom",
            "M0,0 C0.14,0 0.242,0.438 0.272,0.561 0.313,0.728 0.354,1.037 0.362,1.074 0.37,1.059 0.414,0.873 0.455,0.811 0.51,0.726 0.547,0.753 0.56,0.762 0.636,0.812 0.659,1.008 0.666,1.026 0.728,0.942 0.815,0.936 0.834,0.95 0.853,0.964 0.882,1.004 0.896,1.018 0.907,1.014 0.939,0.984 0.954,0.984 0.969,0.984 1,1 1,1"
          )
        });
        this.tl.to(
          this.cube,
          1.4,
          { x: Math.random() * 450 + 250, ease: Power0.linear },
          `-=${1.4}`
        );

        this.tl.to(
          this.cube,
          1.3,
          { rotationX: -90 * this.flips.X, ease: Power0.linear },
          `-=${1.4}`
        );
        this.tl.to(
          this.cube,
          1.2,
          { rotationY: 90 * this.flips.Y, ease: Sine.easeIn },
          `-=${1.4}`
        );
        this.tl.to(
          this.cube,
          1.3,
          { rotationZ: 90 * this.flips.Z, ease: Sine.easeIn },
          `-=${1.4}`
        );
      }
    }
  }
};
</script>

<style lang="scss" scoped>
$dark100: #111;
$dark200: #333;
$dark300: #555;

$cube-red: rgba(200, 0, 0, 1);
$cube-red-2: rgba(190, 0, 0, 1);
$cube-red-3: rgba(180, 0, 0, 1);
$cube-red-4: rgba(170, 0, 0, 1);
$cube-red-5: rgba(160, 0, 0, 1);
$cube-red-6: rgba(150, 0, 0, 1);

$dot-color: #ededed;

$darken: rgba(0, 0, 0, 0.4);

.playfield {
  background: $dark200;
  background-image: radial-gradient($dark300, $dark100);
  position: relative;
  height: 600px;
  width: 1000px;
  overflow: hidden;
}

.cube-wrap {
  position: absolute;
  perspective: 2000px;
}

.cube {
  position: relative;
  width: 200px;
  height: 200px;
  transform-style: preserve-3d;
}

.face {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: $cube-red;
  background-image: radial-gradient(transparent, $darken);
  backface-visibility: visible;
}

.dots {
  display: inline-block;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  font-size: 0;
  background-color: $dot-color;
  position: absolute;
  left: 20px;
  bottom: 20px;
}

.face {
  background-color: red;
  opacity: 1;

  &:nth-child(1) {
    transform: rotateZ(0deg) rotateY(0deg) translateZ(100px);

    .dots {
      box-shadow: 120px -120px 0 0 $dot-color, 60px -60px 0 0 $dot-color,
        1px 1px 3px 1px $darken, 121px -121px 3px 1px $darken,
        61px -61px 3px 1px $darken;
    }
  }

  &:nth-child(2) {
    background-color: $cube-red-2;
    transform: rotateZ(0deg) rotateY(0deg) translateZ(-100px);

    .dots {
      box-shadow: 120px 0 0 0 $dot-color, 0 -120px 0 0 $dot-color,
        120px -120px 0 0 $dot-color, 1px 1px 3px 1px $darken,
        121px 1px 3px 1px $darken, 1px -121px 3px 1px $darken,
        121px -121px 3px 1px $darken;
    }
  }

  &:nth-child(3) {
    background-color: $cube-red-3;
    transform: rotateZ(0deg) rotateY(90deg) translateZ(-100px);

    .dots {
      box-shadow: 120px -120px 0 0 $dot-color, 1px 1px 3px 1px $darken,
        121px -121px 3px 1px $darken;
    }
  }

  &:nth-child(4) {
    background-color: $cube-red-4;
    transform: rotateZ(90deg) rotateY(90deg) translateZ(-100px);

    .dots {
      left: 80px;
      bottom: 80px;
      box-shadow: 1px 1px 3px 1px $darken;
    }
  }

  &:nth-child(5) {
    background-color: $cube-red-5;
    transform: rotateZ(0deg) rotateY(-90deg) translateZ(-100px);

    .dots {
      box-shadow: 120px 0 0 0 $dot-color, 0 -120px 0 0 $dot-color,
        120px -120px 0 0 $dot-color, 60px -60px 0 0 $dot-color,
        121px 1px 3px 1px $darken, 1px -121px 3px 1px $darken,
        121px -121px 3px 1px $darken, 61px -61px 3px 1px $darken,
        1px 1px 3px 1px $darken;
    }
  }

  &:nth-child(6) {
    background-color: $cube-red-6;
    transform: rotateZ(90deg) rotateY(90deg) translateZ(100px);

    .dots {
      box-shadow: 60px 0 0 0 $dot-color, 120px 0 0 0 $dot-color,
        0 -120px 0 0 $dot-color, 60px -120px 0 0 $dot-color,
        120px -120px 0 0 $dot-color, 61px 1px 3px 1px $darken,
        121px 1px 3px 1px $darken, 1px -121px 3px 1px $darken,
        61px -121px 3px 1px $darken, 121px -121px 3px 1px $darken;
    }
  }
}
</style>

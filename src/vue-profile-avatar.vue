<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'ProfileAvatar',
  props: {
    username: { type: String, default: 'Username', required: true },
    size: { type: String, default: 's', required: false },
    customSize: { type: String, required: false },
    border: { type: Boolean, default: true, required: false },
    bgColor: { type: String, required: false },
    borderColor: { type: String, required: false },
    textColor: { type: String, required: false },
    colorType: { type: String, required: false, default: 'normal' },
    image: { type: String, required: false },
  },
  data() {
    return {
      normalBackgroundColors: {
        a: '#FF6633', b: '#FFB399',
        c: '#809900', d: '#FFFF99',
        e: '#00B3E6', f: '#E6B333',
        g: '#3366E6', h: '#999966',
        i: '#99FF99', j: '#B34D4D',
        k: '#80B300', l: '#FF33FF',
        m: '#E6B3B3', n: '#6680B3',
        o: '#6666FF', p: '#FF99E6',
        q: '#CCFF1A', r: '#FF1A66',
        s: '#E6331A', t: '#33FFCC',
        u: '#66994D', v: '#B366CC',
        w: '#4D8000', x: '#B33300',
        y: '#CC80CC', z: '#66664D',
        1: '#4D8066', 2: '#809980',
        3: '#E6FF80', 4: '#1AFF33',
        5: '#999933', 6: '#FF3380',
        7: '#CCCC00', 8: '#66E64D',
        9: '#4D80CC', 0: '#9900B3',
        else: '#6666FF', ñ: '#6666FF'
      },
      pastelBackgroundColors: {
        a: '#FCFFA6', b: '#C1FFD7',
        c: '#B5DEFF', d: '#CAB8FF',
        e: '#F38BA0', f: '#EDF6E5',
        g: '#FFBCBC', h: '#B5EAEA',
        i: '#FFFDDE', j: '#D9D7F1',
        k: '#E7FBBE', l: '#FFCBCB',
        m: '#FCFFA6', n: '#C1FFD7',
        o: '#B5DEFF', p: '#CAB8FF',
        q: '#F38BA0', r: '#EDF6E5',
        s: '#FFBCBC', t: '#B5EAEA',
        u: '#FFFDDE', v: '#D9D7F1',
        w: '#E7FBBE', x: '#FFCBCB',
        y: '#FCFFA6', z: '#C1FFD7',
        ñ: '#B5DEFF', 1: '#CAB8FF',
        2: '#F38BA0', 3: '#EDF6E5',
        4: '#FFBCBC', 5: '#B5EAEA',
        6: '#FFFDDE', 7: '#D9D7F1',
        8: '#E7FBBE', 9: '#FFCBCB',
        0: '#B5DEFF', else: '#CAB8FF'
      },
      brownieBackgroundColors: {
        a: '#ffc7a4', b: '#d69e7c',
        c: '#ab7656', d: '#815133',
        e: '#582e12', f: '#ffe599',
        g: '#ffb363', h: '#ad5900',
        i: '#7f3509', j: '#c6a186',
        k: '#9b6a5b', l: '#573333',
        m: '#cbb397', n: '#ffc7a4',
        o: '#d69e7c', p: '#ab7656',
        q: '#815133', r: '#582e12',
        s: '#ffe599', t: '#ffb363',
        u: '#ad5900', v: '#7f3509',
        w: '#c6a186', x: '#9b6a5b',
        y: '#573333', z: '#cbb397',
        ñ: '#ffc7a4', 1: '#d69e7c',
        2: '#ab7656', 3: '#815133',
        4: '#582e12', 5: '#ffe599',
        6: '#ffb363', 7: '#ad5900',
        8: '#7f3509', 9: '#c6a186',
        0: '#9b6a5b', else: '#573333'
      },
      imageError: false
    };
  },
  computed: {
    avatarClass() {
      return {
        'small': this.size === 's',
        'medium': this.size === 'm',
        'large': this.size === 'l',
      }
    },
    isBorder() {
      return {
        'border': this.border,
      }
    },
    initials() {
      this.backgroundColor
      return this.getInitials(this.username)
    },
    backgroundColor() {
      return this.getColorByInitial(this.getInitials(this.username)[0])
    },
  },
  methods: {
    getInitials(username) {
      const usernameParts = username.split(' ')

      const initials = usernameParts.map((e) => {
        return e.slice(0, 1).toUpperCase()
      })

      if (initials.length > 3) return initials.slice(0, 3).join('')

      return initials.join('')
    },
    getColorByInitial(initial) {
      if (this.bgColor) return this.bgColor

      if (this.colorType === 'normal') return this.normalBackgroundColors[initial.toLowerCase()]

      if (this.colorType === 'pastel') return this.pastelBackgroundColors[initial.toLowerCase()]

      if (this.colorType === 'brownie') return this.brownieBackgroundColors[initial.toLowerCase()]

      return this.normalBackgroundColors[initial.toLowerCase()]
    },
    getBorderColor(color, percent) {
      if (this.image) return 'black'

      if (this.borderColorPropExists()) return this.borderColor

      return this.shadeColor(color, percent)
    },
    getTextColor(color, percent) {
      if (this.textColorPropExists()) return this.textColor

      return this.shadeColor(color, percent)
    },
    shadeColor(color, percent) {
      var R = parseInt(color.substring(1,3),16);
      var G = parseInt(color.substring(3,5),16);
      var B = parseInt(color.substring(5,7),16);

      R = parseInt(R * (100 + percent) / 100);
      G = parseInt(G * (100 + percent) / 100);
      B = parseInt(B * (100 + percent) / 100);

      R = (R<255)?R:255;
      G = (G<255)?G:255;
      B = (B<255)?B:255;

      var RR = ((R.toString(16).length==1)?"0"+R.toString(16):R.toString(16));
      var GG = ((G.toString(16).length==1)?"0"+G.toString(16):G.toString(16));
      var BB = ((B.toString(16).length==1)?"0"+B.toString(16):B.toString(16));

      return "#"+RR+GG+BB;
    },
    borderColorPropExists() {
      if (this.borderColor) return true
      return false
    },
    textColorPropExists() {
      if (this.textColor) return true
      return false
    }
  },
});
</script>

<template>
  <div
    class="avatarContainer"
    :class="[avatarClass, isBorder]"
    :style="{
      height: customSize,
      width: customSize,
      backgroundColor: backgroundColor,
      color: getTextColor(backgroundColor, -45),
      borderColor: getBorderColor(backgroundColor, -45),
    }"
  >
    <span v-if="!image || imageError" class="text"> {{initials}} </span>
    <img v-if="image && !imageError" :src="image" class="image" alt="Avatar" @error="imageError = true"/>
  </div>
</template>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@600&display=swap');

  .avatarContainer {
    display: flex;
    border-radius: 100%;
    background-color: #6666FF;
    overflow: hidden;
  }

  .small {
    height: 50px;
    width: 50px;
  }

  .medium {
    height: 60px;
    width: 60px;
  }

  .large {
    height: 75px;
    width: 75px;
    font-size: 20px;
  }

  .text {
    margin: auto;
    font-family: 'Montserrat', sans-serif;
  }

  .border {
    border: 2px solid black;
  }

  .image {
    width: 100%;
    height: 100%;
  }


</style>

<template>
  <div class="hanged">
    <svg
      width="290px"
      height="500px"
      viewBox="0 0 290 500"
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
    >
      <defs></defs>
      <g id="hanged-man">
        <g id="man">
          <circle
            v-if="badAttemps > 0"
            id="head"
            stroke="#151515"
            fill="none"
            stroke-width="3"
            cx="222.5"
            cy="117.5"
            r="33.5"
          ></circle>
          <rect
            v-if="badAttemps > 1"
            id="body"
            fill="#151515"
            transform="translate(222.500000, 226.500000) scale(-1, 1) translate(-222.500000, -226.500000) "
            x="221"
            y="151"
            width="3"
            height="151"
          ></rect>
          <polygon
            v-if="badAttemps > 2"
            id="right-arm"
            fill="#151515"
            points="222.549038 151.24241 161.049038 225.057466 158.450962 224.018023 219.950962 150.202967"
          ></polygon>
          <polygon
            v-if="badAttemps > 3"
            id="left-arm"
            fill="#151515"
            points="286.799038 223.971604 284.200962 225.011046 222.200962 150.595867 224.799038 149.556425"
          ></polygon>
          <rect
            v-if="badAttemps > 4"
            id="right-leg"
            fill="#151515"
            transform="translate(192.500000, 351.500000) scale(-1, 1) rotate(-30.000000) translate(-192.500000, -351.500000) "
            x="191"
            y="292"
            width="3"
            height="119"
          ></rect>
          <rect
            v-if="badAttemps > 5"
            id="left-leg"
            fill="#151515"
            transform="translate(252.500000, 351.500000) scale(-1, -1) rotate(-30.000000) translate(-252.500000, -351.500000) "
            x="251"
            y="292"
            width="3"
            height="119"
          ></rect>
        </g>
        <g id="pole">
          <rect
            id="Rectangle"
            fill="#151515"
            transform="translate(224.500000, 42.000000) scale(-1, 1) translate(-224.500000, -42.000000) "
            x="223"
            y="0"
            width="3"
            height="84"
          ></rect>
          <rect
            id="Rectangle"
            fill="#151515"
            transform="translate(113.500000, 1.500000) scale(-1, 1) rotate(-270.000000) translate(-113.500000, -1.500000) "
            x="112"
            y="-111"
            width="3"
            height="225"
          ></rect>
          <rect
            id="Rectangle"
            fill="#151515"
            transform="translate(1.500000, 250.000000) scale(-1, 1) translate(-1.500000, -250.000000) "
            x="0"
            y="0"
            width="3"
            height="500"
          ></rect>
        </g>
      </g>
    </svg>
  </div>
</template>

<script>
export default {
  name: 'HangedMan',
  props: {
    badAttemps: {
      type: Number,
      default: 0,
    },
  },
};
</script>

<style>
</style>
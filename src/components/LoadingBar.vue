<template>
  <div :style="loadingBarStyle" :class="['loading-bar', determined ? 'determined': 'undetermined', posBottom ? 'position-bottom': 'position-top', cssClasses]">
	  <div :style="progressBarStyle" class="loading-bar-progress"></div>
  </div>
</template>

<script>
export default {
  name: 'loading-bar',
  props: {
    cssClasses: { //TODO add limit to only accept acknoledged css classes
  	type: String,
  	required: false,
    },
	size: { //TODO add support for predefined sizes.
  	  type: String,
  	  required: false,
  	  default: "1em",
    },
	progress: {
  	  type: Number,
  	  required: false,
  	  default: 50,
    },
    posBottom: {
  	  type: Boolean,
  	  required: false,
  	  default: false,
    },
    determined: {
  	  type: Boolean,
  	  required: false,
  	  default: false,
    },
	color: {
		type: String,
		required: false,
		default: 'rgb(205, 27, 106)',
	}
  },
  computed: {
	  computedPosition () {
		this.position === "top" ? 'top' : 'bottom';
	  },
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
	  loadingBarStyle: {
		  'position': this.computedPosition,
		  'font-size': this.size,
		  'line-height': this.size,
		  'height': this.size,
	  },
	  progressBarStyle: {
		  'width': this.progress + "%",
		  'color': this.color,
	  },
    }
  }
}
</script>

<style lang="scss" scoped>

.loading-bar {
	/*position: absolute;
	width: 100%;*/
	background-color: rgb(152, 165, 166);
	&.position-top {
		top: 0px;
	}
	&.position-bottom {
		bottom: 0px;
	}
	.loading-bar-progress {
		position: relative;
		height: 100%;
		background-color: rgb(205, 27, 106);
		&:after {
			content: '';
			position: absolute;
			top: 0;
			left: 50%;
			margin-left: -10%;
			width: 20%;
			height:100%;
			background-color: rgb(152, 165, 166);
		}
	}
	&.determined {
		text-align: left;
	}
	&.undetermined {
		text-align: center;
		.loading-bar-progress {
			margin: 0 auto;
		}
	}
}
</style>

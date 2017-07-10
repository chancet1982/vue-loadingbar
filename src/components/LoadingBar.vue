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
	progress: { //TODO ensure that this is only being used if loading-bar type is determined
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
	color: { //TODO consider parsing valid and invalid styles using regex
		type: String,
		required: false,
		default: 'rgb(205, 27, 106)',
	},
	loading: {
		type: Boolean,
		required: false,
		default: true // temp while developing
	}
  },
  computed: {
	  computedPosition () {
		this.position === "top" ? 'top' : 'bottom';
	  },
	  progressBarStyle () {
		return {
	  		  'width': this.progress + '%',
	  		  'color': this.color,
		}
	  },
  },
  methods: {
	  isLoading () {
		  const loadingState = this.loading;
		  const payload = {
			  loading: loadingState
		  };
		  this.$emit("loading-bar-state-change", payload);
		  loadingState ? this.$emit("loading-bar-loading") : this.$emit("loading-bar-not-loading");
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
		  'background-color': 'rgb(152, 165, 166)',
		  'border-radius': 5 + 'px',
	  },
    }
  }
}
</script>

<style lang="scss" scoped>

.loading-bar {
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	overflow: hidden;
	&.position-top {
		top: 0px;
	}
	&.position-bottom {
		bottom: 0px;
	}
	.loading-bar-progress {
		position: relative;
		height: 100%;
		width: 50%;
		background-color: rgba(23, 182, 194, 0.2);
	}
	&.determined {
		top: 0%;
		left: 0%;

	}
	&.undetermined {
		.loading-bar-progress {
			top: 0%;
			margin: 0 auto;
			animation: scaling 2s ease-in-out infinite alternate ;
			-webkit-animation: scaling 2s ease-in-out infinite alternate ;
			&:before,
			&:after {
				content: '';
				position: absolute;
				top: 0;
				width: 50%;
				height:100%;
				background-color: rgba(23, 182, 194, 1);
				animation: opacity 2s ease-in-out infinite alternate ;
				-webkit-animation: opacity 2s ease-in-out infinite alternate ;
			}
			&:before {
				left: 0%;
				transform: translateX(-100%);

			}
			&:after {
				right: 0%;
				transform: translateX(100%);
			}
		}
	}
}
@keyframes opacity {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
    }
}

@keyframes scaling {
    0% {
    	opacity: 0;
        transform: scale(0, 1)  ;
    }
    100% {
    	opacity: 1;
        transform: scale(1, 1) ;
    }
}
</style>

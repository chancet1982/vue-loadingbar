<template>
  <div v-if="loadingData" :style="loadingBarStyle" :class="['loading-bar',{'loading': isLoading}, determined ? 'determined': 'undetermined', posBottom ? 'position-bottom': 'position-top', cssClasses]">
	  <div :style="progressBarStyle" class="loading-bar-progress"></div>
  </div>
</template>

<script>
import axios from 'axios';
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
  	  default: "0.5em",
    },
	progress: { //TODO ensure that this is only being used if loading-bar type is determined
  	  type: Number,
  	  required: false,
  	  default: 0,
    },
    posBottom: {
  	  type: Boolean,
  	  required: false,
  	  default: false,
    },
    determined: {
  	  type: Boolean,
  	  required: false,
  	  default: true,
    },
	color: { //TODO consider parsing valid and invalid styles using regex
		type: String,
		required: false,
		default: 'rgb(205, 27, 106)',
	},
	loading: {
		type: Boolean,
		required: false,
		default: false,
	},
	finishedDelay: {
		type: Number,
		required: false,
		default: 2000,
	}
  },
  mounted(){
	  /*
	  Lets add a request interceptor to start loading whenever a
	  request is being made. this should be done Globally but its nice
	  to have it here for testing.
	   */
	axios.interceptors.request.use( (request) => {
		this.startLoading();
		return request;
	}, function (error) {
		return Promise.reject(error);
	});

	// Add a response interceptor
	axios.interceptors.response.use( (response) => {
		this.finishLoading();
		return response;
	}, function (error) {
		return Promise.reject(error);
	});

	this.getData();
  },
  computed: {
	  computedPosition () {
		this.position === "top" ? 'top' : 'bottom';
	  },
	  progressBarStyle () {
		return {
	  		  'width': this.progressData + '%',
	  		  'color': this.color,
		}
	  },
	  isLoading () {
		  const loadingInProgress = this.progressData < 100 && this.loadingData;
		  const payload = {
			  loading: loadingInProgress
		  };
		  this.$emit("loading-bar-state-change", payload);
		  loadingInProgress ? this.$emit("loading-bar-loading") : this.$emit("loading-bar-not-loading");
		  return loadingInProgress;
	  },
  },
  methods: {
	  startLoading () {
		this.loadingData = true;
		this.progressData = 0;
		const progressAway = () => {
			if (this.progressData < 100) {
				this.incProgress(10);
			} else {
				clearInterval(fauxProgress);
				this.finishLoading();
			}
		}
		const fauxProgress = setInterval( progressAway, 100);
	  },
	  incProgress(step) {
		  this.progressData += step;
	  },
	  finishLoading () {
		this.progressData = 100;
		setTimeout( () => {
			this.loadingData = false;
		}, this.finishedDelay);
	  },
	  getData () {
		  //this.startLoading();
		  axios.get("http://www.reddit.com/r/pics.json").then((response) => {
			console.log(response);
			//this.finishLoading();
		  });
	  }
  },
  data () {
    return {
      loadingData: this.loading,
	  progressData: this.progress,
	  loadingBarStyle: {
		  'position': this.computedPosition,
		  'font-size': this.size,
		  'line-height': this.size,
		  'height': this.size,
		  'background-color': 'rgb(152, 165, 166)',
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
	opacity: 0;
	-webkit-transition: opacity 300ms 1000ms linear ;
	transition: opacity 300ms 1000ms linear ;
	&.loading {
		opacity: 1;
	}
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
		.loading-bar-progress {
			background-color: rgba(23, 182, 194, 1);
			-webkit-transition: width 500ms ease-in-out, ;
	    	transition: width 500ms ease-in-out, ;
		}
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

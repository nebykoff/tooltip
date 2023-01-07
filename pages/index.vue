<template>
    <div :class="$style.IndexPage">
        <div
            :class="$style.room"
        >
            <img
                ref="image"
                src="/images/bg.jpg"
                alt=""
            >
            <div
                ref="parent"
                :class="$style.points"
            >
                <div
                    v-for="(point, idx) in points"
                    :key="`point_${idx}`"
                    :style="{top: point.coords[1]+'%', left: point.coords[0]+'%'}"
                    :class="[$style.point, {[$style._active]: activePoint === idx}]"
                    @click="handlePointClick(idx)"
                >
                    {{ point.title }}

                    <div
                        ref="tooltip"
                        :class="$style.tooltip"
                    >
                        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Cumque enim harum
                        possimus tempore unde. Ea, eos fugiat ipsam iste laudantium quasi qui quod
                        repellat sint? Doloribus earum error perspiciatis quam!
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'IndexPage',

    data() {
        return {
            points: [
                {
                    title: '1',
                    coords: [10, 10],
                },
                {
                    title: '2',
                    coords: [20, 20],
                },
                {
                    title: '3',
                    coords: [50, 50],
                },
                {
                    title: '4',
                    coords: [95, 20],
                },
                {
                    title: '5',
                    coords: [20, 85],
                },
            ],

            activePoint: null,
        };
    },

    methods: {
        handlePointClick(index) {
            if (this.activePoint !== null && this.$refs.tooltip[this.activePoint]) {
                this.$refs.tooltip[this.activePoint].style.transform = 'translate(0, 0)';
            }

            if (index !== this.activePoint) {
                if (this.$refs.tooltip[index] && this.$refs.parent) {
                    const tooltipElem = this.$refs.tooltip[index];
                    const parentRect = this.$refs.parent.getBoundingClientRect();
                    const tooltipRect = tooltipElem.getBoundingClientRect();

                    let offsetTop = 0;

                    // Если не влазит снизу
                    if (tooltipRect.top - parentRect.top + tooltipRect.height > parentRect.height) {
                        offsetTop = tooltipRect.top - parentRect.top + tooltipRect.height - parentRect.height;
                    }

                    let offsetLeft = 0;

                    // Если не влазит справа
                    if (tooltipRect.left + tooltipRect.width > parentRect.left + parentRect.width) {
                        offsetLeft = 'calc(-100% - 50px)';
                    }

                    tooltipElem.style.transform = `translate(${offsetLeft}, -${offsetTop}px)`;
                }
                this.activePoint = index;
            } else {
                this.activePoint = null;
            }
        },
    },
};
</script>

<style lang="scss" module>
    .IndexPage {
        //
    }

    .room {
        position: relative;
        overflow: hidden;
        height: 100vh;

        & > img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
    }

    .points {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }

    .point {
        position: absolute;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 50px;
        height: 50px;
        border-radius: 30px;
        background-color: gold;
        cursor: pointer;
        user-select: none;

        &._active {
            .tooltip {
                opacity: 1;
                visibility: visible;
                pointer-events: all;
            }
        }
    }

    .tooltip {
        position: absolute;
        top: 0;
        left: 100%;
        opacity: 0;
        z-index: 1;
        visibility: hidden;
        width: 250px;
        padding: 16px;
        border-radius: 4px;
        background-color: white;
        pointer-events: none;
        transition: opacity .3s ease-out;
    }
</style>

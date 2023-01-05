<template>
    <div :class="$style.IndexPage">
        <div :class="$style.room">
            <img
                ref="image"
                src="/images/bg.jpg"
                alt=""
                @load="resizePointsContainer"
            >
            <div
                :class="$style.points"
                :style="pointsStyle"
            >
                <div
                    v-for="(point, idx) in points"
                    :key="`point_${idx}`"
                    :style="{top: point.coords[0]+'%', left: point.coords[1]+'%'}"
                    :class="$style.point"
                >
                    {{ point.title }}
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
            ],

            pointsStyle: {},
        };
    },

    mounted() {
        this.$nextTick(() => {
            window.addEventListener('resize', this.resizePointsContainer);
        });
    },

    beforeDestroy() {
        window.removeEventListener('resize', this.resizePointsContainer);
    },

    methods: {
        resizePointsContainer() {
            const img = this.$refs.image;
            if (img) {
                const size = this.getObjectFitSize(false, img.width, img.height, img.naturalWidth, img.naturalHeight);
                this.pointsStyle = {
                    width: size.width + 'px',
                    height: size.height + 'px',
                    left: size.x + 'px',
                    top: size.y + 'px',
                };
            }
        },

        getObjectFitSize(contains, containerWidth, containerHeight, width, height) {
            const doRatio = width / height;
            const cRatio = containerWidth / containerHeight;
            let targetWidth = 0;
            let targetHeight = 0;
            const test = contains ? doRatio > cRatio : doRatio < cRatio;

            if (test) {
                targetWidth = containerWidth;
                targetHeight = targetWidth / doRatio;
            } else {
                targetHeight = containerHeight;
                targetWidth = targetHeight * doRatio;
            }

            return {
                width: targetWidth,
                height: targetHeight,
                x: (containerWidth - targetWidth) / 2,
                y: (containerHeight - targetHeight) / 2,
            };
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
    }
</style>

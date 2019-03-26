<template>
    <div id='BaseView'>
        <div id='topBar'>
            aaa
        </div>
        <div id='mapArea'>
            <MapView v-on:panretMessage="messageLog" ref="map"/>
        </div>
        <div id='bottomBar'>
            bbbb
        </div>
    </div>
</template>
<script>
import MapView from './MapContents.vue'

export default {
    name: 'mapview',
    data: function () {
        return {
            selectLonLat: null
        }
    },
    components: {
        MapView,
    },
    methods: {
        messageLog() {
            console.log("parent");
            console.log(this.selectLonLat)
        }
    },
    mounted() {
        this.$refs.map.initialize()
    }
}
</script>

<style scoped>
#BaseView {
    display: flex;
    flex-direction: column;
    width: 100vw;
    height: 100vh;
}

#mapArea {
    align-self: stretch;
    height: 90%;
}

#topBar {
    width: 100%;
    height: 5%;
    
}

#bottomBar {
    width: 100%;
    height: 5%;
}

</style>
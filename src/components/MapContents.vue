<template>
    <div id='OSMCanvas'>
    </div>
</template>
<script>
import 'ol/ol.css'
import {toLonLat , transform} from 'ol/proj'
import Map from 'ol/Map'
import View from 'ol/View'
import TitleLayer from 'ol/layer/Tile'
import Point from 'ol/geom/Point'
import Feature from 'ol/Feature'
import VectorSource from 'ol/source/Vector'
import VectorLayer from 'ol/layer/Vector'
import OSM from 'ol/source/OSM'

import Style from 'ol/style/Style'
import Circle from 'ol/style/Circle'
import Fill from 'ol/style/Fill'
import Stroke from 'ol/style/Stroke'
import OLText from 'ol/style/Text'
import Icon from 'ol/style/Icon'

import pinIcon from '@/assets/pin_icon.png'

export default {
    name: 'mapview',
    components: {},
    data: function () {
        return {
            mapview: null,
            markers: null,
            vectorLayer: null
        }
    },
    methods: {
        initialize () {
            this.createMap()
            this.mapview.on("click", ev => {
                var lonlat = transform(ev.coordinate, 'EPSG:3857', 'EPSG:4326')
                this.$parent.selectLonLat = lonlat
                this.addFeatureByCoord(ev.coordinate)
            })
        },
        addFeatureByLL(lonlat) {
            let coordinate = transform(lonlat , 'EPSG:4326' , 'EPSG:3857')
            addFeatureByCoordinate(coordinate)
        },
        addFeatureByCoord(coord) {
            //MapViewが生成済みの時に実行する
            if (this.mapview) {
                //すでにVectorLayerがある場合は、取り除く
                if (this.mapview != null) {
                    this.mapview.removeLayer(this.vectorLayer)
                }
                //描画したいポイントを生成
                let point = new Point(coord)
                //Featureを生成
                let feature = new Feature(point)
                let iconStyle = this.getIconStyle()
                feature.setStyle(iconStyle)
                //VectorSourceを生成
                let vectorSource = new VectorSource()
                //生成したVectorSourceに対してFeatureを追加
                vectorSource.addFeature(feature)
                //VectorSourceを描画するVectorLayerを生成
                this.vectorLayer = new VectorLayer()
                //VectorLayerにソースを追加する
                this.vectorLayer.setSource(vectorSource)
                //マップに対してレイヤーを追加する
                this.mapview.addLayer(this.vectorLayer)
            }
        },
        getIconStyle () {
            return new Style({
                image: new Icon (({
                    anchor: [0.5, 1],
                    anchorXUnits: 'fraction',
                    anchorYUnits: 'fraction',
                    src: pinIcon
                }))
            })
        },
        createMap () {
            let latlon = transform([137.6850225,38.258595], 'EPSG:4326' , 'EPSG:3857')
            this.mapview = new Map ({
                target: 'OSMCanvas',
                layers: [
                    new TitleLayer({
                        source: new OSM()
                    })
                ],
                view: new View({
                    center:latlon,
                    zoom:5
                })
            })
        }
    },
    mounted() {

    }
}

</script>

<style scoped>

#OSMCanvas {
    width: 100%;
    height: 100%;
}

</style>

<template>
  <div>
    <div ref="map" id="map"></div>
  </div>
</template>


<script>
import { map, tileLayer, marker, LatLng, Icon } from "leaflet";
import "leaflet/dist/leaflet.css";

export default {
  name: "MondialRelayMap",
  props: ["parcelShopList", "parcelSelected"],
  data() {
    return {
      lmap: null,
      lmarks: [],
    };
  },
  mounted() {
    //fix icons leaflet
    delete Icon.Default.prototype._getIconUrl;
    Icon.Default.mergeOptions({
      iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
      iconUrl: require("leaflet/dist/images/marker-icon.png"),
      shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
    });

    //load map
    this.lmap = map(this.$refs.map).setView([51.505, -0.09], 13);
    tileLayer(
      'https://{s}.tile.osm.org/{z}/{x}/{y}.png',
      {
        maxZoom: 18,
        minZoom: 12,
      }
    ).addTo(this.lmap);
  },
  watch: {
    parcelSelected: function (newParcelSelected) {
      this.lmap.setView(
        new LatLng(
          this.parseCoords(newParcelSelected.Lat),
          this.parseCoords(newParcelSelected.Long)
        ),
        18
      );
    },
    parcelShopList: function (newMarkers, oldMarkers) {
      this.lmap.panTo(
        new LatLng(
          this.parseCoords(newMarkers[0].Lat),
          this.parseCoords(newMarkers[0].Long)
        )
      );

      this.lmarks.forEach((mark) => {
        this.lmap.removeLayer(mark);
      });

      newMarkers.forEach((mark) => {
        let item = marker([
          this.parseCoords(mark.Lat),
          this.parseCoords(mark.Long),
        ])
          .addTo(this.lmap)
          .bindPopup(mark.Nom + "<br>" + mark.HoursHtmlTable, {
            minWidth: 292,
          })
          .on("click", () => {
            this.selectParcel(mark);
          });
        this.lmarks.push(item);
      });
    },
  },
  methods: {
    parseCoords(cord) {
      return cord.replace(",", ".");
    },
    selectParcel(parcel) {
      this.$emit("selectParcel", parcel);
    },
  },
};
</script>

<style>
#map {
  height: 450px;
}
.PR-Hours {
  width: 100%;
  color: #666;
  font-size: 10px;
}
.PR-Hours th {
  width: 76px;
}
.PR-Hours td {
  width: 76px;
}
.PR-Hours .d {
  background: #eee;
}
</style>

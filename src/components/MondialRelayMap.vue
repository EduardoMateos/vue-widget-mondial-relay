
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
      resizeObserver: null,
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
    tileLayer("https://{s}.tile.osm.org/{z}/{x}/{y}.png", {
      maxZoom: 18,
      minZoom: 12,
    }).addTo(this.lmap);

    this.resizeObserver = new ResizeObserver(this.onResize).observe(
      this.$refs.map
    );
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
      this.lmap.setView(
        new LatLng(
          this.parseCoords(newMarkers[0].Lat),
          this.parseCoords(newMarkers[0].Long)
        ),
        12
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
          .bindPopup(`<b>${mark.Nom}</b><br><div class="mondial-relay-map-warning">${mark.Warning}</div>${mark.HoursHtmlTable}`, {
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
    onResize() {
      this.lmap.invalidateSize();
    },
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
  width: 100%;
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
.mondial-relay-map-warning{
  color: #ffa500;
  font-weight: bold;
}
</style>

<template>
  <div id="map-container"></div>
</template>

<script>
import mapboxgl from "mapbox-gl";

export default {
  props: {
    mapData: {
      type: Array,
      required: true
    },
    accessToken: {
      type: String,
      required: true
    },
    mapStyle: {
      type: String,
      required: true
    }
  },
  methods: {
    mapInit: function() {
      mapboxgl.accessToken = this.accessToken;
      return new mapboxgl.Map({
        container: "map-container",
        style: this.mapStyle
      });
    },
    getBoundingBox: function(data) {
      let minLat = Math.min.apply(
        Math,
        data.map(function(x) {
          return x.geometry.coordinates[1];
        })
      );
      let maxLat = Math.max.apply(
        Math,
        data.map(function(x) {
          return x.geometry.coordinates[1];
        })
      );
      let minLong = Math.min.apply(
        Math,
        data.map(function(x) {
          return x.geometry.coordinates[0];
        })
      );
      let maxLong = Math.max.apply(
        Math,
        data.map(function(x) {
          return x.geometry.coordinates[0];
        })
      );
      return [
        [minLong, minLat],
        [maxLong, maxLat]
      ];
    }
  },
  mounted() {
    // Initiate the map
    let map = this.mapInit();
    let data = {
      type: "FeatureCollection",
      features: this.mapData
    };
    map.on("load", function() {
      map.addSource("property", {
        type: "geojson",
        data: data
      });

      map.addLayer({
        id: "property-points",
        type: "circle",
        source: "property",
        paint: {
          "circle-radius": 6,
          "circle-color": "#B42222"
        },
        filter: ["==", "$type", "Point"]
      });
    });
    map.addControl(new mapboxgl.NavigationControl());

    var popup = new mapboxgl.Popup({
      closeButton: false,
      closeOnClick: false
    });

    map.on("mouseenter", "property-points", function(e) {
      // Change the cursor style as a UI indicator.
      map.getCanvas().style.cursor = "pointer";

      let project = JSON.parse(e.features[0].properties.project);

      var coordinates = e.features[0].geometry.coordinates.slice();
      var description = project.Title;

      // Ensure that if the map is zoomed out such that multiple
      // copies of the feature are visible, the popup appears
      // over the copy being pointed to.
      while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
        coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
      }

      // Populate the popup and set its coordinates
      // based on the feature found.
      popup
        .setLngLat(coordinates)
        .setText(description)
        .addTo(map);
    });

    map.on("mouseleave", "property", function() {
      map.getCanvas().style.cursor = "";
      popup.remove();
    });

    // Fly and zoom to points of interest
    map.fitBounds(this.getBoundingBox(data.features), { padding: 20 });

    this.map = map;
  },
  data() {
    return {
      map: null,
      dummyData: {
        type: "FeatureCollection",
        features: []
      }
    };
  },
  watch: {
    // Watch for mapData changes and update map source
    mapData: function() {
      if (this.mapData.length == 0) {
        this.map.getSource("property").setData(this.dummyData);
      } else {
        let data = {
          type: "FeatureCollection",
          features: this.mapData
        };
        let bounds = this.getBoundingBox(data.features);
        this.map.getSource("property").setData(data);
        this.map.fitBounds(bounds, { padding: 20 });
      }
    }
  }
};
</script>

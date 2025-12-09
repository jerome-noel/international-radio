<template>
  <div id="map"></div>
  <audio ref="player"></audio>
</template>

<script>
import L from "leaflet";
import "leaflet/dist/leaflet.css";
import { capitals } from "./capitals.js";

export default {
  mounted() {
    const map = L.map("map", {
      minZoom: 2,
      maxZoom: 18
    }).setView([20, 0], 2);

    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      attribution: "&copy; OpenStreetMap contributors"
    }).addTo(map);

    const player = this.$refs.player;

    function playAudio(name) {
      player.src = `/audio/${name}.mp3`;
      player.play();
    }

    // --- Add marker for every capital city ---
    capitals.forEach(cap => {
      L.marker([cap.lat, cap.lon])
        .addTo(map)
        .bindPopup(`${cap.city}, ${cap.country}`)
        .on("click", () => {
          playAudio(cap.city.toLowerCase().replace(/[^a-z0-9]/g, "_"));
        });
    });
  }
};
</script>

<style>
#map {
  height: 100vh;
  width: 100vw;
}
</style>

const express = require("express");
const axios = require("axios");
const cors = require("cors");
const path = require("path");

const app = express();
const PORT = 3000;

app.use(cors());
app.use(express.json());
app.use(express.static(__dirname));

app.get("/api/sensors", async (req, res) => {
  try {
    const response = await axios.get("http://46.101.76.48:3000/data");
    const raw = response.data;

    const sensorData = {
      humidity: raw.humidite ?? 0,
      temperature: raw.temperature ?? 0,
      battery: raw.batterie ?? 0,
      flow: raw.flow ?? 0,
      pourcentage: raw.pourcentage ?? 0
    };

    res.json(sensorData);
  } catch (error) {
    console.error("Erreur API distante :", error.message);
    res.status(500).json({ error: "Impossible de récupérer les données" });
  }
});

app.get("/", (req, res) => {
  res.sendFile(path.join(__dirname, "interface_web.html"));
});

app.listen(PORT, () => {
  console.log(`Serveur lancé sur http://localhost:${PORT}`);
});

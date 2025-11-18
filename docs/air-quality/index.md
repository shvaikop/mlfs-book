# Air Quality Dashboard – Málaga Sensors

![Hopsworks Logo](../titanic/assets/img/logo.png)

---

## Overview

This dashboard shows:

- **Forecasts** of PM2.5 for the next days (per sensor)
- **1-day hindcast**: yesterday’s prediction vs the actual measured PM2.5

All plots are generated daily by the GitHub Actions pipeline and updated automatically.

---

## Sensor Locations

![Forecast El Atabal](./assets/img/malaga_sensors_map.png) 

Values shown on the map could be outdated.

## Forecasts & Hindcasts by Location

<table>
  <thead>
    <tr>
      <th>Location</th>
      <th>Forecast (next days)</th>
      <th>1-Day Hindcast (predictions vs outcomes)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Carranque</strong></td>
      <td>
        <img src="./assets/img/pm25_forecast_spain_malaga_carranque.png" alt="Forecast Carranque" />
      </td>
      <td>
        <img src="./assets/img/pm25_hindcast_1day_spain_malaga_carranque.png" alt="Hindcast Carranque" />
      </td>
    </tr>
    <tr>
      <td><strong>Campanillas</strong></td>
      <td>
        <img src="./assets/img/pm25_forecast_spain_malaga_campanillas.png" alt="Forecast Campanillas" />
      </td>
      <td>
        <img src="./assets/img/pm25_hindcast_1day_spain_malaga_campanillas.png" alt="Hindcast Campanillas" />
      </td>
    </tr>
    <tr>
      <td><strong>Calle Hernando de Zafra</strong></td>
      <td>
        <img src="./assets/img/pm25_forecast_spain_malaga_calle_hernando_de_zafra.png" alt="Forecast Calle Hernando de Zafra" />
      </td>
      <td>
        <img src="./assets/img/pm25_hindcast_1day_spain_malaga_calle_hernando_de_zafra.png" alt="Hindcast Calle Hernando de Zafra" />
      </td>
    </tr>
    <tr>
      <td><strong>El Atabal</strong></td>
      <td>
        <img src="./assets/img/pm25_forecast_spain_malaga_el_atabal.png" alt="Forecast El Atabal" />
      </td>
      <td>
        <img src="./assets/img/pm25_hindcast_1day_spain_malaga_el_atabal.png" alt="Hindcast El Atabal" />
      </td>
    </tr>
  </tbody>
</table>

---

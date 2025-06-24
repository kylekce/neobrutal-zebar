<script lang="ts">
  import type {
    DateOutput,
    NetworkOutput,
    WeatherOutput,
    AudioOutput
  } from "zebar";

  type RightGroupProps = {
    date: DateOutput;
    network: NetworkOutput;
    weather: WeatherOutput;
    audio: AudioOutput;
  };

  let { date, network, weather, audio }: RightGroupProps = $props();

  // Function for time-of-day determination and icon selection
  function getTimeOfDayIcon() {
    if (!date || !date.formatted) return "ti-sun"; // Default to sun

    // Parse time from formatted string
    const timeString = date.formatted;
    const isPM = timeString.toLowerCase().includes("pm");
    const isAM = timeString.toLowerCase().includes("am");

    // Get the hour
    let hour = parseInt(timeString.split(":")[0]);

    // Convert to 24-hour format
    if (isPM && hour < 12) hour += 12;
    if (isAM && hour === 12) hour = 0;

    // Determine time of day and return appropriate icon
    if (!isPM) {
      if (hour < 6) return "ti-moon-stars"; // Early morning (12 AM - 5:59 AM)
      if (hour < 9) return "ti-sunrise"; // Dawn/Early morning (6 AM - 8:59 AM)
      return "ti-sun"; // Late morning (9 AM - 11:59 AM)
    } else {
      if (hour < 17) return "ti-sun"; // Afternoon (12 PM - 4:59 PM)
      if (hour < 19) return "ti-sunset"; // Evening/dusk (5 PM - 6:59 PM)
      return "ti-moon"; // Night (7 PM - 11:59 PM)
    }
  }
</script>

<div class="flex items-center gap-3">
  {#if audio?.defaultPlaybackDevice?.name}
    <div class="flex items-center gap-1">
      {#if audio.defaultPlaybackDevice.isMuted}
        <i class="ti ti-volume-3"></i>
      {:else if audio.defaultPlaybackDevice.volume >= 50}
        <i class="ti ti-volume"></i>
      {:else if audio.defaultPlaybackDevice.volume >= 1}
        <i class="ti ti-volume-2"></i>
      {:else}
        <i class="ti ti-volume-3"></i>
      {/if}
      <div class="overflow-hidden max-w-20 group">
        <div class="inline-block whitespace-nowrap group-hover:animate-marquee">
          <span>
            {audio.defaultPlaybackDevice.name}
          </span>
        </div>
      </div>
    </div>
  {:else}
    <i class="ti ti-volume-off"></i>
  {/if}
  <div class="flex items-center gap-1">
    {#if network?.defaultInterface?.type === "ethernet"}
      <i class="ti ti-network"></i>
    {:else if network?.defaultInterface?.type === "wifi"}
      {#if network.defaultGateway!.signalStrength! >= 75}
        <i class="ti ti-wifi"></i>
      {:else if network.defaultGateway!.signalStrength! >= 50}
        <i class="ti ti-wifi-2"></i>
      {:else if network.defaultGateway!.signalStrength! >= 25}
        <i class="ti ti-wifi-1"></i>
      {:else}
        <i class="ti ti-wifi-off"></i>
      {/if}
      {#if network?.defaultGateway?.ssid}
        <div class="overflow-hidden max-w-20 group">
          <div
            class="inline-block whitespace-nowrap group-hover:animate-marquee"
          >
            <span>
              {network.defaultGateway.ssid}
            </span>
          </div>
        </div>
      {/if}
    {:else}
      <i class="ti ti-wifi-off"></i>
    {/if}
  </div>
  {#if weather}
    <div class="flex items-center gap-1">
      {#if weather.status === "clear_day"}
        <i class="nf nf-weather-day_sunny"></i>
      {:else if weather.status === "clear_night"}
        <i class="nf nf-weather-night_clear"></i>
      {:else if weather.status === "cloudy_day"}
        <i class="nf nf-weather-day_cloudy"></i>
      {:else if weather.status === "cloudy_night"}
        <i class="nf nf-weather-night_alt_cloudy"></i>
      {:else if weather.status === "light_rain_day"}
        <i class="nf nf-weather-day_sprinkle"></i>
      {:else if weather.status === "light_rain_night"}
        <i class="nf nf-weather-night_alt_sprinkle"></i>
      {:else if weather.status === "heavy_rain_day"}
        <i class="nf nf-weather-day_rain"></i>
      {:else if weather.status === "heavy_rain_night"}
        <i class="nf nf-weather-night_alt_rain"></i>
      {:else if weather.status === "snow_day"}
        <i class="nf nf-weather-day_snow"></i>
      {:else if weather.status === "snow_night"}
        <i class="nf nf-weather-night_alt_snow"></i>
      {:else if weather.status === "thunder_day"}
        <i class="nf nf-weather-day_lightning"></i>
      {:else if weather.status === "thunder_night"}
        <i class="nf nf-weather-night_alt_lightning"></i>
      {/if}
      {Math.round(weather.celsiusTemp)}°
    </div>
  {/if}
  <div class="flex items-center gap-1">
    <i class="ti {getTimeOfDayIcon()}"></i>
    {date?.formatted}
  </div>
</div>

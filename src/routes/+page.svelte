<script lang="ts">
  import { onMount } from "svelte";
  import * as zebar from "zebar";
  import type {
    BatteryOutput,
    CpuOutput,
    GlazeWmOutput,
    MemoryOutput,
    DateOutput,
    NetworkOutput,
    WeatherOutput,
    MediaOutput,
    AudioOutput
  } from "zebar";

  import "../app.css";
  import Group from "../components/Group.svelte";
  import LeftGroup from "../components/LeftGroup.svelte";
  import RightGroup from "../components/RightGroup.svelte";
  import Workspaces from "../components/Workspaces.svelte";
  import NowPlaying from "../components/NowPlaying.svelte";

  let battery = $state<BatteryOutput | null>();
  let cpu = $state<CpuOutput | null>();
  let date = $state<DateOutput | null>();
  let glazewm = $state<GlazeWmOutput | null>();
  let memory = $state<MemoryOutput | null>();
  let network = $state<NetworkOutput | null>();
  let weather = $state<WeatherOutput | null>();
  let media = $state<MediaOutput | null>();
  let audio = $state<AudioOutput | null>();

  onMount(() => {
    const providers = zebar.createProviderGroup({
      battery: { type: "battery" },
      cpu: { type: "cpu" },
      date: { type: "date", formatting: "hh:mm a, MMMM d, EEE" },
      glazewm: { type: "glazewm" },
      memory: { type: "memory" },
      network: { type: "network" },
      weather: { type: "weather" },
      media: { type: "media" },
      audio: { type: "audio" }
    });

    providers.onOutput(() => {
      battery = providers.outputMap.battery;
      cpu = providers.outputMap.cpu;
      date = providers.outputMap.date;
      glazewm = providers.outputMap.glazewm;
      memory = providers.outputMap.memory;
      network = providers.outputMap.network;
      weather = providers.outputMap.weather;
      media = providers.outputMap.media;
      audio = providers.outputMap.audio;
    });
  });
</script>

<div
  class="flex items-center justify-between h-bar my-zby mx-zbx text-zb-text text-zb-size font-base"
>
  <Group class="shrink-0">
    <LeftGroup
      battery={battery!}
      cpu={cpu!}
      memory={memory!}
      glazewm={glazewm!}
      audio={audio!}
    />
  </Group>
  <Group class="shrink-0">
    <Workspaces glazewm={glazewm!} />
  </Group>
  <div class="flex items-center justify-between h-full gap-1">
    <NowPlaying glazewm={glazewm!} media={media!} />
    <Group class="shrink-0">
      <RightGroup
        date={date!}
        network={network!}
        weather={weather!}
        audio={audio!}
      />
    </Group>
  </div>
</div>

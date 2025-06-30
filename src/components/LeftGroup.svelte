<script lang="ts">
  import type {
    BatteryOutput,
    CpuOutput,
    MemoryOutput,
    GlazeWmOutput,
    AudioOutput
  } from "zebar";

  import Button from "./Button.svelte";
  import Meter from "./Meter.svelte";

  type LeftGroupProps = {
    battery: BatteryOutput;
    cpu: CpuOutput;
    memory: MemoryOutput;
    glazewm: GlazeWmOutput;
    audio: AudioOutput;
  };

  let { battery, cpu, memory, glazewm, audio }: LeftGroupProps = $props();

  function getVolumeClass(volume: number): string {
    if (volume === 0) return "bg-zb-volume-muted";
    if (volume <= 33) return "bg-zb-volume-low";
    if (volume <= 66) return "bg-zb-volume-medium";
    return "bg-zb-volume-high";
  }

  function getBatteryClass(charge: number): string {
    if (charge <= 20) return "bg-zb-battery-low";
    if (charge <= 50) return "bg-zb-battery-mid";
    return "bg-zb-battery-good";
  }

  function getCpuClass(usage: number): string {
    if (usage >= 80) return "bg-zb-cpu-high-usage";
    return "bg-zb-cpu";
  }

  function getMemoryClass(usage: number): string {
    if (usage <= 50) return "bg-zb-memory-low";
    if (usage <= 75) return "bg-zb-memory-medium";
    return "bg-zb-memory-high";
  }
</script>

<div class="flex flex-row items-center gap-3">
  <Button
    class="text-zb-power"
    iconClass="power"
    callback={() => glazewm!.runCommand("shell-exec shutdown /s /t 0")}
  />
  <Button
    class="text-zb-restart"
    iconClass="refresh"
    callback={() => glazewm!.runCommand("shell-exec shutdown /r /t 0")}
  />
  <div class="flex items-center gap-1">
    <i class="ti ti-ruler-2"></i>
    <Meter
      class={getMemoryClass(Math.round(memory?.usage ?? 0))}
      percent={Math.round(memory?.usage ?? 0)}
    />
  </div>
  <div class="flex items-center gap-1">
    <i class="ti ti-cpu"></i>
    <Meter
      class={getCpuClass(Math.round(cpu?.usage ?? 0))}
      percent={Math.round(cpu?.usage ?? 0)}
    />
  </div>
  <div class="flex items-center gap-1">
    <i class="ti ti-bolt"></i>
    <Meter
      class={getBatteryClass(Math.round(battery?.chargePercent ?? 100))}
      percent={Math.round(battery?.chargePercent ?? 100)}
    />
  </div>
  <div class="flex items-center gap-1">
    <i class="ti ti-volume"></i>
    <Meter
      class={getVolumeClass(
        Math.round(audio?.defaultPlaybackDevice?.volume ?? 0)
      )}
      percent={Math.round(audio?.defaultPlaybackDevice?.volume ?? 0)}
    />
  </div>
</div>

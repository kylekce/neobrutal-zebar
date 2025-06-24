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
</script>

<div class="flex flex-row items-center gap-3">
  <Button
    class="text-zb-icon"
    iconClass="power"
    callback={() => glazewm!.runCommand("shell-exec shutdown /s /t 0")}
  />
  <Button
    class="text-yellow-100"
    iconClass="refresh"
    callback={() => glazewm!.runCommand("shell-exec shutdown /r /t 0")}
  />
  <div class="flex items-center gap-1">
    <i class="ti ti-ruler-2"></i>
    <Meter class="bg-zb-memory" percent={Math.round(memory?.usage ?? 0)} />
  </div>
  <div class="flex items-center gap-1">
    <i class="ti ti-cpu"></i>
    <Meter class="bg-zb-cpu" percent={Math.round(cpu?.usage ?? 0)} />
  </div>
  <div class="flex items-center gap-1">
    <i class="ti ti-bolt"></i>
    <Meter
      class="bg-zb-battery-good"
      percent={Math.round(battery?.chargePercent ?? 100)}
    />
  </div>
  <div class="flex items-center gap-1">
    <i class="ti ti-volume"></i>
    <Meter
      class="bg-lime-100"
      percent={Math.round(audio?.defaultPlaybackDevice?.volume ?? 0)}
    />
  </div>
</div>

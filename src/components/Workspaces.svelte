<script lang="ts">
  import type { Window } from "glazewm";
  import type { GlazeWmOutput } from "zebar";

  import iconMap from "$lib/icon_map.json";
  import ignoredApps from "$lib/ignored_apps.json";

  import Button from "./Button.svelte";

  const getProcessIcon = (child: Window) => {
    const possibleAppNames = [
      child.title.toLowerCase(),
      child.processName.toLowerCase()
    ];

    if (ignoredApps.find((app) => possibleAppNames.includes(app.name))) return;

    let entry = iconMap.find((entry) =>
      entry.appNames
        .map((name) => name.toLowerCase())
        .some((name) => possibleAppNames.includes(name))
    );

    return entry?.iconName ?? "ti-background";
  };

  let { glazewm }: { glazewm: GlazeWmOutput } = $props();
</script>

{#if glazewm}
  <div class="flex flex-row items-center gap-2">
    {#each glazewm.currentWorkspaces as workspace, i}
      <Button
        iconClass="ti {workspace.hasFocus ? 'ti-point-filled' : 'ti-point'}"
        class="text-zb-ws-{i}"
        callback={() =>
          glazewm!.runCommand(`focus --workspace ${workspace.name}`)}
      />
    {/each}
    <Button
      iconClass="ti ti-plus"
      class="text-zb-add-workspace"
      noBg={true}
      callback={() =>
        glazewm!.runCommand(
          `focus --workspace ${glazewm.currentWorkspaces.length + 1}`
        )}
    ></Button>
    <Button
      iconClass="ti ti-switch-{glazewm?.tilingDirection}"
      class="flex items-center justify-center text-zb-tiling-direction"
      noBg={true}
      callback={() => glazewm!.runCommand("toggle-tiling-direction")}
    ></Button>
    {#each glazewm.bindingModes as bindingMode, i}
      <div class="flex items-center">
        <button
          class="pb-[4px]"
          onclick={() => {
            switch (bindingMode.name.toLowerCase()) {
              case "pause":
                glazewm!.runCommand("wm-disable-binding-mode --name pause");
                break;

              case "resize":
                glazewm!.runCommand("wm-disable-binding-mode --name resize");
                break;

              default:
                break;
            }
          }}
        >
          {bindingMode.displayName ?? bindingMode.name}
        </button>
      </div>
    {/each}
    <div class="flex items-center gap-1">
      {#if glazewm.focusedWorkspace}
        {#each glazewm.focusedWorkspace!.children as child}
          {#if child.type == "window" && child.state.type != "minimized"}
            {@const icon = getProcessIcon(child as Window)}
            {#if icon}
              <Button
                iconClass={`ti ${icon}`}
                class={`${
                  child.hasFocus ? "text-zb-focused-process" : "text-zb-process"
                }`}
                noBg={true}
                callback={async () => {
                  if (child.hasFocus) glazewm!.runCommand("toggle-minimized");
                }}
              ></Button>
            {/if}
          {/if}
        {/each}
      {/if}
    </div>
  </div>
{/if}

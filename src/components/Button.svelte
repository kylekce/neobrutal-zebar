<script lang="ts">
  type ButtonProps = {
    iconClass: string;
    label?: string;
    callback?: () => void;
    class: string;
    noBg?: boolean;
  };
  let {
    iconClass,
    label,
    callback,
    noBg = false,
    ...rest
  }: ButtonProps = $props();
</script>

<button
  class="flex {noBg
    ? 'hover:scale-150 transition-transform duration-150'
    : 'bg-blend-30 border border-zb-border rounded-base shadow-button hover:translate-x-boxShadowX hover:translate-y-boxShadowY hover:shadow-none'} p-1 {rest.class}"
  onclick={callback}
  aria-label={label}
>
  <i class="ti ti-{iconClass}"></i>
</button>

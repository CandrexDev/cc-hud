<script lang="ts">
  import Button from '../atoms/button.svelte';
  import { saveUIDataToServer } from '../../utils/eventHandler';
  import PlayerHudStore from '../../stores/playerStatusHudStore';
  import ColorEffectStore from '../../stores/colorEffectStore';
  import LayoutStore from '../../stores/layoutStore';
  import MenuStore from '../../stores/menuStore';
  import Switch from '../atoms/switch.svelte';
  import ProfilePanel from './profilePanel.svelte';
  import UtilityFunctionPanel from './utilityFunctionPanel.svelte';
  import GlobalLayoutPanel from './globalLayoutPanel.svelte';
  import SingleStatusIconPanel from './singleStatusIconPanel.svelte';
  import GlobalStatusIconPanel from './globalStatusIconPanel.svelte';

  let group: string = '';

</script>

<div class="text-sm flex flex-col text-[#e8e8e8] select-none">
  <div class="my-3 text-2xl text-white flex flex-row">
    <div class="flex-1 flex flex-col justify-center min-w-min">
      <div>
        <div class="flex flex-row items-center">
          <p class="ml-3 p-0">Status Icons Settings</p>
        </div>
      </div>
    </div>
    <div class="text-base">
      <p>Design Mode</p>
      <Switch center bind:checked={$PlayerHudStore.designMode}/>
    </div>
    <div class="flex flex-1 min-w-min justify-end">
      <Button name="Reset Status Icon Settings" buttonClass="mr-5 hover:bg-red-600"
        on:click={() => {
          PlayerHudStore.resetPlayerStatusIcons();
          ColorEffectStore.resetColorEffects();
          LayoutStore.resetLayout();
        }}/>
      {#if $MenuStore.adminOnly && $MenuStore.isAdmin}
        <Button name="Save Changes To Server" buttonClass={"my-auto"} disable={$PlayerHudStore.saveUIState == "ready" ? false : true}
          on:click={() => { saveUIDataToServer(); $PlayerHudStore.saveUIState = "updating" }}
        />
      {/if}
    </div>
  </div>
  
  <hr>
  <GlobalStatusIconPanel bind:group/>
  <SingleStatusIconPanel bind:group/>
  <GlobalLayoutPanel bind:group/>
  <UtilityFunctionPanel bind:group/>
  <ProfilePanel bind:group/>
</div>

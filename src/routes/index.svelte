<script lang="ts">
  import PocketBase from 'pocketbase'
  const client = new PocketBase('http://127.0.0.1:8090');
  const resultList = client.records.getList('orders');

  import Button, { Label, Icon } from '@smui/button';
  import CircularProgress from '@smui/circular-progress';
  import LinearProgress from '@smui/linear-progress';

  import Card, {
    Content,
    PrimaryAction,
    Actions,
    ActionButtons,
    ActionIcons,
  } from '@smui/card';

  let clicked = 0;

	function handleClick(event: CustomEvent | MouseEvent) {
		event = event as MouseEvent;
		if (event.button === 0) {
			clicked++;
		}
	}

	function reset() {
		clicked = 0;
	}
</script>

<Button on:mousedown={handleClick}>
	<Icon class="material-icons">thumb_up</Icon>
	<Label>Click Me</Label>
</Button>

<p class="mdc-typography--body1">
	{#if clicked}
		You've clicked the button {clicked} time{clicked === 1 ? '' : 's'}. You can
		<a on:click={reset} href="javascript:void(0);">reset it</a>.
	{:else}
		<span class="grayed">You haven't clicked the button.</span>
	{/if}
</p>

{#await resultList}
  <p>...Loading</p>
{:then value}
  <div class="card-container">
    {#each value.items as item}
      <Card>
        <Content>{item.id}</Content>
        <Content>{item.total_price}</Content>
        <Actions fullBleed>
          <Button on:click={() => clicked++}>
            <Label>Action</Label>
            <i class="material-icons" aria-hidden="true">arrow_forward</i>
          </Button>
        </Actions>
      </Card>
    {/each}
  </div>
{:catch error}
  <p>{Error}</p>

{/await}





<style>
	.grayed {
		opacity: 0.6;
	}
</style>

<script lang="ts">
  import { InlineCalendar, Datepicker } from 'svelte-calendar';
  import PrimaryButton from '$lib/components/PrimaryButton/index.svelte';
  import { isMobile } from '$lib/utils/store/useMobile';
  import { formatDate } from '$lib/utils/functions/routes/dashboard';
  import { VARIANTS } from '$lib/components/PrimaryButton/constants';
  import { t } from '$lib/utils/functions/translations';

  let store;

  const theme = {
    calendar: {
      width: '250px',
      height: '200px',
      shadow: '0px 0px 5px rgba(0, 0, 0, 0.25)',
      colors: {
        background: {
          highlight: '#0F62FE'
        }
      },
      font: {
        regular: '0.7em',
        large: '0.9em'
      }
    }
  };
</script>

<section class="w-full h-full mt-2">
  <p class="text-base font-semibold text-[#040F2D] pb-3 dark:text-white">{$t('schedule.title')}</p>

  <div
    class="flex items-center flex-col border border-[#EAEAEA] dark:bg-neutral-800 rounded w-full h-fit lg:h-[40vh] p-3 overflow-y-auto"
  >
    <div class="w-full h-fit hidden xl:flex items-center justify-center">
      <InlineCalendar bind:store {theme} />
    </div>
    {#if $isMobile}
      <div class="header w-full flex items-center">
        <Datepicker bind:store let:key let:send let:receive>
          <button
            class="text-lg font-bold text-primary-700 p-3 hover:bg-gray-300 rounded-md"
            in:receive|local={{ key }}
            out:send|local={{ key }}
          >
            {formatDate($store?.selected)}
          </button>
        </Datepicker>
      </div>
    {:else}
      <p class="xl:hidden dark:text-white font-bold m-5">
        {formatDate($store?.selected)}
      </p>
    {/if}
    {#if formatDate($store?.selected) == $t('dashboard.today')}
      <span class="flex flex-col md:flex-row items-baseline justify-center gap-10 mt-5">
        <p class="text-xs font-normal text-[#656565] dark:text-white">2:30pm</p>
        <div class="flex flex-col items-start gap-4">
          <span>
            <p class="text-lg font-semibold text-[#262626] dark:text-white">
              {$t('schedule.essence')}
            </p>
            <a href="#" class="text-sm font-normal text-primary-700 dark:text-white"
              >{$t('schedule.figma')}</a
            >
          </span>
          <PrimaryButton label={$t('schedule.join')} variant={VARIANTS.OUTLINED} className="" />
        </div>
      </span>
    {:else}
      <p>{$t('schedule.no')}</p>
    {/if}
  </div>
</section>

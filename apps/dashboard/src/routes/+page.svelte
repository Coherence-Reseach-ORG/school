<script>
  import FlowStreamReferenceIcon from 'carbon-icons-svelte/lib/FlowStreamReference.svelte';
  import ChartClusterBarIcon from 'carbon-icons-svelte/lib/ChartClusterBar.svelte';
  import MachineLearningModelIcon from 'carbon-icons-svelte/lib/MachineLearningModel.svelte';
  import TextField from '$lib/components/Form/TextField.svelte';
  import PrimaryButton from '$lib/components/PrimaryButton/index.svelte';
  import { getSupabase } from '$lib/utils/functions/supabase';
  import { validateEmail } from '$lib/utils/functions/validateEmail';
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  let email = '';
  let isAdding = false;
  let success = false;

  const supabase = getSupabase();
  const animate = 'transition delay-75 duration-300 ease-in-out';
  const areas = [
    {
      title: 'Access',
      description:
        'Quickly find any material you need to get your job done. ClassroomIO offers the All-in-on-platform'
    },
    {
      title: 'Analytics',
      description:
        'Avoid assumptions, better understand the needs of your classrooms and give students a personlized experience'
    },
    {
      title: 'Automation',
      description:
        'Lesson reminders, deadline reminders and many other automated alerts to help everyone in the learning process be proactive'
    }
  ];

  onMount(() => {
    // goto('/login');
  });

  async function handleSubmit() {
    if (!email || !validateEmail(email)) return;
    isAdding = true;

    await supabase.from('waitinglist').insert([{ email }]);

    success = true;

    setTimeout(() => {
      isAdding = false;
      success = false;
      email = '';
    }, 5000);
  }
</script>

<svelte:head>
  <title>Coherence Research School</title>
</svelte:head>

<div>
  <iframe
    src="https://www.coherence-research.com"
    class="h-[100dvh] w-[100dvw]"
    title="Coherence Research"
    frameborder="0"
  ></iframe>
</div>

<div
  class="m-2 flex hidden w-screen flex-col items-center justify-center font-sans sm:m-0 md:h-[93vh]"
>
  <!--
    <img
      src="/logo-192.png"
      alt="ClassroomIO logo"
      class="rounded inline-block mx-auto w-20 h-20 sm:w-auto sm:h-auto"
      data-atf="1"
    />
  -->
  <div>
    <h3 class="text-center text-4xl dark:text-white">
      Classroom<span class="text-primary-700">IO</span>
    </h3>
    <p class="text-center text-lg dark:text-white">
      The operating system for classroooms of the future 🚀🚀🚀.
    </p>
  </div>

  <form on:submit|preventDefault={handleSubmit} class="my-4 hidden">
    <div class="flex flex-col items-center sm:flex-row">
      {#if success}
        <p class="dark:text-white">You have been added successfully. Thanks for joining.</p>
      {:else}
        <TextField
          bind:value={email}
          type="email"
          placeholder="Enter your email.."
          className="mr-3 my-2"
          isRequired={true}
          isDisabled={isAdding}
        />
        <PrimaryButton type="submit" isLoading={isAdding}>Join waiting list</PrimaryButton>
      {/if}
    </div>
  </form>

  <div class="mt-4 flex flex-col md:flex-row">
    {#each areas as area, index}
      <div
        class="active hover:border-primary-700 m-3 max-w-[350px] rounded-md border-2 bg-white px-12 py-3 shadow-xl dark:bg-black {animate}"
      >
        <h3 class="text-3xl dark:text-white">
          {#if index === 0}
            <FlowStreamReferenceIcon size={32} class="carbon-icon dark:text-white" />
          {:else if index === 1}
            <ChartClusterBarIcon size={32} class="carbon-icon dark:text-white" />
          {:else if index === 2}
            <MachineLearningModelIcon size={32} class="carbon-icon dark:text-white" />
          {/if}
          {area.title}
        </h3>
        <p class="dark:text-white">{area.description}</p>
      </div>
    {/each}
  </div>
</div>

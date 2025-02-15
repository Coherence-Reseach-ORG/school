<script lang="ts">
  import { fade } from 'svelte/transition';
  import get from 'lodash/get';
  import { onMount } from 'svelte';
  import LogoFacebook from 'carbon-icons-svelte/lib/LogoFacebook.svelte';
  import LogoTwitter from 'carbon-icons-svelte/lib/LogoTwitter.svelte';
  import LogoLinkedin from 'carbon-icons-svelte/lib/LogoLinkedin.svelte';
  import { Accordion, AccordionItem, Grid, Row, Column } from 'carbon-components-svelte';
  import LocationFilled from 'carbon-icons-svelte/lib/LocationFilled.svelte';
  import Phone from 'carbon-icons-svelte/lib/Phone.svelte';
  import Email from 'carbon-icons-svelte/lib/Email.svelte';
  import Rocket from 'carbon-icons-svelte/lib/Rocket.svelte';
  import PageLoader from './PageLoader.svelte';
  import { orgLandingpageValidation } from '$lib/utils/functions/validator';
  import TextField from '$lib/components/Form/TextField.svelte';
  import PrimaryButton from '$lib/components/PrimaryButton/index.svelte';
  import { VARIANTS } from '$lib/components/PrimaryButton/constants';
  import Navigation from '$lib/components/Navigation/index.svelte';
  import Box from '$lib/components/Box/index.svelte';
  import Card from '$lib/components/Courses/components/Card/index.svelte';
  import CardLoader from '$lib/components/Courses/components/Card/Loader.svelte';
  import CoursesEmptyIcon from '$lib/components/Icons/CoursesEmptyIcon.svelte';
  import { getSupabase } from '$lib/utils/functions/supabase';
  import { courses, courseMetaDeta } from '$lib/components/Courses/store';
  import { getCourseBySiteName } from '$lib/utils/services/org';
  import { currentOrg } from '$lib/utils/store/org';
  import { validateEmail } from '$lib/utils/functions/validateEmail';
  import TextArea from '$lib/components/Form/TextArea.svelte';
  import { goto } from '$app/navigation';
  import { landingPageSettings } from '$lib/components/Org/Settings/store';

  export let orgSiteName = '';
  export let org = {};

  let email: string | undefined;
  let isAdding = false;
  let success = false;
  let successContactSaved = false;
  let viewAll = false;
  let isContactSubmiting = false;
  let player;
  let contact = {
    name: '',
    email: '',
    phone: '',
    message: ''
  };
  let contactError = {};

  const supabase = getSupabase();

  async function handleSubmit() {
    if (!email || !validateEmail(email)) return;
    isAdding = true;

    const { error } = await supabase.from('organization_emaillist').insert({
      email,
      organization_id: org.id
    });

    if (error) {
      console.error('Error', error);
    }

    success = true;
    setTimeout(() => {
      isAdding = false;
      success = false;
      email = undefined;
    }, 5000);
  }

  async function handleContactSubmit() {
    isContactSubmiting = true;
    contactError = orgLandingpageValidation(contact);
    if (Object.keys(contactError).length) {
      isContactSubmiting = false;
      return;
    }

    // Save to db
    const { error } = await supabase.from('organization_contacts').insert({
      name: contact.name,
      email: contact.email,
      phone: contact.phone,
      message: contact.message,
      organization_id: org.id
    });

    if (error) {
      console.error('Something went wrong', error, '\n\nContact', contact);
      isContactSubmiting = false;
    }

    isContactSubmiting = false;
    successContactSaved = true;
    contact = {
      name: '',
      email: '',
      phone: '',
      message: ''
    };
  }
  function isYouTubeLink(link: string) {
    const youtubeRegex =
      /^(https?:\/\/)?(www\.)?(youtube\.com\/watch\?v=|youtu\.be\/)([a-zA-Z0-9_-]{11})/;

    return youtubeRegex.test(link.trim());
  }

  onMount(async () => {
    if (!orgSiteName) return;
    try {
      console.log('sitename', orgSiteName);
      $courseMetaDeta.isLoading = true;
      const coursesResult = await getCourseBySiteName(orgSiteName);
      courses.set(coursesResult);
      $courseMetaDeta.isLoading = false;
    } catch (error) {
      console.log('error', error);
    }
  });
  function initPlyr(_player: any, _video: string | undefined) {
    if (!player) return;

    // @ts-ignore
    const plyr = new Plyr('#player', { width: '400px', height: '300px', borderRadius: '10px' });
    // @ts-ignore
    window.player = plyr;
  }

  // Use default data in store if user hasn't added their own content to landing page
  function setDefault(landingpage) {
    if (landingpage && Object.keys(landingpage).length) {
      if (!landingpage?.header?.banner) {
        landingpage.header.banner = $landingPageSettings.header.banner;
      }

      $landingPageSettings = {
        ...landingpage
      };
    }
  }

  $: initPlyr(player, $landingPageSettings.header?.banner?.video);
  $: setDefault(org.landingpage);
</script>

<svelte:head>
  <title>
    {!org.name ? '' : `${org.name}'s `}Landing Page
  </title>
</svelte:head>

{#if !org.landingpage}
  <PageLoader />
{:else}
  <main>
    <!-- Header Section -->
    {#if $landingPageSettings.header.show}
      <header id="header" class="banner relative mb-10 h-[100vh] w-full md:h-[90vh]">
        <Navigation logo={org.avatar_url} orgName={org.name} disableSignup={true} />
        <div class="absolute top-0 z-10 h-[100vh] w-full bg-white opacity-80 md:h-[90vh]" />
        {#if $landingPageSettings.header.banner.show}
          <div class="flex items-center justify-center py-2 md:h-full">
            <div
              class="relative z-20 flex w-full flex-col-reverse items-center justify-center md:w-11/12 md:flex-row md:justify-between lg:w-5/6"
            >
              <!-- Course Description -->
              <div class="mr-3 flex w-11/12 flex-col items-center py-10 md:w-2/5 md:items-start">
                <p class=" text-primary-600 text-2xl font-semibold capitalize">
                  {org.name}
                </p>
                <h1
                  class="my-4 text-center text-4xl font-bold md:text-start md:text-5xl lg:text-6xl"
                >
                  {$landingPageSettings.header.title} <br /><span class="text-primary-600"
                    >{$landingPageSettings.header.titleHighlight}</span
                  >
                </h1>
                <p class="text-md mb-3 text-center text-xl md:mb-5 md:text-start">
                  {$landingPageSettings.header.subtitle}
                </p>

                <PrimaryButton
                  label={$landingPageSettings.header.action.label}
                  className="mt-2 md:mt-5 px-10 w-fit"
                  onClick={() => {
                    $landingPageSettings.header.action.redirect &&
                      goto($landingPageSettings.header.action.link);
                  }}
                />
              </div>

              <div class="flex w-5/6 md:w-1/2 md:max-w-[650px]">
                {#if isYouTubeLink($landingPageSettings.header?.banner?.video) && $landingPageSettings.header.banner.type === 'video'}
                  <div bind:this={player} id="player" style="width:100%; border-radius:12px">
                    <iframe
                      title="Header video"
                      src={$landingPageSettings.header?.banner?.video}
                      allowfullscreen
                      allowtransparency
                      allow="autoplay"
                    />
                  </div>
                {:else}
                  <!-- <video class="w-full rounded-xl" controls loop autoplay>
                      <source src={$landingPageSettings.header?.banner?.video} type="video/mp4" />
                      <source src="/path/to/video.webm" type="video/webm" />
                      Captions are optional
                      <track kind="captions" />
                    </video> -->
                  <img
                    class="rounded-md"
                    src={$landingPageSettings.header?.banner?.image}
                    alt="landing page banner"
                  />
                {/if}
              </div>
            </div>
          </div>
        {:else}
          <div class="relative z-20 flex h-full w-full items-center justify-center md:flex-row">
            <div class="mx-auto flex w-11/12 max-w-[600px] flex-col items-center py-10">
              <p class=" text-primary-600 text-2xl font-semibold capitalize">{org.name}</p>
              <h1 class="my-4 text-center text-4xl font-bold md:text-5xl lg:text-6xl">
                {$landingPageSettings.header.title} <br /><span class="text-primary-600"
                  >{$landingPageSettings.header.titleHighlight}</span
                >
              </h1>
              <p class="text-md mb-6 text-center text-xl">
                {$landingPageSettings.header.subtitle}
              </p>

              <PrimaryButton
                label={$landingPageSettings.header.action.label}
                className="mt-5 px-10 w-fit"
                onClick={() => {
                  $landingPageSettings.header.action.redirect &&
                    goto($landingPageSettings.header.action.link);
                }}
              />
            </div>
          </div>
        {/if}
      </header>
    {/if}

    <!-- Our Story section -->
    {#if $landingPageSettings.aboutUs.show}
      <section id="aboutus" class="m-h-[400px] mx-auto my-10 w-full max-w-6xl">
        <div class="mx-4 flex flex-col items-center justify-evenly lg:flex-row">
          <div class="mb-5 mr-5 max-w-[600px] lg:mb-0 lg:w-2/5">
            <h2 class="text-4xl md:text-5xl lg:text-6xl">{$landingPageSettings.aboutUs.title}</h2>
            <p class="mb-2">
              {$landingPageSettings.aboutUs.content}
            </p>
          </div>

          <div class="image">
            <img
              src={$landingPageSettings.aboutUs.imageUrl}
              alt="Our Story"
              class=" max-h-[450px] rounded-2xl"
            />
          </div>
        </div>
      </section>
    {/if}

    <!-- Courses Section -->
    {#if $landingPageSettings.courses.show}
      <section id="courses" transition:fade class="mx-auto my-10 w-full max-w-6xl">
        <div class="w-full">
          <div class="mx-auto w-11/12 max-w-[500px] py-10">
            <h1 class="my-4 text-center text-4xl font-bold md:text-5xl lg:text-6xl">
              {$landingPageSettings.courses.title}
              <span class="text-primary-600">{$landingPageSettings.courses.titleHighlight}</span>
            </h1>
            <p class="text-md text-center">
              {$landingPageSettings.courses.subtitle}
            </p>
          </div>
        </div>
        {#if $courseMetaDeta.isLoading}
          <div class="cards-container mx-2 my-4">
            <CardLoader />
            <CardLoader />
            <CardLoader />
          </div>
        {:else if $courses.length > 0}
          <div class="cards-container mx-2 my-4">
            {#each $courses.slice(0, viewAll ? $courses.length : 3) as courseData}
              <Card
                id={courseData.id}
                slug={courseData.slug}
                bannerImage={courseData.logo || '/images/classroomio-course-img-template.jpg'}
                title={courseData.title}
                description={courseData.description}
                role_id={courseData.role_id}
                isPublished={courseData.is_published}
                cost={courseData.cost}
                currency={courseData.currency}
                totalLessons={get(courseData, 'lessons[0].count', 0)}
                isOnLandingPage={true}
              />
            {/each}
          </div>
        {:else}
          <Box>
            <CoursesEmptyIcon />
            <h3 class="my-5 text-2xl dark:text-white">No Course Published</h3>
            <p class="w-1/3 text-center dark:text-white">
              We've got great courses coming your way, stay tuned!!!
            </p>
          </Box>
        {/if}

        {#if $courses.length > 3}
          <div class="mt-3 flex w-full justify-center">
            <PrimaryButton
              variant={VARIANTS.OUTLINED}
              onClick={() => (viewAll = !viewAll)}
              label={viewAll ? 'View Less' : 'View All'}
              className="px-10 py-5 w-fit"
            />
          </div>
        {/if}
      </section>
    {/if}

    <!-- FAQ Section -->
    {#if $landingPageSettings.faq.show}
      <section id="faq" transition:fade class="mx-auto my-10 w-full max-w-[700px]">
        <div class="py-10">
          <h1 class="my-4 text-center text-4xl font-bold md:text-5xl lg:text-6xl">
            {$landingPageSettings.faq.title}
          </h1>
        </div>
        <div class="mx-2">
          <Accordion size="xl">
            {#each $landingPageSettings.faq.questions as faq}
              <AccordionItem title={faq.title} class="text-3xl">
                <p class="text-lg">
                  {faq.content}
                </p>
              </AccordionItem>
            {/each}
          </Accordion>
        </div>
      </section>
    {/if}

    <!-- Contact Section-->
    {#if $landingPageSettings.contact.show}
      <section id="contact" transition:fade class="bg-primary-50 my-10 w-full">
        <div class="mx-auto w-full max-w-6xl">
          <div class="mx-auto w-11/12 max-w-[500px] py-10">
            <h1 class="my-4 text-center text-4xl font-bold md:text-5xl lg:text-6xl">
              {$landingPageSettings.contact.title}
              <span class="text-primary-600">{$landingPageSettings.contact.titleHighlight}</span>
            </h1>
            <p class="text-md text-center">{$landingPageSettings.contact.subtitle}</p>
          </div>
          <Grid class="max-w-[700px] pb-10">
            <!-- Contact Details -->
            <Row>
              <Column
                class="mx-2 flex cursor-pointer flex-col items-center justify-center break-all rounded-lg py-2 text-center transition-all duration-500 hover:shadow-xl"
              >
                <LocationFilled size={32} />
                <p class="mt-3 max-w-[200px] text-xs md:text-sm">
                  {$landingPageSettings.contact.address}
                </p>
              </Column>
              <Column
                class="mx-2 flex cursor-pointer flex-col items-center justify-center break-all rounded-lg py-2 text-center transition-all duration-500 hover:shadow-xl"
              >
                <Phone size={32} />
                <p class="mt-3 text-xs md:text-sm">{$landingPageSettings.contact.phone}</p>
              </Column>
              <Column
                class="mx-2 flex cursor-pointer flex-col items-center justify-center break-all rounded-lg py-2 text-center transition-all duration-500 hover:shadow-xl"
              >
                <Email size={32} />
                <p class="mt-3 text-xs md:text-sm">{$landingPageSettings.contact.email}</p>
              </Column>
            </Row>

            <!-- Contact Form -->
            <div class="mt-8 rounded-lg bg-white p-7">
              {#if successContactSaved}
                <div class="flex w-full items-center justify-center">
                  Thank you for dropping a message, we will get back to you shortly
                </div>
              {:else}
                <form on:submit|preventDefault={handleContactSubmit}>
                  <div class="flex w-full flex-col justify-between md:flex-row">
                    <div class="mr-5 w-full md:w-2/4">
                      <TextField
                        label="Your Name"
                        bind:value={contact.name}
                        errorMessage={contactError.name}
                        className="mb-5"
                        labelClassName="font-bold"
                        placeholder="Elon Musk"
                      />
                      <TextField
                        label="Your Email"
                        bind:value={contact.email}
                        errorMessage={contactError.email}
                        className="text-xs font-normal mb-5"
                        placeholder="musk@x.com"
                      />
                      <TextField
                        label="Your Phone"
                        bind:value={contact.phone}
                        errorMessage={contactError.phone}
                        className="text-xs font-normal mb-5"
                        placeholder="+1194802480"
                      />
                    </div>
                    <div class="w-full md:w-2/4">
                      <TextArea
                        label="Your Message"
                        bind:value={contact.message}
                        errorMessage={contactError.message}
                        rows="9"
                        maxRows={15}
                        placeholder="Your message here"
                      />
                    </div>
                  </div>

                  <PrimaryButton
                    className="w-full mx-auto mt-5 md:mt-0"
                    type="submit"
                    isLoading={isContactSubmiting}
                  >
                    <span class="text-md mr-2">Submit</span>
                    <Rocket size={24} />
                  </PrimaryButton>
                </form>
              {/if}
            </div>
          </Grid>
        </div>
      </section>
    {/if}

    <!-- Waitlist Section -->
    {#if $landingPageSettings.mailinglist.show}
      <section id="waitlist" transition:fade class="mx-auto my-10 w-[95%] max-w-6xl">
        <div
          class="bg-primary-700 flex flex-col rounded-lg px-4 py-14 md:px-10 lg:flex-row lg:items-center"
        >
          <div class="w-full md:mr-4 md:w-[65%]">
            <h1 class="mb-5 mt-0 text-4xl font-bold text-white">
              {$landingPageSettings.mailinglist.title}
            </h1>
            <p class="text-lg text-white">
              {$landingPageSettings.mailinglist.subtitle}
            </p>
          </div>
          <form on:submit|preventDefault={handleSubmit} class="my-4 w-full md:w-fit">
            <div class="flex flex-col items-center sm:flex-row">
              {#if success}
                <p class="text-white">You have been added successfully. Thanks for subscribing.</p>
              {:else}
                <TextField
                  bind:value={email}
                  type="email"
                  placeholder="Enter your email.."
                  className="sm:mr-3 my-0 w-full md:w-fit"
                  isRequired={true}
                  isDisabled={isAdding}
                  inputClassName="py-2"
                />
                <PrimaryButton
                  className="my-1 w-full mt-2"
                  variant={VARIANTS.CONTAINED_LIGHT}
                  type="submit"
                  isLoading={isAdding}>{$landingPageSettings.mailinglist.buttonLabel}</PrimaryButton
                >
              {/if}
            </div>
          </form>
        </div>
      </section>
    {/if}

    <!-- Footer Section -->

    {#if $landingPageSettings.footer.show}
      <footer
        id="footer"
        class="mt-10 flex w-full flex-col justify-center border-b-0 border-l-0 border-r-0 border-t border-gray-300 px-5 py-10 md:py-3"
      >
        <ul class="flex w-11/12 flex-col items-center sm:flex-row">
          <div class="logo">
            <a
              href="/"
              title={`Go to ${org.name} Home`}
              id="logo"
              data-hveid="8"
              class="flex items-center"
            >
              <img
                src={org.avatar_url ||
                  'https://www.coherence-research.com/coherence_research_logo.jpeg'}
                alt={`${org.name} logo`}
                class="mx-auto inline-block h-10 w-10 rounded"
                data-atf="1"
              />
              <h3 class="ml-3 text-xl text-black">{org.name}</h3>
            </a>
          </div>

          <span class="flex-grow" />

          <div class="mt-5 flex sm:mt-0">
            {#if $landingPageSettings.footer.facebook}
              <li class="mx-2">
                <a
                  href={$landingPageSettings.footer.facebook}
                  target="_blank"
                  title="Facebook"
                  id="logo-fb"
                  data-hveid="8"
                >
                  <LogoFacebook size={24} />
                </a>
              </li>
            {/if}
            {#if $landingPageSettings.footer.twitter}
              <li class="mx-2">
                <a
                  href={$landingPageSettings.footer.twitter}
                  target="_blank"
                  title="Twitter"
                  id="logo-tw"
                  data-hveid="8"
                >
                  <LogoTwitter size={24} />
                </a>
              </li>
            {/if}

            {#if $landingPageSettings.footer.linkedin}
              <li class="mx-2">
                <a
                  href={$landingPageSettings.footer.linkedin}
                  target="_blank"
                  title="Linkedin"
                  id="logo-ln"
                  data-hveid="8"
                >
                  <LogoLinkedin size={24} />
                </a>
              </li>
            {/if}
          </div>
        </ul>
        <p class="mt-5 text-center">
          Powered by
          <a class="text-primary-600 underline" href="https://classroomio.com" target="_blank"
            >ClassroomIO</a
          >
        </p>
      </footer>
    {/if}
  </main>
{/if}

<style>
  .banner {
    background: url('/images/org-landingpage-banner.jpeg') no-repeat center center fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
  }
</style>

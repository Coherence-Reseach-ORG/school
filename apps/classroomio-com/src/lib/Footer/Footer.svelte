<script>
  import LogoYoutube from 'carbon-icons-svelte/lib/LogoYoutube.svelte';
  import LogoFacebook from 'carbon-icons-svelte/lib/LogoFacebook.svelte';
</script>

<div class="border-t-[1px] px-[2%] lg:px-[14%]">
  <div class="flex flex-col items-center justify-between py-[10%] pl-5 lg:flex-row lg:pl-0">
    <div class="w-full lg:w-2/4">
      <img
        src="https://www.coherence-research.com/coherence_research_logo.jpeg"
        alt="classroomio logo"
        class="mb-5 w-[10%] lg:mb-10 lg:w-[5%]"
      />
      <p class="lg:text-md mb-5 mr-2 text-sm leading-7 tracking-wide text-gray-600 lg:leading-7">
        Launch Your Online Bootcamp In Minutes
        <br />
        Making teaching and learning seamless for tech bootcamps
      </p>
      <a
        href="https://www.producthunt.com/posts/classroomio?utm_source=badge-featured&utm_medium=badge&utm_souce=badge-classroomio"
        target="_blank"
        ><img
          src="https://api.producthunt.com/widgets/embed-image/v1/featured.svg?post_id=425236&theme=light"
          alt="ClassroomIO - The&#0032;Open&#0032;Source&#0032;Platform&#0032;for&#0032;Tech&#0032;Bootcamps | Product Hunt"
          style="width: 250px; height: 54px;"
          width="250"
          height="54"
        /></a
      >
      <div class="mt-5 flex items-center gap-3 lg:mt-10 lg:gap-7">
        <a href="https://www.facebook.com/classroomiohq" target="_blank">
          <LogoFacebook
            class="scale-90 cursor-pointer fill-gray-400 transition-all hover:scale-110 hover:fill-gray-500 lg:scale-150"
            size={20}
          />
        </a>
        <a href="https://twitter.com/classroomio" target="_blank">
          <img
            src="/twitter_logo.png"
            alt=""
            class="w-3 scale-90 cursor-pointer transition-all hover:scale-110 hover:fill-gray-500 lg:scale-150"
          />
        </a>
        <a href="https://www.youtube.com/@ClassroomIO" target="_blank">
          <LogoYoutube
            class="scale-90 cursor-pointer fill-gray-400 transition-all hover:scale-110 hover:fill-gray-500 lg:scale-150"
            size={20}
          />
        </a>
      </div>
    </div>

    <nav
      class="mt-16 flex w-full flex-col items-start justify-between gap-3 space-y-6 lg:mt-0 lg:w-2/4 lg:flex-row lg:space-y-0"
    >
      <div>
        <h3 class="lg:text-md mb-3 text-base font-semibold leading-6 text-gray-900 lg:mb-10">
          Product
        </h3>
        <ul class="space-y-2 lg:space-y-4">
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a href="/#coursemanagement"> Course Management </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a href="/#customization"> Customizations </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a href="/#collaboration"> Forum </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a href="/#ai"> AI Lesson Planner </a>
          </li>
        </ul>
      </div>

      <div>
        <h3 class="lg:text-md mb-3 text-base font-semibold leading-6 text-gray-900 lg:mb-10">
          Resources
        </h3>
        <ul class="space-y-2 lg:space-y-4">
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a href="/blog"> Blog </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a target="_blank" href="https://classroomio.com/docs/"> Help center </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a target="_blank" href="https://classroomio.com/docs/what-is-classroomio">
              What is ClassroomIO?
            </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a target="_blank" href="https://classroomio.com/docs/quickstart/signup">
              Getting Started
            </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a href="/oss-friends"> OSS Friends </a>
          </li>
        </ul>
      </div>

      <div>
        <h3 class="lg:text-md mb-3 text-base font-semibold leading-6 text-gray-900 lg:mb-10">
          Company
        </h3>
        <ul class="space-y-2 lg:space-y-4">
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a href="mailto:help@classroomio.com" target="_blank"> Contact us </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a
              href="https://app.enzuzo.com/policies/privacy/958fc978-5477-11ee-a03b-7b111830c594"
              target="_blank"
            >
              Privacy policy
            </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a target="_blank" href="https://classroomio.com/docs/quickstart/signup">
              Getting Started
            </a>
          </li>
          <li class="lg:text-md text-sm leading-6 tracking-wide text-gray-600 hover:text-gray-900">
            <a
              href="https://app.enzuzo.com/policies/tos/958fc978-5477-11ee-a03b-7b111830c594"
              target="_blank"
            >
              Terms of use
            </a>
          </li>
        </ul>
      </div>
    </nav>
  </div>
  <footer
    class="lg:text-md border-t-[1px] py-[3%] text-center text-xs font-light leading-5 text-gray-500 lg:text-left"
  >
    <p>All rights reserved.</p>
    <p class="flex items-center">
      ClassroomIO © {new Date().getFullYear()} by
      <a href="https://twitter.com/rotimi_best" target="_blank" class="mx-1 text-blue-700 underline"
        >Rotimi Best</a
      >
      from
      <img alt="Nigerian Flag" src="/ng.png" class="ml-2 h-3 w-3" />
    </p>
  </footer>
</div>

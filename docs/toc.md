<!-- toc-page -->
<style>
  /* Hide the ToC on its own standalone site page (screen), but keep it
     visible inside the combined print page (#print-site-page) so it can
     render into the PDF. Never touches .md-main__inner or the site chrome. */
  @media screen {
    .toc-page { display: none; }
    #print-site-page .toc-page { display: block; }
  }
  /* Keep the ToC's own group headers flowing on one page in print,
     without re-paginating h2 headings anywhere else in the document. */
  @media print {
    .toc-page h2 { page-break-before: auto !important; }
  }
</style>

<div class="toc-page" markdown>

# **Table of Contents**

## [Introduction](index.md)

- [1. GitHub is the connective tissue of modern software delivery](index.md#i-github-is-the-connective-tissue-of-modern-software-delivery)
- [2. What is IBM Bob?](index.md#ii-what-is-ibm-bob)
- [3. The environment](index.md#iii-the-environment)
- [4. Objectives and outline](index.md#iv-objectives-and-outline)
- [5. Next steps](index.md#v-next-steps)

## [Setup and Registration](setup.md)
- [1. Before you begin](setup.md#i-before-you-begin)
- [2. Choosing how to access Bob](setup.md#ii-choosing-how-to-access-bob)
- [3. IBMers: Internal registration](setup.md#iii-ibmers-internal-registration)
- [4. Business Partners: Trial registration](setup.md#iv-business-partners-trial-registration)
- [5. Download the lab assets](setup.md#v-download-the-lab-assets)
- [6. Next steps](setup.md#vi-next-steps)

## [Planning and Scaffolding the Application](build-1.md)
- [1. Why this matters](build-1.md#i-why-this-matters)
- [2. Planning and scaffolding the application](build-1.md#ii-planning-and-scaffolding-the-application)
- [3. Reviewing the generated scaffold](build-1.md#iii-reviewing-the-generated-scaffold)
- [4. Key takeaways](build-1.md#iv-key-takeaways)

## [Building and Validating the Dashboards](build-2.md)
- [1. Why this matters](build-2.md#i-why-this-matters)
- [2. Building and validating the dashboards](build-2.md#ii-building-and-validating-the-dashboards)
- [3. Reviewing the completed build](build-2.md#iii-reviewing-the-completed-build)
- [4. Key takeaways](build-2.md#iv-key-takeaways)

## [Publishing to GitHub](evolve-1.md)
- [1. Why this matters](evolve-1.md#i-why-this-matters)
- [2. Publishing to GitHub](evolve-1.md#ii-publishing-to-github)
- [3. Verification checklist](evolve-1.md#iii-verification-checklist)
- [4. Key takeaways](evolve-1.md#iv-key-takeaways)

## [Implementing a Feature Request](evolve-2.md)
- [1. Why this matters](evolve-2.md#i-why-this-matters)
- [2. Implementing a feature request](evolve-2.md#ii-implementing-a-feature-request)
- [3. Reviewing the enhancement](evolve-2.md#iii-reviewing-the-enhancement)
- [4. Key takeaways](evolve-2.md#iv-key-takeaways)

## [Preparing a Pull Request](evolve-3.md)
- [1. Why this matters](evolve-3.md#i-why-this-matters)
- [2. Preparing a pull request](evolve-3.md#ii-preparing-a-pull-request)
- [3. Complete SDLC coverage with Bob and GitHub](evolve-3.md#iii-complete-sdlc-coverage-with-bob-github)
- [4. Key takeaways](evolve-3.md#iv-key-takeaways)
- [5. Earn accreditation](evolve-3.md#v-earn-accreditation)

</div>

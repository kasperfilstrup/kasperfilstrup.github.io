---
layout: default
title: 'Om os'
---

<div class="contact-page page-padding">
  <div class="contact-page__container">
    <header class="contact-page__header">
      <h1 class="contact-page__title page__title">
        {{ page.title }}
      </h1>
      <p class="contact-page__description text-justify">
        Cafe Manna er en café, etableret af Jane som efter mange år i restaurationsbranchen, nu har realiseret sin drøm.
      </p>
      <p class="contact-page__description text-justify">      
        Inspirationen bag kommer fra min far på De Kanariske Øer og er spiret ud af kærlighed til måltidets særlige rolle. Det er min familie og nære venner, du møder i baren, og vi ønsker at være et levende mødested, hvor folk samles om fællesspisning og arrangementer sammen med og for lokalsamfundet. Det er også muligt at leje Manna Café til selskaber.</p>
      <p class="contact-page__description text-justify">
        Vi arbejder med årstidens råvarer fra lokale og så vidt muligt økologiske forhandlere. Vi laver alt fra bunden. Vi bager surdejsbrød hver morgen, laver store, smagfulde sandwich, serverer tapas og weekendbrunch.</p>
      <p class="contact-page__description text-justify">
        Drikkevarerne er en naturlig forlængelse af madoplevelsen hos os. Vi har et nøje udvalgt sortiment af kaffe, vine og øl, som vi skænker, når stemningen indbyder til det, og særligt når solen titter frem under elmene.</p>
      <p class="contact-page__description text-justify">
        På Under Elmene 13 har Manna slået rod. Navnet udspringer af vores adresse og de små, vingede elmefrø, manna, der stille spreder liv omkring sig. For os symboliserer det, at vi har blik for det, der gror tæt på - mennesker, smage og fællesskab i vores eget nabolag.</p>
      <p class="contact-page__description text-justify">
        <img src="{{ '/assets/images/jane.jpg' | relative_url }}" alt="{{ site.title }} - Jane" width="450" height="541" fetchpriority="high" style="border: 2px solid var(--primary-600)" />
      </p>
    </header>
    <div class="contact-page__grid">
      <section>
        <h2 class="contact-page__section-title">
          Vores Lokation
        </h2>
        <div class="contact-page__info-list">
          <div>
            <h3 class="contact-page__info-item-label">
              Adresse
            </h3>
            <p class="contact-page__info-item-value">
              <a href="https://www.google.com/maps/search/{{ site.address | uri_escape }}" target="_blank" rel="noopener">
              {{ site.address }}
              </a>
            </p>
          </div>
          <div>
            <h3 class="contact-page__info-item-label">
              Telefon
            </h3>
            <p class="contact-page__info-item-value">{{ site.phone }}</p>
          </div>
          <div>
            <h3 class="contact-page__info-item-label">
              Email
            </h3>
            <p class="contact-page__info-item-value">
              {{ site.email }}
            </p>
          </div>
        </div>
      </section>
      <section>
        <h2 class="contact-page__section-title">
          Følg Os
        </h2>
        <div class="contact-page__social">
          <a href="#">
            <div class="icon-wrapper">
              <svg
                fill="currentColor"
                viewBox="0 0 256 256"
                xmlns="http://www.w3.org/2000/svg">
                <path d="M128,24A104,104,0,1,0,232,128,104.11,104.11,0,0,0,128,24Zm8,191.63V152h24a8,8,0,0,0,0-16H136V112a16,16,0,0,1,16-16h16a8,8,0,0,0,0-16H152a32,32,0,0,0-32,32v24H96a8,8,0,0,0,0,16h24v63.63a88,88,0,1,1,16,0Z"></path>
              </svg>
            </div>
            <p>Facebook</p>
          </a>
          <a href="#">
            <div class="icon-wrapper">
              <svg
                fill="currentColor"
                viewBox="0 0 256 256"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M128,80a48,48,0,1,0,48,48A48.05,48.05,0,0,0,128,80Zm0,80a32,32,0,1,1,32-32A32,32,0,0,1,128,160ZM176,24H80A56.06,56.06,0,0,0,24,80v96a56.06,56.06,0,0,0,56,56h96a56.06,56.06,0,0,0,56-56V80A56.06,56.06,0,0,0,176,24Zm40,152a40,40,0,0,1-40,40H80a40,40,0,0,1-40-40V80A40,40,0,0,1,80,40h96a40,40,0,0,1,40,40ZM192,76a12,12,0,1,1-12-12A12,12,0,0,1,192,76Z"
                ></path>
              </svg>
            </div>
            <p>Instagram</p>
          </a>
        </div>
      </section>
    </div>
    <section>
      <div class="contact-page__map">
        <a href="https://www.google.com/maps/search/{{ site.address | uri_escape }}" target="_blank" rel="noopener">
          <picture>
            <source media="(min-width: 640px)" srcset="{{ '/assets/images/cafe-manna-kort.webp' | relative_url }}">
            <img src="{{ '/assets/images/cafe-manna-kort--small.webp' | relative_url }}" alt="{{ site.title }}" width="460" height="261" fetchpriority="high" />
          </picture>
        </a>
      </div>
    </section>
  </div>
</div>

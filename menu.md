---
layout: default
title: 'Vores Menu'
description: 'Friskbrygget kaffe, lækkert bagværk og velsmagende sandwiches.'
---

<div class="menu-page page-padding">
  <div class="menu-page__container">
    <div class="menu-page__header">
      <h1 class="menu-page__title page__title">
        {{ page.title }}
      </h1>
      <p class="menu-page__description page__description">
        {{ page.description }}
      </p>
    </div>

    {% for section in site.data.new_menu.sections %}
      <section class="menu-page__section">
        <h2 class="menu-page__section-title">{{ section.title }}</h2>
        {% if section.description %}
          <p class="menu-page__section-description">{{ section.description }}</p>
        {% endif %}

        {% if section.items %}
          <div class="menu-page__items">
            {% for item in section.items %}
              <div class="menu-page__item">
                <div>
                  <span class="menu-page__item-name">{{ item.name }}</span>
                  {% if item.description != '' %}<p class="menu-page__item-description">{{ item.description }}</p>{% endif %}
                </div>
                <span class="menu-page__item-price">{{ item.price }}</span>
              </div>
            {% endfor %}
          </div>
        {% endif %}

        {% if section.subcategories %}
          {% for sub in section.subcategories %}
            <div class="menu-page__subcategory">
              <h3 class="menu-page__subcategory-title">{{ sub.title }}</h3>
              {% if sub.description %}
                <p class="menu-page__section-description">{{ sub.description }}</p>
              {% endif %}

              {% if sub.items %}
                <div class="menu-page__items">
                  {% for item in sub.items %}
                    <div class="menu-page__item">
                      <div>
                        <span class="menu-page__item-name">{{ item.name }}</span>
                        {% if item.description != '' %}<p class="menu-page__item-description">{{ item.description }}</p>{% endif %}
                      </div>
                      <span class="menu-page__item-price">{{ item.price }}</span>
                    </div>
                  {% endfor %}
                </div>
              {% endif %}

              {% if sub.subcategories %}
                {% for subsub in sub.subcategories %}
                  <div class="menu-page__subcategory">
                    <h4 class="menu-page__subcategory-title">{{ subsub.title }}</h4>
                    {% if subsub.description %}
                      <p class="menu-page__section-description">{{ subsub.description }}</p>
                    {% endif %}

                    <div class="menu-page__items">
                      {% for item in subsub.items %}
                        <div class="menu-page__item">
                          <div>
                            <span class="menu-page__item-name">{{ item.name }}</span>
                            {% if item.description != '' %}<p class="menu-page__item-description">{{ item.description }}</p>{% endif %}
                          </div>
                          <span class="menu-page__item-price">{{ item.price }}</span>
                        </div>
                      {% endfor %}
                    </div>
                  </div>
                {% endfor %}
              {% endif %}
            </div>
          {% endfor %}
        {% endif %}
      </section>
    {% endfor %}

    Hos Manna Cafe bruger vi så vidt muligt økologiske råvarer og gerne fra lokale producenter.

  </div>
</div>

---
layout: default
title: 'Menu'
description: 'Friskbrygget kaffe, surdejsbrød, sandwiches, tapas, vin og øl.'
logo_color: '#9f1b27'
page_bg: '#e7e7e7'
---

<div class="menu-page">
  <div class="menu-page__grid">
    {% for section in site.data.new_menu.sections %}
      <section class="menu-page__section">
        <h2 class="menu-page__section-title">{{ section.title }}</h2>
        {% if section.description %}
          <p class="menu-page__section-description">{{ section.description }}</p>
        {% endif %}

        <ul class="menu-page__items">
          {% for item in section.items %}
            <li class="menu-page__item">
              <div class="menu-page__item-head">
                <span class="menu-page__item-name">{{ item.name }}
                  {% if item.name_extra != '' and item.name_extra != nil %}<span class="menu-page__item-name-extra">{{ item.name_extra }}</span>{% endif %}
                </span>
                <span class="menu-page__item-price">{{ item.price }}</span>
              </div>
              {% if item.description != '' and item.description != nil %}
                <p class="menu-page__item-description">{{ item.description }}</p>
              {% endif %}
            </li>
          {% endfor %}
        </ul>
      </section>
    {% endfor %}
  </div>
</div>
